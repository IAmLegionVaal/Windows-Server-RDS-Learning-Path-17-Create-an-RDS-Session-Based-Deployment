# Windows Server RDS Learning Path 17 — Validate the RDS Session-Based Deployment

**Level:** Beginner · **Module:** 17/70

## Goal
Validate the session-based RDS deployment created in Module 16 and confirm that its assigned role services, management plane and internal connectivity are healthy before a session collection is created.

> Module 16 creates the deployment. Module 17 performs post-deployment validation and does not create a second deployment.

## Prerequisites
- Module 16 completed successfully
- `RDS01` assigned as RD Connection Broker, RD Web Access and RD Session Host for the compact lab
- Required restart completed
- Server Manager and PowerShell administrative access

## Validation workflow
1. Open **Server Manager > Remote Desktop Services > Overview**.
2. Confirm the deployment reports no unresolved role or server errors.
3. Confirm `RDS01` is assigned to Connection Broker, Web Access and Session Host.
4. Verify every deployment server is reachable and managed.
5. Review Deployment Properties for the current certificate, Gateway and Licensing state.
6. Keep Gateway disabled during the internal-only phase.
7. Verify the RDMS, Broker, Web Access and Session Host services.
8. Review the relevant RDS operational logs.
9. Remediate any failed prerequisite before creating a session collection.

```powershell
Import-Module RemoteDesktop -ErrorAction Stop

$ConnectionBroker = 'RDS01.corp.lab'

Get-RDServer -ConnectionBroker $ConnectionBroker |
    Format-Table Server,Roles -AutoSize

Get-Service -Name RDMS,Tssdis,TermService,W3SVC -ErrorAction Stop |
    Format-Table Name,Status,StartType -AutoSize

Get-RDDeploymentGatewayConfiguration -ConnectionBroker $ConnectionBroker `
    -ErrorAction SilentlyContinue
Get-RDLicenseConfiguration -ConnectionBroker $ConnectionBroker `
    -ErrorAction SilentlyContinue
```

## Acceptance criteria
- `Get-RDServer` returns the expected deployment server and role assignments.
- RDMS, Tssdis, TermService and W3SVC are running.
- Server Manager shows no unresolved deployment error.
- DNS, WinRM and secure-channel checks pass between the management host and `RDS01`.
- Gateway remains disabled during this internal phase.
- No unresolved critical RDS event prevents collection creation.

## Evidence
Store the RDS Overview screenshot, `Get-RDServer` output, deployment properties, service state, DNS and WinRM tests, and sanitized event summaries under `evidence/`. Record every error, remediation and retest.

## Troubleshooting
If Server Manager cannot manage `RDS01`, test DNS, WinRM, Windows Firewall, domain trust and pending reboots. Repair the supported deployment rather than manually editing its database.

## Security
Use the internal FQDN and trusted administration paths. Keep the lab internal during validation. External access is added later through RD Gateway.

## Rollback
Module 17 is read-only. If Module 16 created an unusable deployment, remove it only through the supported RDS deployment workflow after confirming that no collection or user data depends on it.

## Next
`Windows-Server-RDS-Learning-Path-18-Create-the-First-RDS-Session-Collection`
