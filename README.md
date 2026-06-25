# Windows Server RDS Learning Path 17 — Create an RDS Session-Based Deployment

**Level:** Beginner · **Module:** 17/70

## Goal
Create and validate the session-based RDS deployment in Server Manager.

## Setup
1. Open Server Manager > Remote Desktop Services > Overview.
2. Confirm Connection Broker, Web Access and Session Host assignments.
3. Verify every server is reachable and managed.
4. Review Deployment Properties for certificates, Gateway and Licensing placeholders.
5. Keep Gateway disabled for the internal-only phase.
6. Review RDMS, Broker, Web Access and Session Host events.

```powershell
Import-Module RemoteDesktop
Get-RDServer
Get-RDDeploymentGatewayConfiguration -ErrorAction SilentlyContinue
Get-RDLicenseConfiguration -ErrorAction SilentlyContinue
Get-Service RDMS,Tssdis,TermService,W3SVC
```

## Evidence
Store RDS Overview screenshots, `Get-RDServer`, deployment properties, service state and event summaries under `evidence/`. Record any role showing an error and its remediation.

## Troubleshooting
If Server Manager cannot manage RDS01, test DNS, WinRM, Firewall, domain trust and pending reboots. Repair the deployment rather than manually editing its database.

## Security
Use the internal FQDN and trusted administration paths. External access is added later through RD Gateway.

## Rollback
Remove a failed deployment through the supported RDS deployment workflow after confirming no collection or user data depends on it.

## Next
`Windows-Server-RDS-Learning-Path-18-Create-the-First-RDS-Session-Collection`
