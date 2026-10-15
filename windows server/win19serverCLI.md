## Windows Server 2019 Server Core domain controller setup

I installed Windows Server 2019 **Server Core** without the Desktop Experience. I signed in as the local administrator and opened Server Configuration:

```powershell
sconfig
```

In Server Configuration, I chose **8) Network Settings** and selected the network adapter. I configured the server with these settings:

I also used **2) Computer Name** in `sconfig` to rename the server to `DC01`, then restarted when prompted.

1. **Set Network Adapter Address** → **S** (Static), then set the server's address to `192.168.10.10` with a `/24` prefix.
2. **Set DNS Servers** → set `127.0.0.1` as the DNS server (loopback), then confirm the change.

I confirmed the network settings in `sconfig` before continuing.

### 2. Install DNS and AD DS

From an elevated PowerShell prompt, I installed the DNS Server and AD DS roles:

```powershell
Install-WindowsFeature DNS, AD-Domain-Services -IncludeManagementTools
```

### 3. Promote the server to a domain controller

I promoted the server by creating a new forest and domain named `corp.local`. I provided the Directory Services Restore Mode (DSRM) password when prompted:

```powershell
Install-ADDSForest -DomainName "corp.local" -InstallDNS
```

I confirmed the promotion. The server restarted when promotion was complete, and I signed in using the domain administrator account.

### 4. Create the OU structure

In an elevated PowerShell session, I imported the Active Directory module and created the parent OUs before their child OUs:

```powershell
Import-Module ActiveDirectory

New-ADOrganizationalUnit -Name "HomeLab" -Path "DC=corp,DC=local"
New-ADOrganizationalUnit -Name "Users" -Path "OU=HomeLab,DC=corp,DC=local"
New-ADOrganizationalUnit -Name "Management" -Path "OU=Users,OU=HomeLab,DC=corp,DC=local"
New-ADOrganizationalUnit -Name "Admin Workstations" -Path "OU=HomeLab,DC=corp,DC=local"
```

The resulting OU structure was:

```text
HomeLab
├── Users
│   └── Management
└── Admin Workstations
```

### 5. Verify the OUs

I verified the OUs by listing their names and distinguished names:

```powershell
Get-ADOrganizationalUnit -Filter "Name -like '*'" |
	Select-Object Name, DistinguishedName
```
