# Active Directory Homelab Project

![Active Directory Homelab](https://i.imgur.com/rDYFHff.png)
<br><br><br>
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

- **Python Script:** With its scripting prowess, Python adds a layer of automation and customization, making user creation smoother and more dynamic.

- **Group Policy Object:** A powerful tool for enforcing policies across the network, Group Policy Objects help maintain consistent settings, security, and access controls.

Collectively, these tools and architectural elements culminate in an environment where virtual machines interact harmoniously, mirroring the intricacies of a corporate ecosystem. Through exploration and manipulation, this lab offers an immersive platform to experiment, learn, and finesse the art of network management and system administration.
## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/j9rsRIH.png)

Initially, for the "BEFORE" metrics, all resources were deployed and directly exposed to the internet. Both Virtual Machines had unrestricted Network Security Groups and open built-in firewalls, while other resources were set up with public endpoints, lacking the utilization of Private Endpoints.

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/iMhHSHA.png)

In contrast, for the "AFTER" metrics, substantial security enhancements were implemented, resulting in improved overall security by mitigating vulnerabilities and safeguarding against unauthorized access and cyber threats. A series of comprehensive security measures were carefully deployed, with a strong focus on three key areas: 

- <b>Network Security Groups (NSGs)</b>: I hardened the NSGs by blocking all inbound and outbound traffic, with the sole exception of my own public IP address. This ensured that only authorized traffic from a trusted source was allowed to access the virtual machines.

- <b>Built-in Firewalls</b>: I secured the virtual machines by configuring their built-in firewalls. Fine-tuning the firewall rules for each entity reduced the potential attack surface.

- <b>Private Endpoints</b>: I improved security by replacing public endpoints with Private Endpoints for Azure resources. This restricted access to sensitive resources, protecting them from unauthorized access and potential threats.<br> 

## Attack Maps Before Hardening / Security Controls
> <b>NOTE:The attack maps were generated from a workbook using pre-built KQL .JSON map files, which also included geolocation data. These files provided structured representations of attack patterns and their data, allowing visualizations to illustrate cyber threats' geographic distribution and their impact on the system.</b>

![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/XjOmjl9.jpg)
- The attack map highlights the risks of leaving the Network Security Group (NSG) open, stressing the importance of implementing proper security measures, such as restricting NSG rules, to minimize threats. <br><br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/V5r4Vst.jpg)<br> <br>
- The attack map shows numerous RDP and SMB failures, emphasizing the need to secure remote access and file sharing services against potential cyber threats. <br> <br>
![Linux Syslog Auth Failures](https://i.imgur.com/KEMHHmO.jpg) <br> <br>
- The attack map shows syslog authentication failures on the Linux server, underscoring the importance of securing it with strong authentication and vigilant log monitoring. <br> <br>

## Metrics Before Hardening / Security Controls

During a 24-hour period in our insecure environment, we gathered the following metrics:

- Start Time: 2023-07-12 08:51:28
- Stop Time: 2023-07-13 08:51:28

| Metric                   | Count |
| ------------------------ | ----- |
| SecurityEvent            | 3186  |
| Syslog                   | 967   |
| SecurityAlert            | 1     |
| SecurityIncident         | 253   |
| AzureNetworkAnalytics_CL | 1094  |

These metrics reflect the data we collected while exploring the vulnerable environment over the span of a day.

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

Below is the information detailing the metrics obtained from our environment for another 24-hour duration, but this time after implementing security controls:

- Start Time: 2023-07-18 08:37:38
- Stop Time: 2023-07-19 08:37:38

| Metric                   | Count |
| ------------------------ | ----- |
| SecurityEvent            | 658   |
| Syslog                   | 0     |
| SecurityAlert            | 0     |
| SecurityIncident         | 0     |
| AzureNetworkAnalytics_CL | 0     |

## Conclusion

Throughout this project, we successfully built a mini honeynet in Microsoft Azure and seamlessly integrated log sources into a Log Analytics workspace. Employing Microsoft Sentinel allowed us to promptly activate alerts and generate incidents based on the collected logs. We meticulously assessed the metrics within the insecure environment prior to implementing security controls, and then once more after applying the necessary measures. The results showcased a remarkable reduction in security events and incidents following the implementation of these security controls, affirming their effectiveness.


The comparison of pre- and post-implementation metrics demonstrated a significant reduction in security events and incidents, which highlights the effectiveness of the enforced security controls.
It's important to mention that if the network's resources were extensively engaged by regular users, it's plausible that a higher number of security events and alerts could have been produced within the 24-hour timeframe post-security control implementation.

