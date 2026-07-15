Defines the SQLAlchemy ORM schema for CineLog. Four models:

- User (models.py:17) — id is a UUID string (via generate_uuid()), username/email unique. Has a collection_entries relationship/backref to CollectionEntry, but no relationship to WatchlistEntry — watchlist rows must be queried directly rather than via user.watchlist_entries.
- Film (models.py:29) — id is an auto-increment integer, not a UUID. The module docstring flags this explicitly: this file reflects branch state before a main-branch refactor that migrates film IDs to UUID, so expect a merge conflict/migration need here.
- CollectionEntry (models.py:52) — join table for "already watched" films; user_id/film_id FKs, unique constraint preventing duplicate (user, film) pairs, optional 1–5 rating.
- WatchlistEntry (models.py:74) — join table for "want to watch" films; same FK shape as CollectionEntry but adds a public boolean and has no unique constraint (a user could add the same film twice), and no rating field.

All four models expose a to_dict() for JSON serialization.

Dependencies:
- Imports db from app.py (the shared SQLAlchemy instance) and uuid/datetime from stdlib.
- Consumed by services/collection_service.py, services/watchlist_service.py, routes/films.py, and tests/test_collection.py.

One thing worth flagging given the docstring's own warning: since Film.id is still Integer here while main has apparently moved to UUID, merging/rebasing this branch will likely require updating Film.id, and the film_id FK columns in both CollectionEntry and WatchlistEntry, to String(36).

> Update Film.id and the FK columns to UUID to match main