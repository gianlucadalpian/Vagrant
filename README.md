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
    
   
   # Was habe ich gelernt?
   
   ## Github
   Ich durfte mich erstmal mit GitHub auseinandersetzen. Ich war zu beginn ein wenig verwirrt für was das zu gebrauchen ist, jedoch habe ich nun verstanden, das es ein Codeverwaltungstool ist, welches mir ermöglich meine Projekte in Repositorys abzuspeichern und zu verwalten. Ich finde es noch cool das man andere Projekte inspizieren kann und mit anderen Leuten teilen kann. Für mich persönlich werde ich keine verwendung dafür finden, jedoch ist es gut zu wissen das es so was gibt.
  
  ## Vagrant
  Vagrant ist eine Ruby-Anwendung welche es ermöglich Virtuelle Maschienen zu erstellen und zu verwalten. Ich wusste zuvor gar nicht das es eine solche Möglichkeit gibt, da ich im Berufsaltag noch nie mit sowas gearbeitet habe. Ich finde es erstaunlich wie aktuell die Betriebssysteme in der Vagrant Clound sind. Ich hatte zu beginn ein wenig Probleme mit der Strucktur vom Vagrantfile. Ich habe mich am anfang mit Hyper-V rumgeschlagen, nach dem ich die selbe Infrastruktur auf einer Virtualbox umgebung laufen lassen wollte, habe ich gemerkt dass es da komplikationen geben kann. Ich durfte viele verschiedene varianten von Vagrantbefehlen kennenlernen und werrde die in Zukunft bei gelegenheit auch anwenden. 

 ## Gitkraken
 Ich hatte in vergangenheit nie was zu tun mit Versionisirungen von Codierungen. Ich habe das Programm Gitkraken von einem Klassenkameraden empfohlen bekommen. Das Programm ist cool aufgebaut und man kann es sehr simpel mit GitHub verknüpfen. Die Versionisierung an sich habe ich zu wenig angewendet da dies für mich zu umständlich rübergekommen ist. Natürlich ist mir der Sinn und Nutzen der Versionisierung klar aber für meine Umgebung war es ein wenig zu umständlich.
 
## Visual Studio Code
Ich habe Visual Studio Code schon in Vergangenheit angewendet. Ich wusste auch das ich verschiedene Programmiersprachen installieren kann. Ich habe einmal mehr gemerkt, dass dieses Tool einfach perfekt ist um Code-Files zu beabeiten.

# Reflexion LB01
LB01 hat mich sehr interessiert und hat mir auch viel spass gemacht. Ich denke das dies eine sehr moderne Art ist, in der heutigen Informatik zu hantieren. Es war für mich sehr mühsam da ich viel mit Krankheiten zu kämpfen hatte eine solide Arbeit abzuliefern, ich denke jedoch das thema begriffen zu haben. Ich werde in Zukunft in meiner beruflichen Abreitsumgebung mit dieser Methode versuchen Projekte zu realisieren, da man mit einem Firmen standartisierten Vagrant File eine VM in wenigen sekunden erstellt hat. Die Verwaltung von VM ist in meinen Umgebungen nicht sehr Sinnvoll da wir viele verschiedene Umgebungen betreuen. Ich werde in Zukunft versuchen mehr mit Versionisierung zu arbeiten, aus dem Grund dass es besser nachzuvolziehen ist was man verändert hat. 

   
    

      
