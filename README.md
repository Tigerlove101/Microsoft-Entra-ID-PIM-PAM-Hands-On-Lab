**Hybrid IAM, PIM/PAM & Microsoft Sentinel Identity Monitoring Lab**

 **Project Overview**

This project demonstrates a hybrid Identity and Access Management (IAM) environment integrating on-premises Active Directory with Microsoft Entra ID, Privileged Identity Management (PIM), Privileged Access Management (PAM), Multi-Factor Authentication (MFA), and Microsoft Sentinel.

The objective was to implement and monitor identity security controls using an enterprise-style IAM and security monitoring workflow.



**Lab Architecture**

* On-Premises Active Directory
* Microsoft Entra ID
* Microsoft Entra Cloud Sync
* Microsoft Entra PIM
* Microsoft Entra MFA
* Microsoft Sentinel
* Log Analytics Workspace
* Windows endpoints
* Active Directory security groups

Evidence:



**Hybrid Identity Implementation**

Configured Microsoft Entra Cloud Sync to synchronize on-premises Active Directory users and groups with Microsoft Entra ID.

Implementation

* Configured Entra Cloud Sync
* Connected on-premises Active Directory to Entra ID
* Synchronized users
* Synchronized security groups
* Verified synchronization in Entra admin center

Evidence:


<img width="1920" height="1012" alt="user sync to cloud entra" src="https://github.com/user-attachments/assets/c737657b-7a76-4dc0-8335-7076297b7f6d" />

<img width="955" height="398" alt="Screenshot 2026-10-28 221148" src="https://github.com/user-attachments/assets/c4de26f7-21e1-4899-afc4-d364d440d754" />

<img width="959" height="464" alt="Screenshot 2026-10-28 221348" src="https://github.com/user-attachments/assets/46c4c392-7610-4096-8822-c4e50fee7632" />




**Active Directory Least Privilege**

Implemented delegated administration using Active Directory security groups and delegated permissions.

Test

A temporary administrator was delegated permission to reset passwords for users within the IAM lab.

The permission was tested successfully and then removed.

Result

* Password reset succeeded while permission was assigned.
* Password reset failed after permission removal.
* Demonstrated least-privilege access and privilege revocation.

Evidence:

<img width="1920" height="1012" alt="delegation wizard" src="https://github.com/user-attachments/assets/58bd77b7-e4d6-4dce-a824-39edce5393f5" />

<img width="1280" height="720" alt="account password rester by delegated group manager" src="https://github.com/user-attachments/assets/58e6ac2d-f0f5-46b3-a8e9-81d97f61192c" />

<img width="1280" height="720" alt="pass reset access denied" src="https://github.com/user-attachments/assets/ca672979-fa6f-458e-a52c-66fef8db369d" />




**Role-Based Access Control**

Implemented group-based access control using Active Directory security groups.

Example

An HR security group was granted access to an HR file share.

The default Everyone permission was removed.

Result

* Authorized HR users successfully accessed the resource.
* Unauthorized users were denied access.

Evidence:
<img width="1920" height="1012" alt="hr security-group manager" src="https://github.com/user-attachments/assets/eed5acfc-eb2c-4156-9379-c2375510eb84" />


<img width="1920" height="1012" alt="security manager permission" src="https://github.com/user-attachments/assets/facc550c-ba89-409e-8ea3-f5965cd54d7c" />

<img width="1920" height="1012" alt="hr user permission" src="https://github.com/user-attachments/assets/3ce1e2b4-b09d-4fac-af18-0802367c5f52" />

<img width="1280" height="720" alt="access denied to update file" src="https://github.com/user-attachments/assets/05a3c64a-5453-46db-b96b-d52fe7262545" />





**Microsoft Entra PIM/PAM**

Configured Microsoft Entra Privileged Identity Management to provide just-in-time privileged access.

Configuration

* Assigned eligible administrative roles
* Configured activation requirements
* Configured activation duration
* Enabled MFA
* Tested role activation
* Tested role expiration

Evidence:

<img width="947" height="328" alt="Screenshot 2026-10-29 001158" src="https://github.com/user-attachments/assets/b3700fbd-48e3-48ed-ae24-ed0633bf6923" />

<img width="1920" height="1012" alt="group member activation process" src="https://github.com/user-attachments/assets/46fd865f-bbe1-4185-bbb8-846f47b631fa" />

<img width="1894" height="724" alt="image" src="https://github.com/user-attachments/assets/4f613680-1656-48f5-aed9-01e3fd418e55" />




**Just-in-Time Privileged Access Test**

Tested administrative access before, during, and after PIM activation.

Result

* User had no privileged access before activation.
* User received temporary privileged access after activation.
* Administrative operation succeeded during activation.
* Privileged access was removed after expiration.
* Administrative operation failed after expiration.

Evidence:



**Multi-Factor Authentication**

Configured Microsoft Authenticator MFA for privileged identity operations.

MFA was required during authentication and privileged access activation.

Evidence:



**Microsoft Sentinel Integration**

Configured Microsoft Entra diagnostic settings to send identity activity to Microsoft Sentinel through Log Analytics.

Monitored:

* Sign-in activity
* Audit activity
* Role management activity
* PIM activity

Evidence:



**Authentication Investigation**

Used Microsoft Sentinel SigninLogs to investigate user authentication activity.

The investigation examined:

* User
* Sign-in time
* Application
* Source IP
* Location
* Client application
* Authentication requirement
* Result
* Correlation ID

Evidence:

<img width="1885" height="904" alt="image" src="https://github.com/user-attachments/assets/4239b8d3-d45f-4a8d-aec0-64c7c0f62571" />

<img width="944" height="444" alt="sentile" src="https://github.com/user-attachments/assets/4aea360f-5d41-485b-bc32-3b7ad3c6b46c" />





**PIM Activity Investigation**

Used Microsoft Sentinel AuditLogs to monitor privileged identity activity.

Filtered RoleManagement events to identify:

* Role assignments
* PIM activation requests
* Successful activations
* Failed activations
* Privileged role changes

KQL:

AuditLogs
| where TimeGenerated > ago(24h)
| where Category == "RoleManagement"
| project
    TimeGenerated,
    OperationName,
    Result,
    Identity,
    CorrelationId,
    TargetResources
| order by TimeGenerated desc

Evidence:

<img width="953" height="446" alt="pim audit log" src="https://github.com/user-attachments/assets/327731f2-e212-46e1-a635-f2d6288975b5" />




**PIM Activation Evidence**

Investigated successful and failed PIM operations in Sentinel.

The events demonstrated:

* PIM activation request
* PIM activation completion
* Successful privileged access
* Failed privileged access request

Evidence:



**Identity Activity Correlation**

Used authentication activity as the investigation starting point.

The investigation process:

1. Identify the user’s successful sign-in.
2. Record the sign-in timestamp.
3. Review activity around the login.
4. Identify privileged operations.
5. Review PIM activation activity.
6. Compare timestamps, users, applications, IP addresses, and operation details.
7. Determine whether the activity was authorized.




**Investigation**

A user authentication event was identified in Microsoft Sentinel.

The investigation then examined activity surrounding the authentication event to determine whether privileged actions occurred.

PIM audit events were reviewed to identify role activation and administrative activity.

This demonstrated an identity-focused SOC investigation workflow:

Authentication → Activity Review → Privileged Access → PIM Audit → Correlation → Investigation



**Security Controls Demonstrated**

* Hybrid identity
* Identity synchronization
* Least privilege
* Role-Based Access Control
* Group-based access control
* Privileged Identity Management
* Just-in-time privileged access
* MFA
* Privilege expiration
* Privilege revocation
* Authentication monitoring
* Audit logging
* PIM monitoring
* Identity activity correlation
* Security investigation



**Skills Demonstrated**

* Active Directory administration
* Microsoft Entra ID
* Microsoft Entra Cloud Sync
* Microsoft Entra PIM
* IAM
* PAM
* RBAC
* Least privilege
* MFA
* Microsoft Sentinel
* KQL
* Identity investigation
* Security monitoring
* Privileged access monitoring
* Hybrid identity management



**Challenges and Troubleshooting**

During implementation, several configuration issues were identified and resolved.

Cloud Sync

Resolved Entra Cloud Sync synchronization issues and verified successful synchronization of users and groups.

PIM

Configured eligible role assignments and adjusted activation duration to comply with role activation policies.

Sentinel

Verified Entra diagnostic settings and Log Analytics ingestion before investigating PIM events.

Identity Monitoring

Validated that PIM events appeared in AuditLogs and used RoleManagement filtering to isolate privileged activity.



**Final Results**

The completed lab demonstrates a functional hybrid IAM security environment with:

* On-premises Active Directory
* Microsoft Entra ID
* Synchronized identities
* Group-based access control
* Delegated administration
* Least-privilege enforcement
* Entra PIM
* Just-in-time privileged access
* MFA
* Privilege expiration
* Microsoft Sentinel monitoring
* Authentication investigation
* PIM activity monitoring
* Identity activity correlation



**Evidence Gallery**

All screenshots supporting this project are stored in the screenshots/ directory.

The evidence demonstrates the configuration, testing, successful operations, denied operations, privileged access activation, expiration, authentication activity, and Sentinel monitoring results.



**Conclusion**

This project demonstrates practical implementation of hybrid IAM, least privilege, RBAC, PIM/PAM, MFA, and identity monitoring.

The lab combines identity administration with security monitoring to demonstrate how privileged access can be controlled, monitored, investigated, and revoked.
