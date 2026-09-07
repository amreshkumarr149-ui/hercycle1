# Security Spec

## Data Invariants
1. A user profile document (`users/{userId}`) can only be read, created, or updated by the authenticated user whose UID matches the `userId`.
2. A user's profile must include a valid `goal`, and strict timestamps (`createdAt`, `updatedAt`).
3. A user's daily logs (`users/{userId}/dailyLogs/{dateId}`) can only be read, created, updated, or deleted by the user whose UID matches `userId`.
4. The `dateId` must strictly follow a date-like regex (e.g. `YYYY-MM-DD` or alphanumeric `^[a-zA-Z0-9_\-]+$`) to prevent path poisoning.
5. `symptoms` arrays must be bounded (max 20 items), and each item must be a short string.

## The "Dirty Dozen" Payloads
1. User Profile Create (Missing `goal`) - PERMISSION_DENIED
2. User Profile Create (Invalid timestamp) - PERMISSION_DENIED
3. User Profile Update (Spoofing `userId` in path) - PERMISSION_DENIED
4. User Profile Update (Modifying `createdAt`) - PERMISSION_DENIED
5. Daily Log Create (Invalid `dateId` poisoning) - PERMISSION_DENIED
6. Daily Log Create (Spoofing another user's `userId`) - PERMISSION_DENIED
7. Daily Log Update (Modifying non-allowed fields) - PERMISSION_DENIED
8. Daily Log Update (Invalid symptoms array size > 20) - PERMISSION_DENIED
9. Daily Log Update (Invalid symptom string type) - PERMISSION_DENIED
10. Daily Log Read (User trying to read another user's daily log) - PERMISSION_DENIED
11. Blanket Read (Unauthenticated user reading logs) - PERMISSION_DENIED
12. Denial of Wallet (1MB string payload in mood) - PERMISSION_DENIED
