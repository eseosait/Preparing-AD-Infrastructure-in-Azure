# Active Directory: Preparing AD Infrastructure in Azure

## Project Overview

In this lab, I prepared the cloud infrastructure required for an Active Directory environment in Microsoft Azure.

I created a Windows Server 2022 virtual machine named **DC-1** and a Windows 10 virtual machine named **Client-1**. I connected both systems to the same Azure virtual network, assigned DC-1 a static private IP address, and configured Client-1 to use DC-1 as its DNS server.

I then used PowerShell to test connectivity between the two virtual machines and verify Client-1’s DNS configuration.

This infrastructure prepares the environment for installing Active Directory Domain Services, promoting DC-1 to a domain controller, creating an Active Directory domain, and joining Client-1 to the domain.

## Environments and Technologies Used

- Microsoft Azure
- Azure Resource Groups
- Azure Virtual Networks and Subnets
- Azure Virtual Machines
- Windows Server 2022
- Windows 10
- Remote Desktop Protocol (RDP)
- PowerShell
- Domain Name System (DNS)
- Internet Control Message Protocol (ICMP)

## AD Infrastructure Overview

| Virtual Machine | Operating System | Purpose | Network Configuration |
|---|---|---|---|
| DC-1 | Windows Server 2022 | Future Domain Controller | Static private IP address |
| Client-1 | Windows 10 | Future Domain Client | DNS configured to use DC-1’s private IP address |

## Part 1: Create the Azure Resource Group

1. Signed in to the Microsoft Azure portal.
2. Searched for **Resource groups**.
3. Selected **Create**.
4. Created a new resource group for the Active Directory lab.
5. Placed both virtual machines and their networking resources inside the same resource group.

![Azure Resource Group] <img width="3024" height="1964" alt="image" src="https://github.com/user-attachments/assets/0ca182a1-1ef3-487a-952c-9c31477ebc51" />

## Part 2: Create the Virtual Network and Subnet

1. Searched for **Virtual networks** in the Azure portal.
2. Created a new virtual network.
3. Created a subnet inside the virtual network.
4. Confirmed that the virtual network was created in the same Azure region as the resource group.

Both virtual machines were connected to the same network so they could communicate through their private IP addresses.

![Virtual Network and Subnet] <img width="3024" height="1811" alt="image" src="https://github.com/user-attachments/assets/72620f20-ca4c-4af0-bf4f-b40f2932ded0" />

## Part 3: Create DC-1 in Azure

I created a Windows Server virtual machine using the following configuration:

| Setting | Configuration |
|---|---|
| Virtual machine name | DC-1 |
| Operating system | Windows Server 2022 |
| Administrator username | labuser |
| Region | Same region as the virtual network |
| Virtual network | Active Directory lab network |


![DC-1 Virtual Machine] <img width="3024" height="1812" alt="image" src="https://github.com/user-attachments/assets/fc8df630-5beb-456f-a555-f7e22557776e" />


## Part 4: Configure a Static Private IP Address for DC-1

DC-1 requires a consistent private IP address because Client-1 will use that address as its DNS server.

1. Opened **DC-1** in the Azure portal.
2. Opened **Networking** and selected the network interface.
3. Selected **IP configurations**.
4. Opened the primary IP configuration.
5. Changed the private IP assignment from **Dynamic** to **Static**.
6. Saved the configuration.

![DC-1 Static Private IP] <img width="3024" height="1814" alt="image" src="https://github.com/user-attachments/assets/fa445ce7-dcd2-4d27-ba68-f56e24cd4076" />

## Part 5: Connect to DC-1 and Configure the Firewall

1. Located DC-1’s public IP address in the Azure portal.
2. Connected to DC-1 using Remote Desktop Protocol.
3. Signed in with the administrator account created during deployment.
4. Temporarily disabled the Windows Defender Firewall profiles to test network connectivity.

> Disabling the firewall was performed only in this controlled lab environment. In a production environment, the firewall should remain enabled and only the required firewall rules should be configured.


## Part 6: Create Client-1 in Azure

I created a Windows client virtual machine using the following configuration:

| Setting | Configuration |
|---|---|
| Virtual machine name | Client-1 |
| Operating system | Windows 10 |
| Administrator username | labuser |
| Region | Same region as DC-1 |
| Virtual network | Same virtual network as DC-1 |

Placing Client-1 and DC-1 on the same virtual network allowed them to communicate privately inside Azure.

![Client-1 Virtual Machine] <img width="3024" height="1896" alt="image" src="https://github.com/user-attachments/assets/73c7a8c5-f827-4c21-b892-a9716d2d3182" />


## Part 7: Configure Client-1’s DNS Settings

I configured Client-1 to use DC-1 as its DNS server.

1. Opened **Client-1** in the Azure portal.
2. Opened **Networking** and selected Client-1’s network interface.
3. Opened **DNS servers**.
4. Selected the **Custom** DNS option.
5. Entered DC-1’s private IP address.
6. Saved the DNS configuration.
7. Restarted Client-1 through the Azure portal.

This configuration prepares Client-1 to locate the Active Directory domain after Active Directory Domain Services is installed on DC-1.

![Client-1 DNS Configuration] <img width="3024" height="1814" alt="image" src="https://github.com/user-attachments/assets/3ac14506-e282-437c-aaca-9371654587bf" />

## Part 8: Test Network Connectivity

After restarting Client-1, I connected to it through Remote Desktop and tested its connection to DC-1.

I opened PowerShell and ran:

```powershell
ping <DC-1-private-IP-address>
```

The successful replies confirmed that Client-1 could communicate with DC-1 across the Azure virtual network.

![Successful Ping Test] <img width="3024" height="1964" alt="image" src="https://github.com/user-attachments/assets/96060bab-b4fb-4adc-abc8-a57cfc000446" />


## Part 9: Verify the DNS Configuration

From PowerShell on Client-1, I ran:

```powershell
ipconfig /all
```

I reviewed the command output and confirmed that Client-1’s DNS server was set to DC-1’s private IP address.

![IPConfig DNS Verification] <img width="3024" height="1964" alt="image" src="https://github.com/user-attachments/assets/82708a76-e171-4825-8012-e0b39f591776" />

## Verification Summary

| Test | Expected Result |
|---|---|
| DC-1 private IP assignment | Static |
| DC-1 and Client-1 region | Same Azure region |
| DC-1 and Client-1 network | Same virtual network |
| Ping from Client-1 to DC-1 | Successful replies |
| Client-1 DNS server | DC-1’s private IP address |

## Skills Demonstrated

- Creating and managing Microsoft Azure resources
- Creating Windows Server and Windows client virtual machines
- Configuring Azure virtual networks and subnets
- Assigning a static private IP address
- Configuring a custom DNS server
- Connecting to virtual machines through Remote Desktop
- Testing network connectivity with `ping`
- Verifying network settings with `ipconfig /all`
- Preparing the infrastructure for an Active Directory domain


## Conclusion

In this lab, I prepared the foundational infrastructure for an Active Directory environment in Microsoft Azure. I created DC-1 and Client-1, connected them to the same virtual network, configured DC-1 with a static private IP address, directed Client-1’s DNS traffic to DC-1, and verified connectivity between the two systems.

The environment is now prepared for installing Active Directory Domain Services, promoting DC-1 to a domain controller, and joining Client-1 to the domain.
