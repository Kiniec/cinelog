Pattern in tests/test_collection.py

Every test follows the same Arrange → Act → Assert shape, built on shared pytest fixtures:

Fixtures (the "given"):
- app (test_collection.py:22) — spins up a fresh Flask app with an in-memory SQLite DB (create_all() before the test, drop_all() after), so every test starts from a clean schema with no leftover state.
- sample_user (test_collection.py:36) — creates and commits one User, returns just its id (not the object — see note below).
- sample_film (test_collection.py:46) — creates and commits one Film, returns just its id.

Test body pattern:
1. Enter with app.app_context(): — every DB operation and service call happens inside this block.
2. Arrange: create any extra rows the test needs directly via the model classes (e.g. test_get_collection_returns_newest_first adds two extra Films and two CollectionEntrys with explicit date_added timestamps to control ordering).
3. Act: call the service function under test (add_to_collection, remove_from_collection, get_collection) directly — not through HTTP/routes.
4. Assert: either
  - a return value's shape (entry.user_id == sample_user), or
  - that an exception is raised via with pytest.raises(SomeError):, or
  - a follow-up DB query to confirm persisted/absent state (e.g. CollectionEntry.query.filter_by(...).count() == 1).

Naming/organization convention:
- Test functions: test_<function>_<scenario>.
- Grouped under # ── Section ── comment banners by behavior category (basic add, deduplication, nonexistent film, sort order).
- One docstring per test stating the expected behavior in plain language, and often stating what it shouldn't do too ("not silently create a duplicate," "not a database integrity error").

What you need to write a watchlist test in this style

1. Reuse the existing app, sample_user, sample_film fixtures unchanged — they're not collection-specific.
2. Import the watchlist equivalents: WatchlistEntry from models, and whatever functions/exceptions exist in services/watchlist_service.py (worth confirming its exception names match the collection service's convention, e.g. AlreadyInWatchlistError/NotInWatchlistError/FilmNotFoundError).
3. Note WatchlistEntry has no unique constraint in models.py (unlike CollectionEntry) — so a "duplicate add" test can only pass if watchlist_service.py enforces that check in code, not at the DB level. Worth checking that file before assuming the pattern carries over 1:1.
4. Wrap each test in with app.app_context():, follow arrange/act/assert, and use pytest.raises(...) for the error-path tests.
5. Since models.py currently has Film.id as Integer but this test file already uses a UUID-shaped string ("00000000-0000-0000-0000-000000000000") for the "nonexistent film" case — that specific fixture pattern will keep working after the UUID migration lands, so it's safe to copy verbatim.


> will help to move faster on Comment 4. 