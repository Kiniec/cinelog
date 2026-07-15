## Project 6: CineLog

### Total Points: 25pts + 3pts bonus
### Required Features:
- 2pts 	Function Rename (Comment 1)
	- 1 	Commit history includes a commit describing the rename of save_to_watchlist() to add_to_watchlist().
	- 1 	PR Response Doc entry for Comment 1 describes where call sites were found and how the student confirmed none were missed — not just "I renamed it."
- 3pts 	Deduplication (Comment 2)
	- 1 	Commit history includes a commit describing the deduplication addition.
	- 1 	PR Response Doc explains what the deduplication logic does — a description of the check and what happens when a duplicate is detected.
	- 1 	PR Response Doc references the existing add_to_collection() pattern or otherwise shows the student looked at how the codebase handles this case before implementing their own version.
- 3pts 	Missing Test (Comment 3)
	- 1 	Commit history includes a test: commit describing the new test.
	- 1 	PR Response Doc entry describes what the test checks and which existing test it was modeled after.
	- 1 	The described test targets the right case — a nonexistent film_id — rather than a different edge case.
- 3pts 	Default Visibility Reasoning (Comment 4)
	- 1 	The response takes a clear position — public, private, or a reasoned alternative — rather than hedging or deferring.
	- 1 	The response explains what user behavior or platform value the chosen default optimizes for. Generic statements ("public is better for discovery") earn partial at best; a specific argument about CineLog's user context earns full.
	- 1 	The response acknowledges the tradeoff — what the other option would optimize for and why the student's choice is preferable in this context.
- 4pts 	Sort Order Decision (Comment 5)
	- 1 	The response takes a clear position — date-added, alphabetical, or a reasoned third option. A well-argued case for keeping alphabetical is worth full credit.
	- 1 	The response explains the user behavior the chosen order optimizes for. Specific argument about how CineLog users interact with watchlists earns full.
	- 1 	The response directly engages with the maintainer's point ("Most users want to see what they added recently") — either agreeing with evidence, disagreeing with a counter-argument, or proposing a synthesis.
	- 1 	The response is substantive enough that a maintainer could approve or push back on it — not a one-liner, and not passive.
- 3pts 	Rebase and Conflict Resolution (Comment 6)
	- 1 	Screenshot or commit history shows no "Merge branch" commits — the history is linear.
	-  1 	Commit history includes a fix: commit referencing the UUID update (e.g., fix: update film IDs to UUID format after main refactor).
	- 1 	PR Response Doc entry describes what conflicted (integer vs. UUID film IDs) and what changes were made to resolve it.
- 3pts 	Commit History
	- 1 	Screenshot or commit history shows all commits using conventional commit format (feat:, fix:, test:, docs:).
	- 1 	Each commit message describes one logical change with a clear, imperative summary — no "fixed stuff," "more changes," or bundled commits.
	- 1 	The history shows at least 4 separate commits reflecting the distinct changes made (rename, dedup fix, test, rebase update, etc.).
- 4pts 	PR Description
	- 1 	PR description explains what the watchlist feature does in plain language — a maintainer who hasn't seen the code can understand it.
	- 1 	Both design decisions are explicitly named: the chosen default visibility and the chosen sort order, with a one-sentence summary of each rationale.
	- 1 	Testing instructions are specific enough to follow — lists the steps a reviewer would take to manually confirm the feature works, not just "test the endpoint."
	- 1 	The PR Response Doc includes an AI Usage section documenting at least one specific use of AI tools during the project, OR a brief note that AI was not used.

Stretch Features
- +1pt 	Add remove_from_watchlist() \
	PR Response Doc describes the implementation, including what it does when the film isn't on the watchlist and how it follows the project's existing patterns. Commit history includes a feat: or fix: commit for the function. PR Response Doc mentions a test was written for it.
- +1pt 	Second Test\
	Commit history includes an additional test: commit beyond the one required for Comment 3. PR Response Doc explains what edge case the test covers and why the student chose that case.
- +1pt 	Visibility Toggle Endpoint \
	Commit history includes a feat: commit describing the visibility toggle addition. PR Response Doc describes how the public parameter works, what the default is, and how a caller would use it.

