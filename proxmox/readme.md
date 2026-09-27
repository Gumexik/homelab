 # Proxmox VE Installation and Configuration

Basic instructions for installing and configuring Proxmox Virtual Environment (VE).

## Prerequisites

- A supported 64-bit server with virtualization enabled in BIOS/UEFI (Intel VT-x or AMD-V).
- At least 4 GB RAM; additional memory is recommended for virtual machines and containers.
- A reliable network connection and a static IP address.
- A Proxmox VE ISO downloaded from the [official website](https://www.proxmox.com/en/downloads).
- A USB drive for creating the installation media.

## Installation

1. Back up any data on the target server. The installer normally formats the selected disk.
2. Write the Proxmox VE ISO to a USB drive using a tool such as Rufus or Etcher.
3. Boot the server from the USB drive.
4. Select **Install Proxmox VE** and accept the license agreement.
5. Choose the installation disk and configure the country, time zone, and keyboard layout.
6. Set a strong root password and an administrative email address.
7. Configure the management network:
	- Hostname, including a resolvable fully qualified domain name.
	- Static IP address and subnet mask.
	- Gateway and DNS server.
8. Confirm the settings and complete the installation.
9. Remove the USB drive and reboot the server.

## First Login

Open the management interface in a browser:

```text
https://<proxmox-ip>:8006
```

Log in as `root` using the password created during installation. Accept the browser certificate warning if using the default self-signed certificate.

## Initial Configuration

1. Update the system:

	```bash
	apt update
	apt full-upgrade
	```

2. In **Datacenter > Storage**, verify the available storage. Add additional disks or network storage as required.
3. In **Datacenter > Network**, verify the management bridge, commonly `vmbr0`, and its physical network interface.
4. Configure time synchronization and confirm the server has the correct date and time.
5. Add a subscription or configure the appropriate package repository for your installation. Avoid mixing enterprise and non-subscription repositories.
6. Create an administrator account under **Datacenter > Permissions > Users** and use it instead of `root` for routine administration.
7. Enable two-factor authentication and restrict firewall access under **Datacenter > Firewall** and **Node > Firewall**.
8. Configure scheduled backups under **Datacenter > Backup**. Store backups on separate storage where possible.

## Create a Virtual Machine

1. Upload an operating system ISO under **Node > local > ISO Images**.
2. Select the node and click **Create VM**.
3. Set the VM name, ISO image, disk size, CPU, and memory.
4. Attach the network device to `vmbr0` or the intended bridge.
5. Finish the wizard and start the VM.
6. Install the guest operating system and install its guest agent if supported.

## Create an LXC Container

1. Download a container template under **Node > local > CT Templates**.
2. Click **Create CT** and configure the hostname, password, storage, CPU, memory, and network.
3. Start the container and complete its operating system configuration.

## Maintenance

- Apply updates regularly and reboot when the kernel or core services are updated.
- Test backups by restoring a VM or container.
- Monitor storage usage, memory, CPU, and disk health.
- Keep the management interface off the public internet; use a VPN or protected administration network.
- Review logs and permissions periodically.

Refer to the [Proxmox VE documentation](https://pve.proxmox.com/pve-docs/) for advanced networking, clustering, storage, and high-availability configuration.
