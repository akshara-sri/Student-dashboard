# Security Specification: CampusPulse Portal

## Data Invariants
1. A user document `/users/{userId}` can only be read by the owner or an admin.
2. A student can only create/update their own submissions (`studentId == request.auth.uid`).
3. An assignment can only be created or modified by faculty or admin.
4. Attendance records can only be created or updated by faculty or admin, while students have read access to their own attendance.
5. Messages can only be read and written by participants (`senderId == request.auth.uid` or `recipientId == request.auth.uid`).
6. Notifications can only be read or modified (marked as read) by the recipient.
7. Exemption requests can be created by the student for themselves, and approved/rejected by faculty or admin.
8. Master denial rule on unmatched document paths.

## "Dirty Dozen" Threat Payloads
1. Unauthorized Admin Elevation: A student updating their `/users/{userId}` with `role: "admin"`.
2. Attendance Tampering: A student forging an attendance document `/attendance/{id}` with `status: "present"`.
3. Grade Injection: A student writing directly to `/results/{id}` or setting `score: 100` on `/submissions/{id}`.
4. Cross-Student Submission Hijack: Student A updating Student B's submission file URL.
5. Private Message Snooping: User C listing messages between User A and User B.
6. Phantom Assignment Creation: Student submitting an assignment definition directly into `/assignments`.
7. Notification Spam: Unauthenticated actor posting notifications to all users.
8. Exemption Self-Approval: Student updating their exemption status to `approved`.
9. ID Path Poisoning: Injecting 2MB character string as user/course ID to exhaust memory.
10. Anonymous Exploits: Unauthenticated request attempting to read grade results.
11. Unverified Email Spoof: User asserting admin rights without email verification.
12. Terminal State Breach: Changing a graded submission's final grade after verification.
