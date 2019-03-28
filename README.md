# Vagrant Dokumentation Gian-Luca Dal Pian

In diesem Repository, sehen sie, wie sie mit Vagrant einen Windows Server 2019 erstellen und konfigurieren können.

### Was ist Vagrant?
Vagrant ist eine freie Ruby-Anwendung zum Erstellen und Verwalten von virtuellen Maschinen.[2] Vagrant ermöglicht einfache Softwareverteilung (englisch Deployment) insbesondere in der Software- und Webentwicklung und dient als Wrapper zwischen Virtualisierungssoftware wie VirtualBox, KVM/QEMU, VMware und Hyper-V und Software-Configuration-Management-Anwendungen beziehungsweise Systemkonfigurationswerkzeugen wie Chef, Saltstack und Puppet. 

# System Anforderungen
- Windows 10 Pro
- Vagrant
- Hyper-V 

# Initializing Vagrant

Als erstes werden wir Vagrant bereit machen. Dazu erstellen wir ein Verzeichniss, in welchem wir die Vagrant Dateien erstellen. Dan werden wir in das Verzeichniss wechseln und folgender Befehl ausführen.
```sh
cd D:\Projects\Vagrant
```
```sh
vagrant init
```

### .Vagrant file bearbeiten

Für unsere WindowsVM werden wir das .Vagrant file folgendermassen bearbeiten:

```sh
# -- mode: ruby --
# vi: set ft=ruby :

#VAGRANT FILE
# CONRAD STEIGER

$script = <<'SCRIPT'

# Add User_01
NET USER User_01 "123Hallo321" /ADD

# Enable Remote Desktop
(Get-WmiObject Win32_TerminalServiceSetting -Namespace root\cimv2\TerminalServices).SetAllowTsConnections(1,1) | Out-Null
(Get-WmiObject -Class "Win32_TSGeneralSetting" -Namespace root\cimv2\TerminalServices -Filter "TerminalName='RDP-tcp'").SetUserAuthenticationRequired(0) | Out-Null
Get-NetFirewallRule -DisplayName "Remote Desktop*" | Set-NetFirewallRule -enabled true

cls

#PC Information
write-host "###### SYSTEM SETUP DONE ######"
Get-WmiObject -Class Win32_ComputerSystem | format-table
Get-WmiObject -Class Win32_LogicalDisk | format-table
Get-LocalUser | format-table 

SCRIPT

Vagrant.configure("2") do |config1|
	config1.vm.provider "hyperv"
	config1.vm.network "public_network"
	config1.vm.box = "StefanScherer/windows_2019"
	config1.vm.hostname = "Gianluca"
	config1.vm.provision "shell", inline: $script
end
```

# Vagrant Up

Nun führen wir das Skript aus. Dazu verwende ich Das Windows Terminal "CMD". Als erstes wechseln wir wider in das Verzeichniss und führen folgenden Befehl aus.
```sh
cd D:\Projects\Vagrant
```
```sh
vagrant up
```

### Netzwerkplan

    +---------------------------------------------------------------+
    ! Host = NB02                      !
    !                                                               !	
    !    +--------------------+          +---------------------+    ! 
    !    ! NB02 Gian-Luca     !          ! Vagrant             !    !       
    !    ! Host: Hyper-v      !          ! .Vagrant file       !    !
    !    !                    !          +---------------------+    !
    !    +--------------------+                   |                 !
    !                                             |                 !	
    !                                     +--------------------+    ! 
    !                                     ! Windows Server 2019!    !       
    !                                     ! Host: Gianluca     !    !
    !                                     +--------------------+    !
    +---------------------------------------------------------------+
    

