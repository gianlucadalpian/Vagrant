# Vagrant Dokumentation Gian-Luca Dal Pian

In diesem Repository, sehen sie, wie sie mit Vagrant einen Windows Server 2019 erstellen und konfigurieren können.

### Wissensstand

Ich habe schon oft mit Virtuellen Maschienen zu tun gehabt. Jedoch habe ich noch nie was von GitHub oder Vagrant gehört. Ich habe jedoch schon mit Containers in meinem Betieb zu tun gehabt. Aus diesem Grund interessiert mich dieses Modul grundsätzlich sehr. Ich habe mich im Internet ein wenig schlau egmacht und gesehen, das man mit Vagrant ganz einfach per Konpfdruck mehrere Vm's erstellen und sogar konfigurieren kann. Mit Linux habe ich in der Schule schon gearbeitet aber im Berufsalltag verwende ich es eher weniger, aus diesem Grund werde ich servascheinlich auch mehr mit Windows arbeiten. 

### Was ist Vagrant?
Vagrant ist eine freie Ruby-Anwendung zum Erstellen und Verwalten von virtuellen Maschinen.[2] Vagrant ermöglicht einfache Softwareverteilung (englisch Deployment) insbesondere in der Software- und Webentwicklung und dient als Wrapper zwischen Virtualisierungssoftware wie VirtualBox, KVM/QEMU, VMware und Hyper-V und Software-Configuration-Management-Anwendungen beziehungsweise Systemkonfigurationswerkzeugen wie Chef, Saltstack und Puppet. 

# System Anforderungen
- Windows 10 Pro
- Vagrant
- Hyper-V 

# Initializing Vagrant

Als erstes werden wir Vagrant bereit machen. Dazu erstellen wir ein Verzeichniss, in welchem wir die Vagrant Dateien erstellen. Dan werden wir in das Verzeichniss wechseln und folgender Befehl ausführen.
```sh
cd Documents\Vagrant
```
```sh
vagrant init
```

### .Vagrant file bearbeiten

Für unsere WindowsVM werden wir das .Vagrant file folgendermassen bearbeiten:

```sh
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.define "server1" do |server1|
    server1.vm.box = "StefanScherer/windows_2019"

    server1.vm.guest = :windows
    server1.vm.communicator = "winrm"
    server1.vm.boot_timeout = 600
    server1.vm.graceful_halt_timeout = 600

    server1.vm.network "private_network", ip: "192.168.1.100"
    server1.vm.network :forwarded_port, guest: 3389, host: 3388
    server1.vm.network :forwarded_port, guest: 5985, host: 5985, id: "winrm", auto_correct: true

    server1.vm.provider "virtualbox" do |vm|
        vm.name = "vagrant-pc"
        vm.gui = true
        vm.cpus = 2
        vm.memory = 2048

    end
  end 

    # Every Vagrant virtual environment requires a box to build off of.
  config.vm.define "server2" do |server2|
    server2.vm.box = "StefanScherer/windows_2019"

    server2.vm.guest = :windows
    server2.vm.communicator = "winrm"
    server2.vm.boot_timeout = 600
    server2.vm.graceful_halt_timeout = 600

    server2.vm.network "private_network", ip: "192.168.1.101"
    server2.vm.network :forwarded_port, guest: 3389, host: 3388
    server2.vm.network :forwarded_port, guest: 5985, host: 5985, id: "winrm", auto_correct: true

    server2.vm.provider "virtualbox" do |vm|
        vm.name = "vagrant-pc"
        vm.gui = true
        vm.cpus = 2
        vm.memory = 2048

    end

   end 
   
end
```
# Erfolglose versuche
```sh

NET USER User_01 "123Hallo321" /ADD

# Enable Remote Desktop
(Get-WmiObject Win32_TerminalServiceSetting -Namespace root\cimv2\TerminalServices).SetAllowTsConnections(1,1) | Out-Null
(Get-WmiObject -Class "Win32_TSGeneralSetting" -Namespace root\cimv2\TerminalServices -Filter "TerminalName='RDP-tcp'").SetUserAuthenticationRequired(0) | Out-Null
Get-NetFirewallRule -DisplayName "Remote Desktop*" | Set-NetFirewallRule -enabled true
```


# Vagrant Up

Nun führen wir das Skript aus. Dazu verwende ich Das Windows Terminal "CMD". Als erstes wechseln wir wider in das Verzeichniss und führen folgenden Befehl aus.
```sh
cd Documents\Vagrant
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
    !    +--------------------+           |       |                 !
    !                              ||||||||       |                 !	
    !     +--------------------+   |        +--------------------+  ! 
    !     ! Windows Server 2019!   |        ! Windows Server 2019!  !       
    !     ! Host: Gianluca     !---         ! Host: Gianluca     !  !
    !     +--------------------+            +--------------------+  !
    +---------------------------------------------------------------+
    

