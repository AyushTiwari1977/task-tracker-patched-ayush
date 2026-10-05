# Patch Notes

## Summary of changes
- Fixed task search SQL operator precedence so `archived = FALSE`, the title/description search, and the optional status filter are all applied together.
- Removed artificial request latency from `TaskController`. The previous code slept for up to 1 second on every request, making normal searches unnecessarily slow.
- Added validation for `page`, `pageSize`, and invalid status values so malformed requests return HTTP 400 instead of producing incorrect results or server errors.
- Added request cancellation/error reset in the React task hook to prevent stale responses from overwriting newer searches.
- Reset pagination when search/status filters change so a filter cannot leave the UI on an invalid/out-of-range page.
- Kept the reference H2/Oracle SQL artifacts consistent with the corrected predicate logic.

## What I chose not to change
I did not rewrite pagination to database-level `Pageable` queries, add a debounce, or redesign the UI. Those are worthwhile improvements, but the exercise is timeboxed and the current functional correctness issues were higher priority.

## Biggest remaining risk
The backend still loads every matching task into memory and then paginates the Java list. This will become inefficient as the task table grows. Database-level pagination and a count query would be the next improvement.

## Tools/AI used
I used ChatGPT to inspect the codebase, identify likely defects, reason about SQL operator precedence and async React behavior, and review the patch. I inspected the changes manually and kept the implementation intentionally small rather than accepting a broad rewrite.
