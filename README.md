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


