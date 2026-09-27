# Installing Windows 11 on Proxmox

## Prepare the installation media

1. Download the Windows 11 ISO from Microsoft's website.
2. Download the VirtIO drivers ISO and upload both ISO files to Proxmox storage.

## Create the VM

Create a virtual machine with these settings:

- **VM ID:** 210
- **Name:** Win11Client
- **Memory:** 8192 MB (4 GB minimum)
- **CPU:** 4 cores
- **Disk:** 80 GB
- Attach the Windows 11 ISO and VirtIO drivers ISO.
- Check the **QEMU Guest Agent** option.

Start the VM and run Windows Setup. If the installer cannot see the virtual disk, load the appropriate storage driver from the VirtIO ISO.

## Install and configure Windows

1. Select **Windows 11 Pro** during setup so the machine can join a domain.
2. If prompted to connect to a network, press **Shift+F10** to open Command Prompt and enter:

   ```cmd
   OOBE\BYPASSNRO
   ```

   After the VM restarts, continue setup using the offline option. This skips the network requirement; it does not bypass Windows licensing or activation.

3. Finish setup and create a password for the administrator account.
4. After Windows starts, install the necessary VirtIO drivers and QEMU Guest Agent from the VirtIO ISO.
5. Create a snapshot in Proxmox to keep a restore point from immediately after installation.
