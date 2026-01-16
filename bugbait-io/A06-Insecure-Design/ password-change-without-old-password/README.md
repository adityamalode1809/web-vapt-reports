# Password Change Allowed Without Old Password Verification

Category: A06:2025 – Insecure Design  
Severity: High  
Affected Endpoint: `POST /user/profile/update-password`



## Description

The application allows users to change their account password **without
validating the current (old) password**.

By leaving the old password field empty and submitting a new password,
the backend processes the request successfully. This indicates missing
workflow validation and improper authentication checks during sensitive
account operations.



## Proof of Concept (PoC)

1. Login as a normal user.
2. Navigate to **Profile / Change Password** page.
3. Leave the **Old Password** field empty.
4. Enter a new password and confirm it.
5. Submit the request.
6. Observe that the application responds with:  
   **“User updated successfully”**

![Password Change Without Old Password PoC](./screenshot/password-change-no-old-password.png)




## Impact

- Enables unauthorized password changes.
- Allows account takeover if an attacker gains a valid session.
- Breaks core authentication security guarantees.
- High risk to user account integrity.



## Recommendation

- Enforce mandatory server-side validation of the old password.
- Reject password change requests without the correct current password.
- Require re-authentication for sensitive account changes.
- Include strict workflow validation during authentication feature design.



