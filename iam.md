### IAM (Identity and Access Management)

<details><summary>1. What is AWS IAM?</summary>
<code>AWS IAM is a service that lets you manage users, groups, and roles and securely control access to AWS services and resources.</code>
</details>

<details><summary>2. Purpose of IAM in AWS?</summary>
<code>IAM provides centralized access control, letting you define identities and permission sets to enhance AWS security and compliance.</code>
</details>

<details><summary>3. What are IAM users, groups, and roles?</summary>
<code>
- Users: Individual identities with credentials.
- Groups: Collections of users with shared permissions.
- Roles: Permission sets assumed temporarily by trusted entities (like services or external users).
</code>
</details>

<details><summary>4. How to secure your AWS account with IAM?</summary>
<code>
- Use strong password policies and MFA.
- Apply least privilege.
- Rotate access keys regularly.
- Use roles instead of static credentials.
- Monitor via CloudTrail.
- Use IAM/resource-based policies.
</code>
</details>

<details><summary>5. How to grant permissions to an IAM user?</summary>
<code>Attach policies directly to the user or through group membership. Policies define what actions are allowed or denied.</code>
</details>

<details><summary>6. What is an IAM policy?</summary>
<code>A JSON document defining permissions for AWS resources. Can be attached to users, groups, or roles.</code>
</details>

<details><summary>7. Types of IAM policies?</summary>
<code>
- Managed Policies: Reusable, AWS- or user-managed.
- Inline Policies: Embedded directly into one user/group/role.
</code>
</details>

<details><summary>8. What is the principle of least privilege?</summary>
<code>Grant only the minimum required access needed for tasks. Helps prevent misuse or damage from excess permissions.</code>
</details>

<details><summary>9. How to manage access keys for IAM users?</summary>
<code>Create, rotate, and delete access keys using Console, CLI, or SDKs. Prefer IAM roles for secure access.</code>
</details>

<details><summary>10. What is MFA in IAM?</summary>
<code>Multi-Factor Authentication requires multiple forms of identity verification, enhancing login security for AWS accounts.</code>
</details>

<details><summary>11. IAM roles for EC2 instances?</summary>
<code>Allow EC2 to assume roles for temporary credentials, avoiding hardcoded access keys. Roles are assigned at launch.</code>
</details>

<details><summary>12. What is IAM federation?</summary>
<code>Enables users from external identity providers (e.g., corporate directory, SSO) to access AWS without IAM user accounts.</code>
</details>

<details><summary>13. IAM policy evaluation logic?</summary>
<code>Deny by default. If no policy allows a request, it’s denied. Most specific and least permissive policy applies.</code>
</details>

<details><summary>14. How to create a custom IAM policy?</summary>
<code>Use Console, CLI, or SDKs. Write a JSON policy specifying actions, resources, and conditions, then attach it.</code>
</details>

<details><summary>15. What is the IAM condition element?</summary>
<code>Conditions restrict when policies apply, using key-value logic like time of day, source IP, or user attributes.</code>
</details>

<details><summary>16. How to rotate IAM access keys?</summary>
<code>Create a new key → update services → disable and delete old key to avoid downtime or compromise.</code>
</details>

<details><summary>17. What is IAM policy versioning?</summary>
<code>Supports multiple policy versions. Updates create new versions and allow rollback or comparison as needed.</code>
</details>

<details><summary>18. How to monitor IAM activity?</summary>
<code>Enable CloudTrail to log API calls. Analyze logs to detect access anomalies or policy-related errors.</code>
</details>

<details><summary>19. What is AWS Organizations and how does it relate to IAM?</summary>
<code>AWS Organizations manages multiple AWS accounts centrally. IAM governs access within each account; Organizations applies policies across accounts.</code>
</details>

<details><summary>20. How to troubleshoot IAM permission issues?</summary>
<code>Review IAM policies, roles, and group memberships. Use CloudTrail to check denied actions and validate permissions.</code>
</details>
