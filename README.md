# Active Directory Homelab Project

![Active Directory Homelab](https://i.imgur.com/rDYFHff.png)
<br><br>
![Active Directory Homelab](https://i.imgur.com/hGfReqr.png)


## Introduction

Welcome to my dynamic Homelab project, where I'll be crafting a virtual environment to simulate a mini corporate setup. Through VirtualBox, I've orchestrated two key players: a Windows Server 2019 Domain Controller and a Windows 10 Client. The Domain Controller stands as the authority figure, managing the digital domain, while the Windows 10 Client plays the role of an employee navigating within this intricate network. As I dive into configuring network interfaces, my aim is to create a functional small-scale representation that reflects the complexities of a real-world corporate IT environment. Join me as I navigate the details of this Homelab journey, aiming to create a rich learning experience in network management and system administration.

The architecture of the Active Directory Homelab has the following componets:

- **VirtualBox:** Serving as the foundation, VirtualBox enables the creation and management of virtual machines, offering a platform to experiment and simulate real-world scenarios.

- **Windows Server 2019:** This heavyweight player acts as the Domain Controller, holding the reins of the digital realm. It manages users, devices, and policies while acting as the core of Active Directory Domain Services.

- **Windows 10:** Emulating the role of an employee workstation, Windows 10 offers insights into the end-user experience within the corporate setting.

- **Network Interfaces:** These digital connectors facilitate communication, with one dedicated to external internet access and another fostering internal network interaction.

- **Active Directory Domain Services:** A pivotal tool for user management and network organization, it helps structure users, computers, and resources in a coherent manner.

- **DHCP:** The Dynamic Host Configuration Protocol ensures that devices within the network receive IP addresses automatically, streamlining network configuration.

- **RAS/NAT:** Remote Access Server and Network Address Translation come together to enable seamless connectivity between the internal network and external internet, while also offering a bridge for VPN connections.

- **User List Text File:** This simple yet effective file serves as a repository for user information, facilitating efficient user setup and management.

- **Python Script:** After careful research and lessons, I created a Python script that adds a layer of automation and customization, making user creation smoother and more dynamic.

- **Group Policy Object:** A powerful tool for enforcing policies across the network, Group Policy Objects help maintain consistent settings, security, and access controls.

Collectively, these tools and architectural elements culminate in an environment where virtual machines interact harmoniously, mirroring the intricacies of a corporate ecosystem. Through exploration and manipulation, this lab offers an immersive platform to experiment, learn, and finesse the art of network management and system administration.


