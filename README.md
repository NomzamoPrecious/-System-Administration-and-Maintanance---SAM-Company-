# -System-Administration-and-Maintanance---SAM-Company-
# 🖥️ SYSTEM ADMINISTRATION AND MAINTENANCE PROJECT

---

## **1. Company Network Structure**

SAM Company operates a multi-branch network environment across four locations: **Pretoria (Head Office), Soshanguve, Ga-Rankuwa, and Polokwane**. The Pretoria office functions as the centralized data center and hosts all core server infrastructure, including **Active Directory Domain Services (AD DS), DNS, DHCP, file storage, and Windows Deployment Services (WDS)**. Centralization of critical services ensures that authentication, identity management, and data access are uniform across all branches, reducing administrative complexity and improving security.

All remote branches connect securely to Pretoria HQ via **VPN tunnels**, which provide encrypted communication channels that safeguard data in transit. While each branch maintains its own local workstations, essential services such as identity verification, network authentication, and file access rely on the Pretoria servers. This setup ensures that:

- User credentials are centrally validated through **AD DS**  
- Network resources are consistently available to all authorized users  
- Security policies can be enforced from a central location  
- Troubleshooting and system updates can be applied efficiently  

### Core Services Provided to Branches

- **Active Directory Domain Authentication:** Centralized authentication and access management for all users and devices.  
- **DNS Name Resolution:** Translates domain names to IP addresses, allowing devices to locate servers and services efficiently.  
- **DHCP IP Assignment:** Automates IP address allocation to prevent conflicts and ensure smooth network communication.  
- **File Sharing and Centralized Storage:** Provides secure access to departmental files and ensures data backup and version control.  
- **Remote Support Using RDP:** Enables IT administrators to manage systems remotely for troubleshooting or configuration.  
- **Image Deployment Services:** Automates workstation and server operating system installations, saving time and ensuring consistency.  

---

## **2. Server IP Configuration**

A **static IP address** was assigned to the main server to ensure stability, reliability, and uninterrupted network services. Static addressing is critical for core infrastructure because services such as **DNS, DHCP, and AD DS** rely on fixed IPs for proper resolution and operation. Dynamic IPs could cause service interruptions if addresses were reassigned.

### Configuration Process

1. **Network and Sharing Center** was opened to access network adapter settings.  
2. **Ethernet Properties** were modified to configure TCP/IPv4 manually.  
3. The following parameters were applied:
   - **Static IP Address** to ensure consistent network identity  
   - **Subnet Mask:** 255.255.255.0 to define the network range  
   - **Default Gateway:** Router IP for outbound traffic  
   - **Preferred DNS:** Server’s own IP to provide domain name resolution internally  
   - **Alternate DNS:** External DNS (8.8.8.8) for redundancy  

This configuration ensured that the server could reliably provide domain, DHCP, and DNS services without interruptions.

---

## **3. Roles and Features Installation**

Using **Server Manager**, key roles were installed to enable the server to function as a full **enterprise network hub**. These roles provide essential services for authentication, resource management, and network administration.

### Roles Installed

- **Active Directory Domain Services (AD DS):** Centralized management of user accounts, computer accounts, and authentication policies.  
- **DNS Server:** Resolves domain names to IP addresses, which is essential for communication across the network.  
- **DHCP Server:** Automatically assigns IP addresses to devices, simplifying administration and reducing errors.  
- **File and Storage Services:** Provides secure file storage and sharing capabilities.  
- **Group Policy Management:** Allows IT administrators to enforce security and configuration policies across multiple devices.  
- **Windows Deployment Services (WDS):** Automates operating system deployment, saving time and ensuring consistency across devices.  

Each role was installed with **default settings optimized for enterprise environments**, and post-installation checks were performed to ensure proper integration.

---

## **4. Domain Configuration**

The server was promoted to a **Domain Controller** to centralize user authentication, policy enforcement, and resource management. This step is critical for secure, scalable network administration.

### Steps Executed

1. The post-installation wizard was accessed.  
2. **Add a new forest** was selected to define a unique domain environment.  
3. Domain name configured as **samcompany.local**, which represents the organization internally.  
4. **Directory Services Restore Mode (DSRM)** password was set for disaster recovery purposes.  
5. Server restarted to complete promotion to a Domain Controller.  

Once complete, the server provided centralized authentication, simplified administration, and support for **Group Policy Objects (GPOs)** and shared resource management.

---

## **5. Organizational Units (OUs)**

Organizational Units (OUs) were created using **Active Directory Users and Computers (ADUC)** to logically separate users, computers, and resources by department. Proper OU design enables:

- Targeted policy enforcement  
- Simplified administration  
- Enhanced security through access segregation  

### OUs Created

- IT Department  
- HR Department  
- Finance Department  
- Procurement Department  
- Staff Computers  
- Admin Accounts  

This hierarchy allows GPOs to be applied specifically to departments or device types without affecting unrelated users.

---

## **6. User Account Creation**

Users were created within their respective OUs to reflect departmental organization. Security groups were assigned to control access to network resources.

### Key Configuration Points

- Username format: **firstname.surname**  
- Temporary passwords assigned; “User must change password at next logon” enabled  
- Membership in security groups: ITGroup, HRGroup, FinanceGroup, ProcurementGroup  

This ensures consistent naming, secure initial access, and proper group-based resource control.

---

## **7. Shared Folder Configuration**

Department-specific shared folders were created to provide secure access to critical files. Both **NTFS** and **share permissions** were configured to ensure granular security at the file and network levels.

| Department  | Permission Level | Description                         |
|------------|-----------------|-------------------------------------|
| IT         | Full Control    | Administer files and settings       |
| HR         | Modify          | Access employee-related files       |
| Finance    | Modify          | Access financial documents          |
| Procurement| Read/Write      | Manage supplier and purchasing data |

---

## **8. IT Group Privileges**

IT users were granted elevated privileges to manage infrastructure and troubleshoot issues efficiently:

- Added to **Local Administrators** on workstations  
- PowerShell access enabled  
- Remote Desktop Protocol (RDP) access provided  
- Software installation and configuration permissions granted  

These permissions allow IT to maintain network health without compromising security for regular users.

---

## **9. Departmental Restrictions**

HR, Finance, and Procurement departments were restricted to sensitive resources only. Specific measures included:

- **Run command disabled** to prevent execution of unauthorized tools  
- PowerShell access limited  
- Windows configuration settings restricted  
- Access granted only to departmental folders  

---

## **10. Group Policy Object (GPO) Implementation**

A **GPO** named **Disable_Run_For_NonIT** was created to enforce system restrictions on non-IT users.  

- Path: `User Configuration → Administrative Templates → Start Menu and Taskbar → Remove Run menu from Start Menu = Enabled`  
- Linked to HR, Finance, and Procurement OUs  
- Effectiveness confirmed through testing  

---

## **11. Remote Desktop Protocol (RDP) Configuration**

RDP access was restricted to ITGroup only:

- GPO **Enable_RDP_IT** applied  
- ITGroup added to **Remote Desktop Users**  
- Firewall port 3389 opened for remote connections  

Testing confirmed that:

- Non-IT users could not access RDP  
- IT users could successfully connect for remote administration  

---

## **12. Storage Management**

### Dynamic Disk Conversion

Basic disks were converted to **dynamic disks** to allow:

- Volume expansion without downtime  
- Mirrored or striped volumes for redundancy  
- Flexible storage management  

### Disk Quotas

Quota management was implemented on shared drives:

- Warning level: 80%  
- Hard limit: 100%  

This prevented excessive consumption of storage by individual users and ensured equitable space usage.

---

## **13. Windows Deployment Services (WDS)**

WDS was configured to automate the deployment of operating systems across workstations:

- Boot images (WinPE) added  
- Install images (Windows 10/11) added  
- PXE boot configured  
- DHCP integration completed  

**Benefits:**  

- Standardized installations reduce configuration errors  
- Time-efficient deployment saves administrative effort  
- Consistent environment across all workstations  

---

## **14. DHCP Configuration**

DHCP scopes were defined to automate IP assignment:

- Scope range, subnet mask, default gateway, and DNS configured  
- Simplified network configuration for all branch devices  
- Reduced risk of IP conflicts  

---

## **15. DNS Configuration**

### Forward Lookup Zone

- Translates **hostnames to IP addresses**  
- Static A records configured for all servers  

### Reverse Lookup Zone

- Translates **IP addresses to hostnames**  
- PTR records created to aid in troubleshooting and auditing  

---

## **Conclusion**

The project successfully implemented an enterprise **Windows Server network** including:

- Active Directory structure and user management  
- DNS and DHCP services  
- GPO security and access control  
- Shared folder configuration and permission management  
- Storage optimization with dynamic disks and quotas  
- RDP restriction for secure remote management  
- Automated OS deployment using WDS  

The SAM Company network is now **secure, scalable, and fully operational**, reflecting professional system administration standards.
