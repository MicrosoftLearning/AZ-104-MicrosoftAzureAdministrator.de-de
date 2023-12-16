---
demo:
  title: "Demo\_08: Verwalten von Azure Virtual Machines"
  module: Administer Azure Virtual Machines
---


# 08: Verwalten von Azure Virtual Machines

## Demo: Erstellen von virtuellen Computern im Portal

In dieser Demo erstellen wir im Portal einen virtuellen Azure-Computer und greifen darauf zu.

**Referenzen**

[Schnellstart: Erstellen eines virtuellen Windows-Computers im Azure-Portal](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal)

[Schnellstart: Erstellen eines virtuellen Linux-Computers im Azure-Portal](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-portal)

[Herstellen einer Verbindung mit einem virtuellem Computer mit Bastion](https://learn.microsoft.com/azure/bastion/tutorial-create-host-portal#connect)

**Erstellen des virtuellen Computers**

**Hinweis:** In diesen Schritten werden nur wenige Parameter für virtuelle Computer behandelt. Sie können auch andere Bereiche erkunden und erörtern. Je nach Zielgruppe können Sie virtuelle Windows- oder Linux-Computer erstellen.

1. Verwenden Sie dafür das Azure-Portal.

1. Suchen Sie nach **virtuellen Computern**. 

1. Erstellen Sie einen einfachen virtuellen Computer. Überprüfen Sie die Verfügbarkeitsoptionen, Images und eingehenden Regeln.

1. Besprechen Sie die Wichtigkeit der Erstellung eines sicheren Administratorkontos.

1. Erstellen Sie den virtuellen Computer, und warten Sie, bis die Ressource bereitgestellt wird.  

**Verbinden mit dem virtuellen Computer**

1. Es gibt mehrere Möglichkeiten, eine **Verbindung** mit dem virtuellen Computer herzustellen. 

1. Für einen Windows-Server können Sie **RDP** verwenden, wie im Schnellstart gezeigt. 

1. Für einen Linux-Server können Sie **SSH** verwenden, wie im Schnellstart gezeigt. 

1. Für beide Server können Sie eine Verbindung mit dem **Bastion-Dienst** herstellen (Schnellstart). Sehen Sie sich an, warum Bastion RDP oder SSH vorgezogen wird. 

## Konfigurieren der Verfügbarkeit von VMs

In dieser Demo untersuchen wir Skalierungsoptionen für virtuelle Computer.

**Referenzen**

[Erstellen von VMs in einer Skalierungsgruppe über das Azure-Portal](https://learn.microsoft.com/azure/virtual-machine-scale-sets/flexible-virtual-machine-scale-sets-portal)

1. Verwenden Sie das Azure-Portal.

1. Suchen Sie nach **Virtual Machine Scale Sets**, und wählen Sie den Dienst aus. 

1. Erstellen Sie eine **Virtual Machine Scale Sets-Instanz**. Erläutern Sie den Zweck von VM-Skalierungsgruppen. Sehen Sie sich die Unterschiede zwischen den Orchestrierungsmodi **Einheitlich** und **Flexibel** an. Erläutern Sie Ihre Auswahl, die sich auf Ihre Skalierungsoptionen auswirken kann. 

1. Wechseln Sie zur Registerkarte **Skalierung**. 

1. Überprüfen Sie, wie die **Manuelle Skalierung** und die **Richtlinie für horizontales Skalieren** verwendet werden. 

1. Wechseln Sie zur einer **benutzerdefinierten** Skalierungsrichtlinie. 

1. Überprüfen Sie, wie der **CPU-Schwellenwert (%)** zum Aufskalieren und Abskalieren in den Instanzen virtueller Computer verwendet wird. 

