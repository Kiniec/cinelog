## AI Usage 
<!-- Fill in at the end - how AI tools were used during the project -->
used AI in to understanding the logic for add_to_collection, collection of services, models, a test_collections

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
 Navigated to services and reviewed collection_servive.py. Created a test for test_watchlist.py and used test_collection.py as a model for the test. 

**How I verified**

The logic was verified by test_watchlist.py in the folder tests. Test ran: 
> pytest tests/test_watchlist.py -v

## Comment 4 - Default visibility 
**My position**
**Reasoning**
**Tradeoff acknowledged**
## Comment 5 - Sort order 
**My position:**
**Reasoning:**
**Engagement with reviewer's point:**
 
## Comment 6 - Rebase 
**What conflicted:**
**How I resolved it:**
**How I verified no conflict remains:**


## PR Description 

<!-- Written at the end — feature overview, design decisions, manual testing steps -->
