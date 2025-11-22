# -System-Administration-and-Maintanance---SAM-Company-
# 🖥️ SYSTEM ADMINISTRATION AND MAINTENANCE PROJECT

---

## **1. Company Network Structure**

The SAM Company operates a multi-branch network across **Pretoria (Head Office), Soshanguve, Ga-Rankuwa, and Polokwane**. The Pretoria branch hosts the main data center containing all critical servers, acting as the core network hub. This topology ensures centralized authentication, data access, and system management, enabling stronger security and easier administration.

Each branch contains workstations and departmental PCs relying on Pretoria’s infrastructure for services such as:

- **Active Directory Domain Authentication**  
- **DNS name resolution**  
- **DHCP IP assignment**  
- **File sharing and centralized storage**  
- **Remote support using RDP**  
- **Image deployment services**  

This documentation outlines all the steps completed to design, secure, and manage this environment.

---

## **2. Setting IP for the Server**

A stable network requires predictable IP addressing. The server was configured with a **static IP address**, ensuring uninterrupted domain services.

### Steps followed:

1. **Network and Sharing Center** was opened  
2. Navigation to **Adapter Settings**  
3. **Ethernet Properties** of the server was opened  
4. **Internet Protocol Version 4 (TCP/IPv4)** was selected  
5. Configuration applied:
   - **Static IP Address**  
   - **Subnet Mask:** 255.255.255.0  
   - **Default Gateway:** Router IP  
   - **Preferred DNS:** Server's own IP  
   - **Alternate DNS:** Google 8.8.8.8  

Static IP ensures:

- DNS server stability  
- Domain Controller operations  
- DHCP services  
- Remote access (RDP)  

---

## **3. Adding Roles and Features**

Using **Server Manager**, several roles were installed to support enterprise network operations.

### Installed roles:

- **Active Directory Domain Services (AD DS)**  
- **DNS Server**  
- **DHCP Server**  
- **File and Storage Services**  
- **Group Policy Management**  
- **Windows Deployment Services (WDS)**  

Roles function as follows:

- **AD DS:** manages users, computers, and authentication  
- **DNS:** resolves names to IP addresses  
- **DHCP:** automates IP allocation  
- **Group Policy:** enforces security and configurations  
- **WDS:** automates workstation and server installations  

These installations prepared the server as the core of the domain network.

---

## **4. Setting Domain Name**

After AD DS installation, the server was promoted to a **Domain Controller**.

### Steps followed:

1. Post-installation wizard opened  
2. **Add a new forest** selected  
3. Domain name entered: **samcompany.local**  
4. Directory Services Restore Mode (DSRM) password configured  
5. Configuration completed and server restarted  

The server now functions as the primary domain controller, supporting user, computer, OU, and policy management.

---

## **5. Creating Organizational Units (OUs)**

Active Directory was structured using **ADUC** by creating OUs for each department:

- **IT Department**  
- **HR Department**  
- **Finance Department**  
- **Procurement Department**  
- **Staff Computers**  
- **Admin Accounts**  

This structure improved security, reduced misconfigurations, and allowed fine-grained administrative control.

---

## **6. Creating New Users**

Users were added per department with the following conventions:

- Username format: **firstname.surname**  
- Temporary passwords assigned  
- “User must change password at next logon” enabled  
- Added to **security groups** (ITGroup, HRGroup, FinanceGroup, ProcurementGroup)  

This ensured proper access control and compliance.

---

## **7. Shared Folder Access**

Departmental shared folders were created:

| Department  | Access Level |
| ----------- | ------------ |
| IT          | Full Control |
| HR          | Modify       |
| Finance     | Modify       |
| Procurement | Read/Write   |

Permissions applied using:

- **NTFS permissions** (file-level)  
- **Share permissions** (network-level)  

This setup secured sensitive data and simplified administration.

---

## **8. IT Group Permissions**

The IT department was granted elevated privileges:

- Added to **Local Administrators** on workstations  
- Granted **PowerShell** access  
- Enabled **RDP**  
- Allowed software installation and system configuration  

---

## **9. HR Permissions**

HR department was restricted to sensitive resources:

- **Run command** disabled  
- PowerShell and Windows settings limited  
- Exclusive access to **HR shared folders**  

---

## **10. Disabling the Run Command for HR, Finance, and Procurement**

The Run command was disabled for the HR, Finance, and Procurement departments to:

- Prevent misuse of commands  
- Reduce malware risk  
- Maintain system stability  

---

## **11. Creating Group Policy Object (GPO)**

A GPO named **Disable_Run_For_NonIT** was created.

Configuration applied:

`User Configuration → Administrative Templates → Start Menu and Taskbar → Remove Run menu from Start Menu = Enabled`

---

## **12. Linking the GPO to OUs**

The GPO was linked to:

- HR OU  
- Finance OU  
- Procurement OU  

Run command restrictions were successfully applied to all targeted users.

---

## **13–16. Testing Run Command and IT Exception**

- HR, Finance, Procurement → **Blocked**  
- ITGroup → **Allowed**  
- Access verified using **Win + R** and **PowerShell**  

---

## **17–18. Enabling and Testing RDP for ITGroup**

- GPO: **Enable_RDP_IT** applied  
- ITGroup added to **Remote Desktop Users**  
- Firewall port **3389** opened  

**Tests:**

- Non-IT → Denied  
- IT → Allowed, confirming remote troubleshooting capability  

---

## **19. Converting Basic Disk to Dynamic Disk**

Dynamic disks were implemented to allow:

- Volume expansion  
- Mirrored and striped volumes  
- Flexible storage management  

Conversion confirmed using **Disk Management**.

---

## **20. Configuring Disk Quotas**

Disk quotas configured:

- **Drive Properties → Quota Management** enabled  
- Warning level: 80%  
- Hard limit: 100%  

Applied to all non-IT users to prevent storage overuse.

---

## **21. IT Department Automatic Deployment – WDS**

Windows Deployment Services (WDS) installed to automate OS deployments:

- Role installed  
- Boot images (WinPE) added  
- Install images (Windows 10/11) added  
- PXE boot configured  
- DHCP integrated  

Benefits:

- Time-saving  
- Standardized installations  
- Reduced manual errors  

---

## **22. DHCP IP Address Scope**

DHCP configured to automatically assign IP addresses:

- Scope range, subnet, gateway, DNS  
- Enabled seamless connectivity for all branch devices  

---

## **23–24. DNS Forward and Reverse Lookup Zones**

- Forward Zone configured: names → IP  
- Reverse Zone configured: IP → names  
- Static A and PTR records created for servers  

Supports domain operations and troubleshooting.

---

## **Conclusion**

The project successfully implemented:

- **Active Directory**  
- **DNS & DHCP**  
- **GPO security policies**  
- **Shared folder management**  
- **Storage quotas and dynamic disks**  
- **RDP restrictions**  
- **Automated OS deployment (WDS)**  

The SAM Company network is now **secure, scalable, and fully operational**, demonstrating professional Windows Server administration practices.
