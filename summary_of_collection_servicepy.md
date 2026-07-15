Business logic layer for a user's "already watched" film collection, sitting between routes and the ORM models.

Three custom exceptions: FilmNotFoundError, AlreadyInCollectionError, NotInCollectionError — used to signal domain errors up to the route layer rather than letting None/DB errors leak through.

Three functions:
- add_to_collection(user_id, film_id, rating=None) (collection_service.py:27) — looks up the Film via db.session.get, raises FilmNotFoundError if missing, checks for an existing CollectionEntry for that (user, film) pair and raises AlreadyInCollectionError if found, otherwise creates and commits a new CollectionEntry.
- remove_from_collection(user_id, film_id) (collection_service.py:61) — finds the matching CollectionEntry, raises NotInCollectionError if none exists, else deletes and commits.
- get_collection(user_id) (collection_service.py:88) — queries all CollectionEntry rows for the user ordered by date_added descending, and returns a list of film dicts (via entry.film.to_dict(), using the film backref from models.py) each annotated with date_added and rating from the entry.

Depends on:
- app.db — the shared SQLAlchemy session/engine.
- models.Film and models.CollectionEntry — see the earlier summary of models.py. Note the docstrings here say film_id is a UUID string, but models.py:29-30 currently defines Film.id as an auto-increment Integer (the pre-migration state) — this file's docstrings appear to already assume the post-migration UUID schema, so there's a latent type mismatch worth resolving before/at merge.
- Presumably consumed by a routes/collection.py-style route module (not shown here) that catches the three custom exceptions and maps them to HTTP responses.