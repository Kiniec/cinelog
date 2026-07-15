## AI Usage 
<!-- Fill in at the end - how AI tools were used during the project -->
Utilized Claude Code in understanding and summarizing the logic for add_to_collection(), collection of services(), models.py, and test_collections.py \
Utilized Claude Code for counter argument for comment 4 and 5. Received the suggestion to change the default to False in models.py and change from alphabetical order to date first in get_watchlist() function in watchlist_service.py
## Commit 1 - Rename 
**What I did:**

Searched services folder and navigated to the file watchlist_services.py. From there, found the function save_to_watchlist() and changed to add_to_watchlist(). Also changed all occurrences of the function save_to_watchlist, particular in the routes folder within the file watchlist.py. 

**How I verified:**

Utilized the editor's find-all-references to ensure all changes were made the function save_to_watchlist to add_to_watchlist.

## Comment 2 - Deduplication
**What I did:**

Navigated to the watchlist_service.py with in the services folder. Inside the add_to_watchlist(), deduplication logic was created to ensure a watch list was not duplicated. The newly added error, AlreadyInWatchListError, would be raised if a film was added to the list and it already existed.


**How I verified**

The logic was verified by test_watchlist.py in the folder tests. Test ran: 
> pytest tests/test_watchlist.py -v

## Comment 3 - Missing test
**What I did**
 Navigated to services and reviewed collection_service.py. Created a test for test_watchlist.py and used test_collection.py as a model for the test. 

**How I verified**

The logic was verified by test_watchlist.py in the folder tests. Test ran: 
> pytest tests/test_watchlist.py -v

## Comment 4 - Default visibility 

**My position**

My position is public should be set to False. 

**Reasoning**

Although a user's watch list being set as public=True, allows for sharing of film ratings for other users, it does present a risks factor. Often a community receive suggestions from within, such as Reddit, Facebook, etc. This optimize the approach of sharing allowing the system to grow and be utilized by newbies and film buffs alike. This only works it the user can decide to share and given the choice to do so. 


**Tradeoff acknowledged**

The trade off of course would be keeping watchlists set to public=True. Although not an opposite of public=False, the trade-off would be to provide users with no say in how collections, ratings, and watchlist is shared among the community. By doing so, would possible divide the community and would stifle the growth of Cinelog. Users participate in the film tracking app to share movies viewed, rated, and collected with some form of user control. 

## Comment 5 - Sort order 
**My position:**

I whole hearty agree with your preference for watchlists to be defaulted to date added vs adding films alphabetically. 

**Reasoning:**

Accessing the most recent data is often easier than recall. A user may not remember the last film seen or the title of the film. If a film rating was low, the user may not recall it was low and spend time searching for it, only to see the film had a low rating. 


**Engagement with reviewer's point:**
 
 From my experience, most users, on any platform, like to be able to access the most recent items first. Anything added recently would want to be seen by the user. Therefore I do agree. 


## Comment 6 - Rebase 
**What conflicted:**

Reference to integer IDs and not UUID in watchlist code. 


**How I resolved it:**

Resolved the conflict by updating watchlist code using UUIDs where it still referenced integer IDs.



**How I verified no conflict remains:**
 
 Verified no conflict remains by reviewing git status.

## Stretch - remove_from_watchlist()

**What I did:**

Added `remove_from_watchlist(user_id, film_id)` to `services/watchlist_service.py`, following the same pattern as `remove_from_collection()` in `collection_service.py`: look up the matching `WatchlistEntry` for the (user_id, film_id) pair, and if none exists, raise a new `NotInWatchlistError` instead of silently doing nothing. If found, delete and commit, returning `True`. Also added a `DELETE /watchlist/<user_id>/remove` route in `routes/watchlist/watchlist.py` mirroring the existing `DELETE /collection/<user_id>/remove` route, including catching `NotInWatchlistError` and returning a 404.

**How I verified:**

Added two tests to `tests/test_watchlist.py`: `test_remove_from_watchlist_removes_entry` (confirms the entry is deleted and the count drops to 0) and `test_remove_from_watchlist_not_in_watchlist_raises` (confirms `NotInWatchlistError` is raised, rather than a silent no-op, when the film isn't on the watchlist). Ran:
> pytest tests/test_watchlist.py -v

All tests passed (8/8 across the full suite).

## PR Description 

<!-- Written at the end — feature overview, design decisions, manual testing steps -->

Adds a watchlist feature: users can save films to watch later, view their watchlist, and remove films from it, separate from their existing "watched" collection. Duplicate adds are rejected rather than silently duplicated.

**Design decisions:** Watchlist entries default to `public=False` so a user's saved films aren't shared until they opt in. `get_watchlist()` returns entries newest-added first, since users are more likely to act on what they just saved than scan alphabetically.

**Manual testing:** Start the app (`python app.py`), create a user/film via `flask shell` (no create endpoint exists yet), then: `POST /watchlist/<user_id>/add` with `{"film_id": ...}` (expect 201; repeat to confirm a 409 on duplicate) → `GET /watchlist/<user_id>` (confirm newest-first, `public: false`) → `DELETE /watchlist/<user_id>/remove` with the same body (expect 200; repeat to confirm a 404). Also run `pytest tests/ -v` (8/8 passing).

![git log](git_log.png)