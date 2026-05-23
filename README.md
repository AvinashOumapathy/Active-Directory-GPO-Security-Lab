# Active-Directory-GPO-Security-Lab
# Enterprise Active Directory & GPO Security Sandbox Lab

An enterprise-grade, isolated Active Directory (AD) home lab designed to simulate a corporate network environment. This project demonstrates hands-on technical proficiency in virtualization, network subnetting, directory infrastructure design, Identity & Access Management (IAM), and security baseline enforcement via Group Policy Objects (GPOs).

---

## 🛠️ Project Architecture & Specifications

The entire sandbox is hosted locally inside an isolated virtual network, ensuring zero interference with production environments while simulating real-world enterprise networking conditions.

* **Hypervisor:** VirtualBox
* **Virtual Network Switch:** `AD-Sandbox` (NAT Network)
* **Network Range:** `10.0.2.0/24` (Default Gateway: `10.0.2.1`)
* **Domain Name:** `hq.local` (Active Directory Domain Services)

```text
                     ┌────────────────────────────────┐
                     │     VirtualBox Host (Mac)      │
                     └───────────────┬────────────────┘
                                     │ (AD-Sandbox NAT Network)
          ┌──────────────────────────┴──────────────────────────┐
          ▼                                                     ▼
┌───────────────────┐                                 ┌───────────────────┐
│       DC-02       │                                 │       PC-01       │
│ Windows Server    │                                 │    Windows 10     │
├───────────────────┤                                 ├───────────────────┤
│ Role: Domain Ctrl │◄───────────────────────────────┤ Role: Workstation │
│ IP: 10.0.2.10     │          (Active Domain         │ IP: 10.0.2.20     │
│ DNS: 127.0.0.1    │           Handshake)            │ DNS: 10.0.2.10    │
└───────────────────┘                                 └───────────────────┘

Virtual Machine Configuration Matrix
Machine Name	Operating System	Network Mode	            IP Address	  Primary Role
DC-02	Windows  Server 2022	      NAT Network (AD-Sandbox)	10.0.2.10/24	Primary Domain Controller & DNS Server
PC-01	Windows  10 Enterprise	     AT Network (AD-Sandbox)	10.0.2.20/24	Corporate Workstation (Joined to hq.local)
Lab Deployment Milestones

Phase 1: Virtual Infrastructure & Sandbox Isolation
Configured a custom NAT Network named AD-Sandbox in the VirtualBox hypervisor mapping the private IP subnet range 10.0.2.0/24.
Consolidated scattered configuration files (.vbox, .vdi) out of external host media into an organized local directory structure.
Assigned both network cards to the matching virtual network switch to ensure complete host-to-guest networking sandbox isolation.
* Assigned both network cards to the matching virtual network switch to ensure complete host-to-guest networking sandbox isolation.

![VirtualBox Operational VM Dashboard](screenshot1.jpg)

![Isolated NAT Network Switch Environment](screenshot2.jpg)

Phase 2: Domain Controller Static Mapping
Hardcoded DC-02 with a static IPv4 address configuration to ensure reliable service delivery:
IP Address: 10.0.2.10
Subnet Mask: 255.255.255.0
Default Gateway: 10.0.2.1
Assigned a loopback address (127.0.0.1) for the Preferred DNS Server so the Domain Controller points to its local host resolver database.
* Assigned a loopback address (`127.0.0.1`) for the Preferred DNS Server so the Domain Controller points to its local host resolver database.

![Domain Controller Core Network Interface](screenshot3.jpg)

![Windows Defender Firewall Core Profile Mitigation](screenshot4.jpg)

Phase 3: Active Directory Domain Services Provisioning
Installed the core Active Directory Domain Services (AD DS) directory system roles and administrative dependencies.
Promoted DC-02 to a primary Domain Controller by initiating a brand-new forest structure under the FQDN: hq.local.
Verified successful directory schema registration and domain database health inside the Active Directory Users and Computers administrative console.
* Verified successful directory schema registration and domain database health inside the Active Directory Users and Computers administrative console.

![Initial Directory Services Framework Deployment](screenshot5.jpg)


Phase 4: Identity & Access Management (IAM)
Structured a logical corporate schema within Active Directory by creating a department-specific Organizational Unit (OU) for the IT department.
Created a standard enterprise domain user account for employee John Doe (jdoe / John Doe) within the IT OU.
Defined standard password requirements and administrative security parameters to manage the user profile context securely.
* Provisioned standard enterprise testing user accounts to simulate diverse departmental deployment scopes across the network database.

![Initial Identity Structuring Context](screenshot6.jpg)

![Finalized Departmental OU Architecture with Active User Object](screenshot7.jpg)

Phase 5: Client Enrollment & Domain Authentication
Configured the workstation client adapter setting (PC-01) to point to the server (DC-02 at 10.0.2.10) as its primary DNS resolver.
Resolved VirtualBox APIPA interface loops (169.254.x.x) by performing hardware reset adapter toggles and mapping static workstation IPs (10.0.2.20).
Executed active domain path validation via ICMP ping to ensure DNS successfully translated hq.local to the Domain Controller IP.
Enrolled the client workstation to the domain using administrative credentials, rebooted the node, and successfully logged in using John Doe's credentials (jodoe@hq.local).
* Resolved VirtualBox APIPA interface loops (169.254.x.x) by performing hardware reset adapter toggles and mapping static workstation IPs (10.0.2.20).

![Client Workstation Manual Network Mapping Properties](screenshot8.jpg)

* Executed active domain path validation via ICMP ping to ensure DNS successfully translated hq.local to the Domain Controller IP.

![Successful Network Handshake Validation Interface](screenshot9.jpg)

* Enrolled the client workstation to the domain using administrative credentials, rebooted the node, and successfully logged in using John Doe's credentials (jdoe@hq.local).

![Client Workstation Corporate Domain Authentication Prompt](screenshot10.jpg)


Phase 6: System Hardening & Group Policy Enforcement (GPO)
Implemented security-hardening baselines to restrict end-user capabilities on corporate assets:
Generated a Group Policy Object named IT_Department_Restrictions inside the Group Policy Management Console.
Linked the GPO directly to the IT Organizational Unit to target domain users selectively while preserving system administrator privileges.
Configured the following security template restriction: User Configuration -> Policies -> Administrative Templates -> Control Panel -> Prohibit access to Control Panel and PC settings.
Triggered immediate policy distribution on the workstation client by forcing GPO propagation from the command line:
DOS
gpupdate /force
Evaluated enforcement by attempting to open the classic Control Panel under the jdoe profile, resulting in immediate administrative block errors.
* Triggered immediate policy distribution on the workstation client by forcing GPO propagation from the command line:
![Group Policy Template Restriction Schema Configuration](screenshot11.jpg)
* Evaluated enforcement by attempting to open the Control Panel under the jdoe profile, resulting in immediate administrative block errors.
![Active Endpoint Security Policy Lockout Warning](screenshot12.jpg)

### Phase 7: Tiered Access Controls & File Share Security

Implemented data loss prevention and access security structures by grouping identities and enforcing strict directory boundaries:

* Proportioned corporate access lines by creating two distinct Active Directory Security Groups inside the IT folder: `IT-Admins` and `IT-Staff`.
* Enrolled domain user John Doe (`jdoe`) as an active member of the restricted `IT-Staff` group to manage credential inheritance.

![Active Directory Security Group Membership Allocation](screenshot13.jpg)

* Provisioned a secured network file server volume (`CompanyData`) inside the root server directory, masking its accessibility across the local virtual network range (`\\10.0.2.10\CompanyData`).
* Hardcoded explicit Share and local NTFS Access Control Lists (ACLs) to strip write/modify capabilities away from standard tier accounts while preserving full administrative control.
* Validated least-privilege enforcement by logging back into `PC-01` as John Doe, mapping the directory volume, and attempting file alterations—resulting in immediate system access denial block windows.

![Active Endpoint Access Control List Permission Lockout](screenshot14.jpg)


🧠 Core Technical Skills Proven
Virtualization Architecture: Virtual host-only/NAT network switch design, virtual resource management.
Network Infrastructure & Subnetting: Custom static IP mapping, gateway routing paths, DNS loopbacks.
Directory Services Administration: Active Directory schema layout, Organization Units (OUs), IAM user provisioning.
Security Governance via Group Policy (GPOs): Designing, linking, and enforcing Group Policy Objects to control local operating system registries.
IT Troubleshooting & Diagnostics: Resolving IP address conflicts, mitigating DHCP/APIPA network card isolation loops, and tracing network paths using command line utilities (ipconfig, ping, gpupdate).
