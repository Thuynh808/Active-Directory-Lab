# Active Directory Homelab Project

![Active Directory Homelab](https://i.imgur.com/rDYFHff.png)
<br><br>
![Active Directory Homelab2](https://i.imgur.com/hGfReqr.png)


## Introduction

Welcome to my dynamic Homelab project, where I'll be crafting a virtual environment to simulate a mini corporate setup. Through VirtualBox, I've orchestrated two key players: a Windows Server 2019 Domain Controller and a Windows 10 Client. The Domain Controller stands as the authority figure, managing the digital domain, while the Windows 10 Client plays the role of an employee navigating within this intricate network. As I dive into configuring network interfaces, my aim is to create a functional small-scale representation that reflects the complexities of a real-world corporate IT environment. Join me as I navigate the details of this Homelab journey, aiming to create a rich learning experience in network management and system administration.


The Active Directory Homelab architecture includes:


- **VirtualBox:** The foundation for creating and managing virtual machines, facilitating real-world scenario simulation

- **Windows Server 2019:** As the Domain Controller, it manages users, devices, and policies, forming the core of Active Directory Domain Services

- **Windows 10:** Representing an employee workstation, it provides insights into the corporate end-user experience

- **Network Interfaces:** Enabling communication, with one for internet access and another for internal network interaction

- **Active Directory Domain Services:** Crucial for user and network organization, structuring users, computers, and resources coherently

- **DHCP:** Automates IP address assignment, streamlining network configuration

- **RAS/NAT:** Facilitates seamless internal-external network connectivity

- **User List Text File:** Stores user info, aiding efficient user setup and management

- **Python Script:** Adds automation and customization, enhancing user creation

- **Group Policy Object:** Enforces network-wide policies, maintaining settings, security, and access controls


Collectively, these tools and architectural elements culminate in an environment where virtual machines interact harmoniously, mirroring the intricacies of a corporate ecosystem. Through exploration and manipulation, this lab offers an immersive platform to experiment, learn, and finesse the art of network management and system administration.

<details>
  <summary><h2><b>Section 1: NETWORK INTERFACE CARD (NIC) CONFIGURATIONS</b></h2></summary>
  <br> <br>
  In this section, We'll be configuring the 2 NICs on the Windows Server 2019.<br><br>
  
  ![Image 1](https://i.imgur.com/wDilWI5.png)
  <br><br>
  
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
  <summary><h2><b>Section 2: INSTALL ACTIVE DIRECTORY DOMAIN SERVICES</b></h2></summary>
  <br><br>
  
  In this section, we'll be installing Active Directory Domain Services (AD DS) on Windows Server 2019.<br><br>
  
  
  **Step 1: Install AD DS:**
  - Open Server Manager.
  - Click "Manage" > "Add Roles and Features."
  - Choose "Role-based or feature-based installation" and click "Next."
  - Select the local server and click "Next."
  - Check "Active Directory Domain Services" and proceed.
  - Click through until you reach the installation summary, then click "Install."<br><br>

  ![Image 2](https://i.imgur.com/L2OqS5J.png)
<br><br>
  
  **Step 2: Promote Server to Domain Controller:**
  - After installation, click "Promote this server to a domain controller."
  - Choose "Add a new forest" and set domain details.
    - Server: DC
    - Operation System: Windows Server 2019
    - Domain Name: Streetrack.com
  - Set a Directory Services Restore Mode (DSRM) password.
  - DNS can be left alone for automatic configuration.
  - Complete the wizard and let the server restart.<br><br>
  
  ![Image 3](https://i.imgur.com/2TLFD6o.png)
<br><br>
  
  Awesome! We've successfully installed and configured Active Directory Domain Services on our Windows Server 2019.
</details>

<details>
  <summary><h2><b>Section 3: CREATING USERS AND ORGANIZATIONAL UNITS (OUs)</b></h2></summary>
  <br><br>
  
  Here, we'll be exploring how to efficiently manage users by creating Organizational Units (OUs), adding users, and assigning administrative privileges.<br><br>
  
  ![Image 4](https://i.imgur.com/eGgqXno.png)
<br><br>
  
  **Step 1: Create Organizational Units (OUs):**
  - Open "Active Directory Users and Computers"
  - Right-click on the domain name and choose "New" > "Organizational Unit"
  - Create 2 OUs and name them: "_ADMINS" and "_USERS" respectively<br><br>

  ![Image 5](https://i.imgur.com/nBDdKb0.png)
<br><br>
  
  **Step 2: Create User Account:**
  - Right-click on the "_ADMINS" OU and choose "New" > "User"
  - Enter user details:
    - First Name: Thong
    - Last Name: Huynh
    - User Logon Name: thuynh<br><br>
      
  ![Image 6](https://i.imgur.com/kozipGr.png)
<br><br>

  **Step 3: Add User to Domain Admins Group:**
  - Locate the user we just created and right-click
  - Select "Properties"
  - In the "Member Of" tab, click "Add"
  - Enter "Domain Admins" and click "Check Names"
  - Click "OK" to add the user to the "Domain Admins" group<br><br>
  
  ![Image 7](https://i.imgur.com/TA0MoBV.png)<br><br>
  
  ![Image 8](https://i.imgur.com/bgI8oMM.png)<br><br>
  
  **Step 4: Verify User and OU Creation:**
  - Refresh Active Directory by restarting and log in with new Admin User credentials to confirm User and OU Creation<br><br>
  
  ![Image 9](https://i.imgur.com/DLMH7ra.png)
<br><br>
  
  Yay! we've successfully created Organizational Units (OUs), added a user to the "_ADMINS" OU, and granted administrative privileges by adding our user to the "Domain Admins" group.
</details>

<details>
  <summary><h2><b>Section 4: INSTALL & CONFIGURE DHCP</b></h2></summary>
  <br><br>
  
  In this section, we'll explore the process of installing and configuring the Dynamic Host Configuration Protocol (DHCP) to automate IP address assignment within our network.
  
  **Step 1: Open Server Manager:**
  - Launch "Server Manager" on the Windows Server 2019.<br><br>
  
  ![Image 10](https://i.imgur.com/0yCKtnX.png)<br><br>
  
  **Step 2: Add DHCP Role:**
  - Click "Manage" > "Add Roles and Features"
  - Select "Role-based or feature-based installation" and click "Next"
  - Choose the local server(DC) and proceed
  - Check "DHCP Server" and complete the installation wizard<br><br>
  
  ![Image 11](https://i.imgur.com/HUQbLnu.png)<br><br>

  **Step 3: Configure DHCP:**
  - After installation, open "DHCP Manager" from "Administrative Tools"
  - Right-click on our server name and choose "Configure DHCP"
  - Follow the wizard, selecting the appropriate network connection<br><br>
  
  ![Image 12](https://i.imgur.com/A4WWdGF.png)<br><br>
  
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
    - Lease Duration: 8 days<br><br>
  
  ![Image 13](https://i.imgur.com/5n4O0CU.png)<br><br>

  **Step 5: Authorize DHCP Server:**
  - If needed, we'll right-click on the server name in "DHCP Manager" and choose "Authorize."<br><br>
  
  Great! We've successfully installed and configured DHCP, automating IP address assignment to devices within our network.
</details>

<details>
  <summary><h2><b>Section 5: INSTALL & CONFIGURE NETWORK ADDRESS TRANSLATION (NAT)</b></h2></summary>
  <br><br>
  
  In this section, we'll focus on installing and configuring Network Address Translation (NAT), a technique that enables devices within our internal network to access the external internet while using a single public IP address.<br><br>
  
  ![Image 14](https://i.imgur.com/t5Vyt6T.png)<br><br>

  **Step 1: Open Server Manager:**
  - Launch "Server Manager" on the Windows Server 2019
  - Click "Manage" > "Add Roles and Features"
  - Select "Role-based or feature-based installation" and click "Next"
  - Choose the local server (DC) and proceed
  - Check "Remote Access" and continue
  - Check "Routing" and finish the installation wizard<br><br>

  ![Image 15](https://i.imgur.com/M5CBamo.png)<br><br>

  ![Image 16](https://i.imgur.com/9i7EKSO.png)<br><br>
  
  **Step 2: Configure NAT:**
  - After installation, open "Routing and Remote Access" from "Administrative Tools"
  - Right-click on our server name and choose "Configure and Enable Routing and Remote Access"<br><br>

  ![Image 17](https://i.imgur.com/zoDo8Aj.png)<br><br>
  
  - Follow the wizard, selecting "Network address translation (NAT)"
  - Select the external network interface (Internet) for public connection<br><br>
  
  ![Image 18](https://i.imgur.com/nMT8je3.png)<br><br>

  - In the "NAT" section, right-click on the server name and choose "NAT" > "Enable"<br><br>
</details>

<details>
  <summary><h2><b>Section 6: JOIN DOMAIN </b></h2></summary>
  <br><br>
  
  **Step 3: Join Domain With Windows 10 VM:**<br><br>

  Here, we will run the Windows 10 VM, join the domain (Streetrack.com),  and test connectivity <br><br>
  
  ![Image 19](https://i.imgur.com/ZSyic1G.png)<br><br>
  
  - On the client VM (Windows 10), log in using the Domain Admin "thuynh"
  - Right-click the "Start Menu" and choose "System"<br><br>

  ![Image 20](https://i.imgur.com/bAi0515.png)<br><br>
  
  - Click on "Advanced system settings"<br><br>

  ![Image 21](https://i.imgur.com/MMVbJTg.png)<br><br>
    
  - Go to the "Computer Name" tab and click "Change"<br><br>
  
  ![Image 22](https://i.imgur.com/yzG1KKq.png)<br><br>
  
  - Change "Computer name" to "Client-1"
  - Choose "Domain" and enter our domain name "Streetrack.com"<br><br>
  
  ![Image 23](https://i.imgur.com/W8OxzO3.png)<br><br>
  
  - Provide the "thuynh" credentials to join the domain<br><br>
  
  ![Image 24](https://i.imgur.com/EFKnAvz.png)<br><br>

  - Let's go! We've joined the Domain!<br><br>
</details>

<details>
  <summary><h2><b>Section 7: TEST CONNECTIVITY </b></h2></summary>
  <br><br>

  Now, we will test and confirm the confgurations that we set, ensuring the proper DHCP assignments and being able to connect to the internet.
  
  **Step 4: Test Connectivity:**<br><br>

  - On the Windows 10 VM, open a Command Prompt
  - Use the following commands to verify network settings and connectivity:
    - Run `ipconfig` to check the assigned IP configuration
    - Run `ping www.google.com` to test internet connectivity<br><br>
  
  ![Image 25](https://i.imgur.com/ofXY8Sf.png)<br><br>
  
  There we go! We've successfully configured Network Address Translation (NAT), joined the domain using "thuynh" credentials, and verified internet and internal network connectivity on the client VM.
</details>

<details>
  <summary><h2><b>Section 8: CREATING USERS WITH PYTHON SCRIPT</b></h2></summary>
  <br><br>
  
  In this section, we will be going through the process of creating and running a Python script that takes a text file with a list of usernames to make user creation smoother and more dynamic. This will add a layer of automation and customization to our homelab environment.<br><br>

  I've created the following files that we'll be using for this section:

  My_users_list.txt 
   - A list of over 100 names(first and last)<br><br>
  
  ![Image 26](https://i.imgur.com/LuDIMca.png)<br><br>

  <details>
  <summary>Create_AD_Users.py <b>(CLICK HERE TO VIEW)</b></summary>
  
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
  </details>
  
   - A Python script to create Users from the My_users_list.txt file
   - Users will be placed in the "_USERS" OU in "Streetrack.com" Domain
   - Default password will be set to "Cyberlab123!"<br><br>
  
  ![Image 27](https://i.imgur.com/NSCjdXp.png)<br><br>
  
  **Step 1: Download and Install Python:**
  - Download Python from website, right-click install file and choose "Run as Administrator"<br><br>
  
  ![Image 28](https://i.imgur.com/LfjqV1i.png)<br><br>
  
  **Step 2: Install Required Dependencies:**
  - Open a Command Prompt as administrators<br><br>
  
  - Run the each of the following commands to install the necessary libraries and components:
    ```
    curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
    python get-pip.py
    pip install pyad
    pip install pywin32
    ```

  ![Image 33](https://i.imgur.com/L0A0n1e.png)<br><br>
  
  ![Image 34](https://i.imgur.com/1GwzbDo.png)<br><br>
  
  ![Image 35](https://i.imgur.com/9EreQX8.png)<br><br>
  
  **Step 3: Navigate to Script Directory:**
  - Navigate to the directory where the Python script and user list text file resides<br><br>

  ![Image 36](https://i.imgur.com/W0aHdyI.png)<br><br>
  
  ![Image 37](https://i.imgur.com/AKnhjst.png)<br><br>
  
  **Step 4: Run the Python Script:**
  - In the Command Prompt, run the script using the command:
    ```
    python create_ad_users.py
    ```
  <br><br>
  
  ![Image 38](https://i.imgur.com/H8cpUUg.png)<br><br>
  
  **Step 5: Verify User Creations:**
  - In Active Directory Users and Computers, navigate to the "_USERS" OU to verify that the users created by the scripts are listed<br><br>
  
  ![Image 39](https://i.imgur.com/CP0Lcax.png)<br><br>
  
  **Step 6: Test User Accounts:**
  - Log into one of the created user accounts to confirm its functionality and attributes<br><br>
  
  ![Image 40](https://i.imgur.com/Aaz86PX.png)<br><br>
  
  Let's go!! We now have created and ran the Python script to automate user creations in Active Directory, streamlining the process and enhancing efficiency!
</details>

<details>
  <summary><h2><b>Section 9: GROUP POLICY CONFIGURATIONS</b></h2></summary>
  <br><br>
  
  In this section, we're diving into Group Policy configurations, a powerful tool that allows us to manage various settings, security policies, and access controls across our Active Directory environment.<br><br>
  
  ![Image 26](https://i.imgur.com/9uBKp7F.png)<br><br>

  **Step 1: Access Group Policy Management:**
  - Open "Server Manager" on the Windows Server 2019.
  - Click "Tools" > "Group Policy Management."<br><br>
  
  ![Image 26](https://i.imgur.com/9uBKp7F.png)<br><br>
  
  **Step 2: Create a New Group Policy Object (GPO):**
  - Right-click on the "Group Policy Objects" container and select "New."
  - We're going to name the GPO "Homelab Users GPO"<br><br>
  
  ![Image 26](https://i.imgur.com/9uBKp7F.png)<br><br>
  
  **Step 3: Edit GPO Settings:**
  - Right-click on the newly created GPO and select "Edit."<br><br>
  
  ![Image 26](https://i.imgur.com/9uBKp7F.png)<br><br>
  
  - Navigate through the GPO editor to configure the following settings:
    - Password Policy:
      - Set password complexity requirements.
      - Enable "Password must meet complexity requirements."
    - Account Lockout Policy:
      - Set account lockout threshold to 3 attempts.
    - Account Policies:
      - Set minimum password length to 20 characters.<br><br>
  
  ![Image 26](https://i.imgur.com/9uBKp7F.png)<br><br>

   - User Configuration:
      - Administrative Templates > Control Panel:
        - Prohibit access to the Control Panel. (Good Practice: Prevents unauthorized system changes.)
      - Administrative Templates > System:
        - Prevent access to the command prompt. (Good Practice: Reduces misuse risks.)
        - Turn off forced restart. (Good Practice: Avoids unwanted restarts.)
      - Administrative Templates > System > Removable Storage Access:
        - All Removable Storage classes: Deny all access. (Good Practice: Prevents data leakage.)
      - Administrative Templates > System > Group Policy:
        - Disable software installation. (Good Practice: Maintains controlled software environment.)
      - Administrative Templates > System > Group Policy > Application Management:
        - Prohibit user installs. (Good Practice: Limits unauthorized software.)
      - Administrative Templates > System > Group Policy > Microsoft Store:
        - Turn off the Store application. (Good Practice: Controls software sources.)
      - Administrative Templates > System > Group Policy > OneDrive:
        - Prevent the usage of OneDrive for file storage. (Good Practice: Enhances data control.)
      - Administrative Templates > Windows Components > Windows Defender Antivirus:
        - Turn off Windows Defender Antivirus. (Good Practice: Supports independent antivirus solutions.)<br><br>
  
  ![Image 26](https://i.imgur.com/9uBKp7F.png)<br><br>
        
  **Step 4: Apply GPO to "_USERS" OU:**
  - Link the GPO to the "_USERS" Organizational Unit.
  - Right-click on the "_USERS" OU and select "Link an Existing GPO."
  - Choose the GPO you created and link it to the "_USERS" OU.<br><br>
  
  ![Image 26](https://i.imgur.com/9uBKp7F.png)<br><br>
  
  **Step 5: Force Group Policy Update:**
  - On the client machine, open a Command Prompt as an administrator.
  - Run the command: `gpupdate /force`<br><br>
  
  ![Image 26](https://i.imgur.com/9uBKp7F.png)<br><br>
  
  **Step 6: Test GPO Effects:**
  - Verify that the applied GPO settings are in effect on the client machine.<br><br>
  
  ![Image 26](https://i.imgur.com/9uBKp7F.png)<br><br>
  
  Group Policy configurations offer a structured way to manage and enforce consistent settings, security policies, and access controls. By effectively using GPOs, we can enhance the security and organization of the network, creating a more efficient system.
</details>

# Active Directory Homelab Project

![Active Directory Homelab](https://i.imgur.com/rDYFHff.png)
<br><br>
![Active Directory Homelab2](https://i.imgur.com/hGfReqr.png)

## Conclusion

In this dynamic Homelab project, we embarked on a journey to create a virtual environment that simulates a mini corporate setup. Through the orchestration of a Windows Server 2019 Domain Controller and a Windows 10 Client using VirtualBox, we ventured into the complexities of network management and system administration.

Our Active Directory Homelab architecture consisted of essential components:

- **VirtualBox:** Laying the foundation for creating and managing virtual machines, enabling realistic scenario simulation.
- **Windows Server 2019:** As the Domain Controller, it managed users, devices, and policies, forming the core of Active Directory Domain Services.
- **Windows 10:** Representing an employee workstation, it provided insights into the corporate end-user experience.
- **Network Interfaces:** Enabling communication, with distinct connections for internet access and internal network interaction.
- **Active Directory Domain Services:** Crucial for user and network organization, ensuring coherent structuring of users, computers, and resources.
- **DHCP:** Streamlined IP address assignment, simplifying network configuration.
- **RAS/NAT:** Facilitated seamless internal-external network connectivity.
- **Python Script:** Brought automation and customization, enhancing user creation.
- **Group Policy Object:** Enforced network-wide policies, maintained settings, security, and access controls.

This project allowed us to explore various aspects of network administration and system management:

- **NIC Configurations:** We configured network interfaces, establishing distinct connections for internal and external communication.
- **Installing AD DS:** Active Directory Domain Services were installed and configured, forming the backbone of our domain management.
- **Creating Users and OUs:** We learned to create organizational units, add users, and grant administrative privileges for effective user management.
- **DHCP Setup:** Dynamic Host Configuration Protocol simplified IP assignment, enabling smoother network integration.
- **RAS/NAT Configuration:** Network Address Translation empowered internal devices to access the internet using a single public IP.
- **Joining the Domain:** The Windows 10 Client was successfully joined to the domain, ensuring seamless interaction with our network.
- **User Automation:** With a Python script, we automated user creation, enhancing efficiency and customization.
- **Group Policy Configuration:** We explored Group Policy configurations, implementing security practices and access controls network-wide.

Throughout this endeavor, we've crafted a comprehensive environment that mirrors the complexities of a corporate network. By mastering these tools and techniques, we've honed our skills in network management and system administration. This Homelab project offers a rich platform for experimentation, learning, and refinement, providing a valuable experience for anyone aspiring to excel in the realm of IT infrastructure.





