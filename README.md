aws-iam-policies-boundaries-lab

AWS IAM Permissions Boundary Lab

Objective
Demonstrate how AWS IAM Permissions Boundaries restrict effective permissions even when broader permissions are granted through identity policies.



Architecture Concept

AWS evaluates permissions using:

Effective Permissions = Identity Policy ∩ Permissions Boundary

Even if a user has broad permissions, the permissions boundary enforces a maximum allowed scope.


Implementation Steps

1. Created Identity Policy

Policy Name: DevMixedAccessPolicy

Permissions:
- S3: ListBucket, GetObject
- EC2: DescribeInstances, DescribeRegions
- IAM: ListUsers

This policy intentionally grants broader access across multiple services.



2. Created Permissions Boundary

Policy Name: S3BoundaryPolicy

Permissions:
- Allows only S3 actions

Purpose:
- Restrict maximum permissions to S3 only

---

3. Created IAM User

User Name: DevUser

Attached:
- DevMixedAccessPolicy (identity policy)
- S3BoundaryPolicy (permissions boundary)

---

Testing

Manual Testing

| Service | Action | Result |
|--------|--------|--------|
| S3     | Access bucket | Allowed |
| EC2    | Describe instances | Denied |
| IAM    | List users | Denied |



Policy Simulator Results

 S3 actions → Allowed  
 EC2 actions → Denied  
 IAM actions → Denied  



Key Observation

Although the identity policy allows EC2 and IAM actions, they are denied because:

- The permissions boundary does not include those actions
- AWS applies implicit deny when no matching allow statement exists



Important Concepts

1. Implicit Deny
If no policy explicitly allows an action → it is denied

2. No Matching Statement
Occurs when:
- No applicable policy allows the action
- Common with permissions boundaries
3. Permissions Boundary Behavior
Acts as a maximum permission filter

Final Result

Identity Policy: S3 + EC2 + IAM  
Permissions Boundary: S3 only  

Effective Permissions: **S3 only**

---

Conclusion

This lab demonstrates that:

- Identity policies define potential permissions
- Permissions boundaries enforce maximum limits
- Effective permissions are always the intersection of both

This ensures least privilege and prevents privilege escalation.

---

Screenshots

- step2_identity_policy_json.png  
- step3_identity_policy_created.png  
- step4_attach_policy.png  
- step5_attach_boundary.png  
- step6_user_created.png  
- step7_console_login_credentials.png  
- step8_ec2_access_denied.png  
- step9_iam_access_denied.png  
- step10_policy_simulator_results.png  
