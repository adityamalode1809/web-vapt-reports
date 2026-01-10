# Profile Update Succeeds Without Proper Session State Synchronization

Category: A06:2025 – Insecure Design  
Severity: Low to Medium  
Affected Endpoint: `POST /user/profile/update`



## Description

The application allows profile updates and returns a success response;
however, the updated data is **not reflected in the active user session**.

Changes only become visible after the user logs out and logs in again,
indicating improper session state synchronization.

This behavior suggests missing business logic validation and inconsistent
state handling between backend updates and session/frontend state.



## Proof of Concept (PoC)

1. Login as a normal user.
2. Navigate to the profile page.
3. Update profile fields (e.g., first name or last name).
4. Observe the success message:  
   **“User updated successfully”**
5. Note that the updated data is not reflected immediately.
6. Logout and login again to observe the updated profile data.

![Profile Update Session Sync PoC](./screenshot/profile-update-session-sync.png)



## Impact

- Causes inconsistent application state.
- Enables repeated profile update abuse without restrictions.
- May lead to audit inconsistencies and user confusion.
- Indicates missing validation of update operations.



## Recommendation

- Ensure session state is updated immediately after profile changes.
- Validate update operations server-side before returning success responses.
- Implement idempotency and abuse prevention on profile update endpoints.
- Perform a design review of state management workflows.



