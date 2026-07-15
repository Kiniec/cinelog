Step by step:

1. Look up the film — film = db.session.get(Film, film_id) fetches the Film row by primary key using SQLAlchemy's identity-map-aware get() (faster than a filtered query for PK lookups).
2. Check it exists — if film is None: — if no film has that ID, it immediately raises FilmNotFoundError(f"No film found with id '{film_id}'") and returns nothing — execution stops here, no CollectionEntry is created, no DB write happens.
3. Check for a duplicate — queries CollectionEntry for an existing row matching this exact (user_id, film_id) pair. If one is found, raises AlreadyInCollectionError.
4. Create the entry — builds a new CollectionEntry(user_id=user_id, film_id=film_id, rating=rating) (rating defaults to None if not passed).
5. Persist it — db.session.add(entry) stages it, db.session.commit() writes it to the database.
6. Return — returns the newly created CollectionEntry object (not a dict — the caller would call .to_dict() on it if they need JSON).

If film_id doesn't exist: the function never reaches entry creation. It raises FilmNotFoundError at step 2 — it does not return None or a falsy value, it throws. The caller (presumably a route handler) needs a try/except FilmNotFoundError to catch this and turn it into a 404-style response; if uncaught, it propagates as an unhandled exception.

One thing to note: since this raises before any db.session.add, there's no partial/dirty session state left behind on the not-found path — safe to catch and continue using the session afterward.


> use on Comment 2. 