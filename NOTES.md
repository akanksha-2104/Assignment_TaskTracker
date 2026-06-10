

## Overview

I focused on fixing the highest-impact bugs affecting correctness, performance, and user experience while keeping the changes minimal and maintainable.

---

## 1. SQL Operator Precedence Bug

**Issue:** The search query mixed `AND` and `OR` without parentheses, causing incorrect filtering.

**Fix:** Wrapped the title/description conditions:

```sql
AND (
    LOWER(title) LIKE :term
    OR LOWER(description) LIKE :term
)
```

**Result:** Archived and status filters now apply consistently to both title and description matches.

---

## 2. Artificial Request Delay

**Issue:** The controller intentionally delayed requests using `Thread.sleep()`, with shorter queries incurring longer delays.

**Fix:** Removed the artificial sleep logic.

**Result:** Faster response times and improved backend performance.

---

## 3. Loading State on API Errors

**Issue:** When an API request failed, the loading state was never cleared.

**Fix:** Added `setLoading(false)` in the error path and guarded against stale async updates.

**Result:** Users see the error message instead of an infinite loading screen.

---

## 4. Pagination Reset on Filter Changes

**Issue:** Changing the search query or status filter while on later pages could display empty results.

**Fix:** Reset the page to `1` whenever filters change and added a 300ms search debounce.

**Result:** Better UX and fewer unnecessary API requests.

---

## Additional Improvement

* Added search debounce to reduce backend load.
* Improved async state handling to avoid outdated responses updating the UI.

---

## Remaining Limitation

The backend loads all matching records and performs pagination in memory. For large datasets, database-level pagination (`LIMIT/OFFSET` with a `COUNT(*)` query) would be a better long-term solution.

---

## Approach

I prioritized:

1. Correctness of results.
2. User experience.
3. Performance improvements.
4. Small, maintainable changes with minimal impact on the existing codebase.
