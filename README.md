![Active Directory lab](https://i.imgur.com/mCz35nj.png)

## Introduction

This project implements a small Active Directory environment designed to mirror common enterprise identity fundamentals. A Windows Server 2019 Domain Controller provides centralized authentication and policy enforcement while supporting core infrastructure services like DNS and DHCP. A Windows 10 client is joined to the domain to validate workstation enrollment, Group Policy application, and end-user behavior.

The lab emphasizes how directory services depend on networking fundamentals and how services like DNS, DHCP, and Group Policy interact to form a functional Windows-based infrastructure.


## Design Goals

* Build a realistic single-domain Active Directory environment
* Deploy DNS + DHCP as foundational services for domain operations
* Structure identity using OUs, groups, and delegated scope concepts
* Apply Group Policy to enforce baseline security and workstation restrictions
* Automate repetitive identity tasks using Python-based bulk user provisioning
* Validate end-to-end functionality through domain join and client testing

## Lab Architecture

### Domain Controller

* A Windows Server 2019 VM functions as the control plane for identity and policy:
* Active Directory Domain Services (AD DS)
* DNS role installed alongside AD DS
* DHCP for IP assignment within the internal subnet
* Optional NAT/RAS to provide controlled internet access for internal clients
* Group Policy Management for workstation and user restrictions

### Domain Client

* A Windows 10 VM represents an end-user workstation:
* Joined to the domain
* Receives IP configuration via DHCP
* Resolves internal resources via domain DNS
* Applies GPO settings linked to the appropriate OU
* Used to validate authentication and policy enforcement behavior

## Components

| Component          | Role                                                         |
|--------------------|--------------------------------------------------------------|
| VirtualBox         | Virtualization platform for hosting the lab                 |
| Windows Server 2019| Domain Controller running AD DS, DNS, DHCP, and GPO         |
| Windows 10         | Domain-joined workstation validating identity and policy    |
| DNS                | Name resolution for domain services and internal resources  |
| DHCP               | Automated IP address assignment for clients                 |
| RAS/NAT            | Optional routing for internal-to-external connectivity      |
| OUs + Groups       | Identity and policy scoping structure                       |
| Python + pyad      | Bulk user provisioning via LDAP                             |
| GPO                | Centralized enforcement of security and workstation controls|

## Workflow

1. The lab follows a typical infrastructure deployment sequence:
2. Configure network interfaces for internal domain traffic and external internet access
3. Install AD DS and promote the server to a Domain Controller (new forest)
4. Create Organizational Units (OUs) and identity structure for admins and users
5. Install and configure DHCP scope for internal subnet assignment
6. Configure NAT/RAS to allow controlled outbound connectivity (lab convenience)
7. Join Windows client to the domain and verify authentication
8. Validate DHCP assignment, DNS resolution, and internal connectivity
9. Automate bulk user creation using Python and LDAP binding
10. Apply Group Policy to enforce security controls and workstation restrictions

This mirrors how identity infrastructure is typically introduced in a small-to-mid sized environment.

## Tutorial Sections

Detailed walkthrough steps are provided below in collapsed sections.

<details>
  <summary><h4><b>Section 1: NETWORK INTERFACE CARD (NIC) CONFIGURATIONS</b></h2></summary>

  In this section, We'll be configuring the 2 NICs on the Windows Server 2019.
  
  ![Image 1](https://i.imgur.com/wDilWI5.png)
  
  **Step 1: Access Network Settings:**
  - Open "Network Connections" from the Control Panel
  
  **Step 2: Identify NICs:**
  - Identify the two NICs and renaming them to "Internet" and "Internal"
  
  **Step 3: Assign IP Addresses and Configure DNS:**
  - For NIC 1 (Internal):
    - IP Address: 10.2.22.1
    - Subnet Mask: 255.255.255.0
    - Default Gateway: (empty)
    - Preferred DNS Server: 127.0.0.1
  - For NIC 2 (Internet):
    - Obtain IP settings automatically (DHCP) for internet access
    - Obtain DNS server address automatically 
  
  **Reasons for the Configuration:**
  - NIC 1: Provides a gateway for the internal network.
    - **Explanation:** NIC 1 with IP "10.2.22.1" connects devices inside our network. We don't set a gateway to keep this network separate from the internet.
  - NIC 2: Enables connection to the internet.
    - **Explanation:** NIC 2 gets settings from the network, letting us connect online easily.
   
</details>

<details>
  <summary><h4><b>Section 2: INSTALL ACTIVE DIRECTORY DOMAIN SERVICES</b></h4></summary>
  
  In this section, we'll be installing Active Directory Domain Services (AD DS) on Windows Server 2019.
  
  
  **Step 1: Install AD DS:**
  - Open Server Manager.
  - Click "Manage" > "Add Roles and Features."
  - Choose "Role-based or feature-based installation" and click "Next."
  - Select the local server and click "Next."
  - Check "Active Directory Domain Services" and proceed.
  - Click through until you reach the installation summary, then click "Install."

  ![Image 2](https://i.imgur.com/L2OqS5J.png)

  
  **Step 2: Promote Server to Domain Controller:**
  - After installation, click "Promote this server to a domain controller."
  - Choose "Add a new forest" and set domain details.
    - Server: DC
    - Operation System: Windows Server 2019
    - Domain Name: Streetrack.com
  - Set a Directory Services Restore Mode (DSRM) password.
  - DNS can be left alone for automatic configuration.
  - Complete the wizard and let the server restart.
  
  ![Image 3](https://i.imgur.com/2TLFD6o.png)
  
  Awesome! We've successfully installed and configured Active Directory Domain Services on our Windows Server 2019.
</details>

<details>
  <summary><h4><b>Section 3: CREATING USERS AND ORGANIZATIONAL UNITS (OUs)</b></h4></summary>
  
  Here, we'll be exploring how to efficiently manage users by creating Organizational Units (OUs), adding users, and assigning administrative privileges.
  
  ![Image 4](https://i.imgur.com/eGgqXno.png)

  
  **Step 1: Create Organizational Units (OUs):**
  - Open "Active Directory Users and Computers"
  - Right-click on the domain name and choose "New" > "Organizational Unit"
  - Create 2 OUs and name them: "_ADMINS" and "_USERS" respectively

  ![Image 5](https://i.imgur.com/nBDdKb0.png)

  
  **Step 2: Create User Account:**
  - Right-click on the "_ADMINS" OU and choose "New" > "User"
  - Enter user details:
    - First Name: Thong
    - Last Name: Huynh
    - User Logon Name: thuynh
      
  ![Image 6](https://i.imgur.com/kozipGr.png)


  **Step 3: Add User to Domain Admins Group:**
  - Locate the user we just created and right-click
  - Select "Properties"
  - In the "Member Of" tab, click "Add"
  - Enter "Domain Admins" and click "Check Names"
  - Click "OK" to add the user to the "Domain Admins" group
  
  ![Image 7](https://i.imgur.com/TA0MoBV.png)
  
  ![Image 8](https://i.imgur.com/bgI8oMM.png)
  
  **Step 4: Verify User and OU Creation:**
  - Refresh Active Directory by restarting and log in with new Admin User credentials to confirm User and OU Creation
  
  ![Image 9](https://i.imgur.com/DLMH7ra.png)
  
  Yay! we've successfully created Organizational Units (OUs), added a user to the "_ADMINS" OU, and granted administrative privileges by adding our user to the "Domain Admins" group.
</details>

<details>
  <summary><h4><b>Section 4: INSTALL & CONFIGURE DHCP</b></h4></summary>
  
  In this section, we'll explore the process of installing and configuring the Dynamic Host Configuration Protocol (DHCP) to automate IP address assignment within our network.
  
  **Step 1: Open Server Manager:**
  - Launch "Server Manager" on the Windows Server 2019
  
  ![Image 10](https://i.imgur.com/0yCKtnX.png)
  
  **Step 2: Add DHCP Role:**
  - Click "Manage" > "Add Roles and Features"
  - Select "Role-based or feature-based installation" and click "Next"
  - Choose the local server(DC) and proceed
  - Check "DHCP Server" and complete the installation wizard
  
  ![Image 11](https://i.imgur.com/HUQbLnu.png)

  **Step 3: Configure DHCP:**
  - After installation, open "DHCP Manager" from "Administrative Tools"
  - Right-click on our server name and choose "Configure DHCP"
  - Follow the wizard, selecting the appropriate network connection
  
  ![Image 12](https://i.imgur.com/A4WWdGF.png)
  
  **Step 4: Create DHCP Scope:**
  - In "DHCP Manager," right-click on "IPv4" and choose "New Scope"
  - Set the scope name, IP range, subnet mask, default gateway, DNS servers, and lease duration:
    - Scope Name: 10.2.22.100-200
    - Start IP Address: 10.2.22.100
    - End IP Address: 10.2.22.200
    - Length: 24
    - Subnet Mask: 255.255.255.0
    - Default Gateway: 10.2.22.1
    - DNS: 127.0.0.1
    - Lease Duration: 8 days  
  
  ![Image 13](https://i.imgur.com/5n4O0CU.png)

  **Step 5: Authorize DHCP Server:**
  - If needed, we'll right-click on the server name in "DHCP Manager" and choose "Authorize."
  
  Great! We've successfully installed and configured DHCP, automating IP address assignment to devices within our network.
</details>

<details>
  <summary><h4><b>Section 5: INSTALL & CONFIGURE NAT</b></h4></summary>
  
  In this section, we'll focus on installing and configuring Network Address Translation (NAT), a technique that enables devices within our internal network to access the external internet while using a single public IP address.
  
  ![Image 14](https://i.imgur.com/t5Vyt6T.png)

  **Step 1: Open Server Manager:**
  - Launch "Server Manager" on the Windows Server 2019
  - Click "Manage" > "Add Roles and Features"
  - Select "Role-based or feature-based installation" and click "Next"
  - Choose the local server (DC) and proceed
  - Check "Remote Access" and continue
  - Check "Routing" and finish the installation wizard

  ![Image 15](https://i.imgur.com/M5CBamo.png)

  ![Image 16](https://i.imgur.com/9i7EKSO.png)
  
  **Step 2: Configure NAT:**
  - After installation, open "Routing and Remote Access" from "Administrative Tools"
  - Right-click on our server name and choose "Configure and Enable Routing and Remote Access"

  ![Image 17](https://i.imgur.com/zoDo8Aj.png)
  
  - Follow the wizard, selecting "Network address translation (NAT)"
  - Select the external network interface (Internet) for public connection
  
  ![Image 18](https://i.imgur.com/nMT8je3.png)

  - In the "NAT" section, right-click on the server name and choose "NAT" > "Enable"
</details>

<details>
  <summary><h4><b>Section 6: JOIN DOMAIN </b></h4></summary>
  
  **Step 3: Join Domain With Windows 10 VM:**  

  Here, we will run the Windows 10 VM, join the domain (Streetrack.com),  and test connectivity
  
  ![Image 19](https://i.imgur.com/ZSyic1G.png)
  
  - On the client VM (Windows 10), log in using the Domain Admin "thuynh"
  - Right-click the "Start Menu" and choose "System"

  ![Image 20](https://i.imgur.com/bAi0515.png)
  
  - Click on "Advanced system settings"

  ![Image 21](https://i.imgur.com/MMVbJTg.png)  
    
  - Go to the "Computer Name" tab and click "Change"  
  
  ![Image 22](https://i.imgur.com/yzG1KKq.png)  
  
  - Change "Computer name" to "Client-1"
  - Choose "Domain" and enter our domain name "Streetrack.com"  
  
  ![Image 23](https://i.imgur.com/W8OxzO3.png)  
  
  - Provide the "thuynh" credentials to join the domain  
  
  ![Image 24](https://i.imgur.com/EFKnAvz.png)  

  - Let's go! We've joined the Domain!  
</details>

<details>
  <summary><h4><b>Section 7: TEST CONNECTIVITY </b></h4></summary>

  Now, we will test and confirm the confgurations that we set, ensuring the proper DHCP assignments and being able to connect to the internet.
  
  **Step 4: Test Connectivity:**  

  - On the Windows 10 VM, open a Command Prompt
  - Use the following commands to verify network settings and connectivity:
    - Run `ipconfig` to check the assigned IP configuration
    - Run `ping www.google.com` to test internet connectivity  
  
  ![Image 25](https://i.imgur.com/ofXY8Sf.png)  
  
  There we go! We've successfully configured Network Address Translation (NAT), joined the domain using "thuynh" credentials, and verified internet and internal network connectivity on the client VM.
</details>

<details>
  <summary><h4><b>Section 8: CREATING USERS WITH PYTHON SCRIPT</b></h4></summary>

  In this section, we will be going through the process of creating and running a Python script that takes a text file with a list of usernames to make user creation smoother and more dynamic. This will add a layer of automation and customization to our homelab environment.  

  I've created the following files that we'll be using for this section:

  My_users_list.txt 
   - A list of over 100 names(first and last)  
  
  ![Image 26](https://i.imgur.com/LuDIMca.png)  

  Create_AD_Users.py
  
  ```python
  # This will import everything from the pyad module
  from pyad import *

  # Here, we'llset the default connection parameters for the Active Directory server
  pyad.set_defaults(ldap_server="10.2.22.1", username="thuynh@streetrack.com", password="Cyberlab123!")

  # This line will create a container object for the "_USERS" Organizational Unit (OU)
  ou = pyad.adcontainer.ADContainer.from_dn("OU=_USERS,DC=Streetrack,DC=com")

  # This will open the my_users_list text file and read its lines into the 'lines' variable
  with open('my_users_list.txt', 'r') as file:
      lines = file.readlines()

  # Iterate through each line in the 'lines' list
  for line in lines:
      # Here, we split the line into 'first_name' and 'last_name'
      first_name, last_name = line.strip().split()
    
      # Create a username by capitalizing the first letter of 'first_name' and making 'last_name' lowercase
      username = first_name[0].upper() + last_name.lower()
    
      try:
          # This line will create the Active Directory user with the 'username' and 'ou' specified
          user = pyad.aduser.ADUser.create(username, ou)
        
          # These updates will give the various attributes of the user
          user.update_attribute('displayName', f"{first_name} {last_name}")
          user.update_attribute('sAMAccountName', username)
          user.update_attribute('givenName', first_name)
          user.update_attribute('sn', last_name)
        
          # And now, the user's password
          password = "Cyberlab123!"
          user.set_password(password)
        
          # This line will print a success message
          print(f"User {username} created successfully.")
        
      except Exception as e:
          # This will print an error message if an exception occurs and will help with error handling. 
          print(f"Error creating user {username}: {str(e)}")
  ```

  
  - A Python script to create Users from the My_users_list.txt file
  - Users will be placed in the "_USERS" OU in "Streetrack.com" Domain
    
  
  ![Image 27](https://i.imgur.com/NSCjdXp.png)  
  
  **Step 1: Download and Install Python:**
  - Download Python from website, right-click install file and choose "Run as Administrator"  
  
  ![Image 28](https://i.imgur.com/LfjqV1i.png)  
  
  **Step 2: Install Required Dependencies:**
  - Open a Command Prompt as administrators  
  
  - Run the each of the following commands to install the necessary libraries and components:
    ```
    curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
    python get-pip.py
    pip install pyad
    pip install pywin32
    ```

  ![Image 33](https://i.imgur.com/L0A0n1e.png)  
  
  ![Image 34](https://i.imgur.com/1GwzbDo.png)  
  
  ![Image 35](https://i.imgur.com/9EreQX8.png)  
  
  **Step 3: Navigate to Script Directory:**
  - Navigate to the directory where the Python script and user list text file resides  

  ![Image 36](https://i.imgur.com/W0aHdyI.png)  
  
  ![Image 37](https://i.imgur.com/AKnhjst.png)  
  
  **Step 4: Run the Python Script:**
  - In the Command Prompt, run the script using the command:
    ```
    python create_ad_users.py
    ```
   
  
  ![Image 38](https://i.imgur.com/H8cpUUg.png)  
  
  **Step 5: Verify User Creations:**
  - In Active Directory Users and Computers, navigate to the "_USERS" OU to verify that the users created by the scripts are listed  
  
  ![Image 39](https://i.imgur.com/CP0Lcax.png)  
  
  **Step 6: Test User Accounts:**
  - Log into one of the created user accounts to confirm its functionality and attributes  
  
  ![Image 40](https://i.imgur.com/Aaz86PX.png)  
  
  Let's go!! We now have created and ran the Python script to automate user creations in Active Directory, streamlining the process and enhancing efficiency!
</details>
<details>
  <summary><h4><b>Section 9: GROUP POLICY CONFIGURATIONS</b></h4></summary>
  
  In this section, we're diving into Group Policy configurations, a powerful tool that allows us to manage various settings, security policies, and access controls across our Active Directory environment.  

  **Step 1: Access Group Policy Management:**
  - Open "Server Manager" on the Windows Server 2019.
  - Click "Tools" > "Group Policy Management."  
  
  ![Image 82](https://i.imgur.com/hPri6r2.png)  
  
  **Step 2: Create a New Group Policy Object (GPO):**
  - Right-click on the "Group Policy Objects" container and select "New."
  - We're going to name the GPO "Homelab Users GPO"  
  
  ![Image 83](https://i.imgur.com/gA7abT1.png)  
  
  ![Image 84](https://i.imgur.com/ZtkDWFo.png)  
  
  **Step 3: Edit GPO Settings:**
  - Right-click on the newly created GPO and select "Edit."  
  
  ![Image 85](https://i.imgur.com/wRANEkt.png)  
  
  - After selecting "Edit," navigate to the following path within the editor:
    
    - Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Password Policy
      - Password Policy:
        - Set password complexity requirements.
        - Enable "Password must meet complexity requirements."
        - Set minimum password length to 14 characters.  
        
  ![Image 86](https://i.imgur.com/o70pjTw.png)  
  
  ![Image 87](https://i.imgur.com/CXQg0dV.png)  
  
   - Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Account Lockout Policy
     - Account Lockout Policy:
      - Set account lockout threshold to 3 attempts
      - Other Account Lockout Policies will be changed to suggested values   
  
  ![Image 88](https://i.imgur.com/pOqc2nW.png)  

  - Computer Configuration > Administrative Templates > System 
    - Turn off forced restart. (Good Practice: Avoids unwanted restarts, preventing interruptions and data loss)  

  ![Image 89](https://i.imgur.com/nnhopXN.png)  
      
  - Computer Configuration > Administrative Templates > All Settings:
    - Prohibit user installs. (Good Practice: Limits unauthorized software installation to maintain control over applications running on the network)  

  ![Image 90](https://i.imgur.com/nIo4EHC.png)  
  
  - Computer Configuration > Administrative Templates > Windows Components > Store
    - Turn off the Store application. (Good Practice: Controls software sources by disabling the Microsoft Store application)  
        
  ![Image 91](https://i.imgur.com/H4SJGTe.png)  

  - Computer Configuration > Administrative Templates > Windows Components > OneDrive 
    - Prevent the usage of OneDrive for file storage. (Good Practice: Enhances data control by preventing accidental or unauthorized data exposure)  
  
  ![Image 92](https://i.imgur.com/oBgG0nj.png)  
    
  - User Configuration > Administrative Templates > Control Panel:
    - Prohibit access to the Control Panel (Good Practice: Prevents unauthorized system changes and helps maintain a consistent and secure enviroment)  
        
  ![Image 93](https://i.imgur.com/S4bzHwt.png)  
  
  - User Configuration > Administrative Templates > System:
    - Prevent access to the command prompt (Good Practice: Reduces misuse risks by preventing users from running commmands that might cause unintended damage or securtiy breach)  
        
  ![Image 94](https://i.imgur.com/BVA8llG.png)  
  
  - User Configuration > Administrative Templates > System > Removable Storage Access:
    - All Removable Storage classes: Deny all access (Good Practice: Prevents data leakage by preventing unauthorized copying or transfering sensitive data onto external devices)  
        
  ![Image 95](https://i.imgur.com/Qj7QsSw.png)  
        
  **Step 4: Apply GPO to "_USERS" OU:**
  - Go back to Group Policy Management
  - Right-click on the "_USERS" OU and select "Link an Existing GPO."
  - Choose the GPO we've created (Homelab Users GPO) and link it to the "_USERS" OU.  
  
  ![Image 96](https://i.imgur.com/BYugf90.png)  

  ![Image 97](https://i.imgur.com/SERQGPT.png)  
  
  **Step 5: Force Group Policy Update:**
  - On the client machine, open a Command Prompt as an administrator.
  - Run the command: `gpupdate /force`  
  
  ![Image 98](https://i.imgur.com/8MN86eY.png)  
  
  **Step 6: Test GPO Effects:**
  - Verify that the applied GPO settings are in effect on the client machine.  
  
  ![Image 99](https://i.imgur.com/J6eEOLm.png)  

  ![Image 100](https://i.imgur.com/5TfBer6.png)  
  
  Group Policy configurations offer a structured way to manage and enforce consistent settings, security policies, and access controls. By effectively using GPOs, we can enhance the security and organization of the network, creating a more efficient system.
</details>

## Validation
  
![Active Directory lab2](https://i.imgur.com/NDN69mG.png)

**Validated outcomes:**

* Domain identity services function correctly
  * Domain Controller provides Active Directory, DNS, and authentication services
  * Internal DNS supports domain join and authentication workflows
* Client networking and domain enrollment are successful
  * Client receives network configuration via DHCP
  * Client joins the domain and authenticates using domain credentials
* OU structure and Group Policy enforcement work as intended
  * Users and admins are scoped into appropriate Organizational Units
  * Linked Group Policy Objects apply after `gpupdate /force`
* Automated user provisioning is validated
  * Python script creates user accounts in Active Directory
  * Accounts are placed in the correct OU and can authenticate on the client VM
* End-to-end identity flow is confirmed
  * Users created in Active Directory can log in to a domain-joined client
  * Policy restrictions apply consistently at user logon

## Summary

This Active Directory lab demonstrates foundational enterprise identity concepts by deploying a functional domain environment with DNS, DHCP, Group Policy, and domain-joined client validation. The focus is on understanding dependencies, implementing core services correctly, and proving behavior from the workstation perspective rather than treating Active Directory as a standalone configuration exercise.





