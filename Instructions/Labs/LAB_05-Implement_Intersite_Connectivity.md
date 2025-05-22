---
lab:
  title: 'Lab 05: Implementieren von standortübergreifender Konnektivität'
  module: Administer Intersite Connectivity
---

# Übung 05 – Implementieren von Konnektivität zwischen Standorten

## Einführung

In diesem Lab wird die Kommunikation zwischen virtuellen Netzwerken behandelt. Sie implementieren das Peering virtueller Netzwerke sowie Testverbindungen. Außerdem wird hier eine benutzerdefinierte Route erstellt. 

Für dieses Lab wird ein Azure-Abonnement benötigt. Ihr Abonnementtyp kann sich auf die Verfügbarkeit von Features in diesem Lab auswirken. Die Region kann geändert werden. In den Schritten wird allerdings die Region **USA, Osten** verwendet. 

## Geschätzte Dauer: 50 Minuten
    
## Labszenario 

Ihre Organisation trennt grundlegende IT-Apps und -Dienste (z. B. DNS und Sicherheitsdienste) mittels Segmentierung von anderen Teilen des Unternehmens – unter anderem von Ihrer Fertigungsabteilung. In einigen Szenarien müssen Apps und Dienste im Kernbereich jedoch mit Apps und Diensten im Fertigungsbereich kommunizieren. In diesem Lab konfigurieren Sie die Konnektivität zwischen den segmentierten Bereichen. Dieses gängige Szenario dient dazu, die Produktion von der Entwicklung oder Tochtergesellschaften voneinander zu trennen.  

## Interaktive Labsimulation

Für dieses Thema stehen mehrere hilfreiche interaktive Labsimulationen zur Verfügung. In der Simulation können Sie sich in Ihrem eigenen Tempo durch ein ähnliches Szenario klicken. Es gibt zwar Unterschiede zwischen der interaktiven Simulation und diesem Lab, aber viele der Kernkonzepte sind identisch. Ein Azure-Abonnement ist nicht erforderlich. 

+ [Verbinden von zwei virtuellen Azure-Netzwerken mithilfe des globalen Peerings virtueller Netzwerke](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Connect%20two%20Azure%20virtual%20networks%20using%20global%20virtual%20network%20peering): Testen Sie die Verbindung zwischen zwei virtuellen Computern in verschiedenen virtuellen Netzwerken. Erstellen Sie ein Peering virtueller Netzwerke, und testen Sie erneut.

+ [Konfigurieren der Überwachung für virtuelle Netzwerke](https://learn.microsoft.com/training/modules/configure-monitoring-virtual-networks/). Erfahren Sie, wie Sie den Verbindungsmonitor von Azure Network Watcher, Datenflussprotokolle, die NSG-Diagnose und die Paketerfassung verwenden, um die Konnektivität über Ihre Azure IaaS-Netzwerkressourcen hinweg zu überwachen.

+ [Implementieren der standortübergreifenden Konnektivität](https://mslabs.cloudguides.com/en-us/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%209): Führen Sie eine Vorlage aus, um eine virtuelle Netzwerkinfrastruktur mit mehreren virtuellen Computern zu erstellen. Konfigurieren Sie Peerings für virtuelle Netzwerke, und testen Sie die Verbindungen. 

## Architekturdiagramm

![Lab 05: Architekturdiagramm](../media/az104-lab05-architecture.png)

## Stellenqualifikationen

+ Aufgabe 1: Erstellen eines virtuellen Computers in einem virtuellen Netzwerk
+ Aufgabe 2: Erstellen eines virtuellen Computers in einem anderen virtuellen Netzwerk
+ Aufgabe 3: Testen der Verbindung zwischen virtuellen Computern mithilfe von Network Watcher 
+ Aufgabe 4: Konfigurieren von Peerings virtueller Netzwerke zwischen verschiedenen virtuellen Netzwerken
+ Aufgabe 5: Testen der Verbindung zwischen virtuellen Computern mithilfe von Azure PowerShell
+ Aufgabe 6: Erstellen einer benutzerdefinierten Route 

## Aufgabe 1:  Erstellen eines virtuellen Computers und eines virtuellen Netzwerks für grundlegende Dienste

In dieser Aufgabe wird ein virtuelles Netzwerk mit einem virtuellen Computer für grundlegende Dienste erstellt. 

1. Melden Sie sich beim **Azure-Portal** - `https://portal.azure.com` an.

1. Suchen Sie nach `Virtual Machines`, und wählen Sie diese Option aus.

1. Wählen Sie auf der Seite für virtuelle Computer die Option **Erstellen** und anschließend **Virtueller Azure-Computer** aus.

1. Verwenden Sie die folgenden Informationen, um das Formular auf der Registerkarte „Grundeinstellungen“ auszufüllen, und wählen Sie anschließend die folgende Option aus: **Weiter: Datenträger >**. Behalten Sie bei allen Einstellungen, die nicht angegeben sind, den jeweiligen Standardwert bei.
 
    | Einstellung | Wert | 
    | --- | --- |
    | Abonnement |  *Ihr Abonnement* |
    | Ressourcengruppe |  `az104-rg5` (Wählen Sie bei Bedarf **Neu erstellen** aus. )
    | Name des virtuellen Computers |    `CoreServicesVM` |
    | Region | **(USA) USA, Osten** |
    | Verfügbarkeitsoptionen | Keine Infrastrukturredundanz erforderlich |
    | Sicherheitstyp | **Standard** |
    | Abbildung | **Windows Server 2019 Datacenter: x64 Gen2** (Beachten Sie die anderen verfügbaren Optionen.) |
    | Größe | **Standard_DS2_v3** |
    | Benutzername | `localadmin` | 
    | Kennwort | **Geben Sie ein komplexes Kennwort an.** |
    | Öffentliche Eingangsports | **Keine** |

    ![Screenshot: Seite mit den Grundeinstellungen für die VM-Erstellung ](../media/az104-lab05-createcorevm.png)
   
1. Übernehmen Sie auf der Registerkarte **Datenträger** die Standardwerte, und wählen Sie **Weiter: Netzwerk >** aus.

1. Wählen Sie auf der Registerkarte **Netzwerk** unter „Virtuelles Netzwerk“ die Option **Neu erstellen** aus.

1. Verwenden Sie die folgenden Informationen, um das virtuelle Netzwerk zu konfigurieren, und wählen Sie anschließend **OK** aus. Entfernen oder ersetzen Sie die vorhandenen Informationen bei Bedarf.

    | Einstellung | Wert | 
    | --- | --- |
    | Name | `CoreServicesVnet` (Neu erstellen) |
    | Adressbereich | `10.0.0.0/16`  |
    | Subnetzname | `Core` | 
    | Subnetzadressbereich | `10.0.0.0/24` |

1. Klicken Sie auf die Registerkarte **Überwachung**. Wählen Sie unter „Startdiagnose“ die Option **Deaktivieren** aus.

1. Klicken Sie auf **Review + Create** (Überprüfen und erstellen) und dann auf **Create** (Erstellen).

1. Sie müssen nicht warten, bis die Ressourcen erstellt wurden. Fahren Sie mit der nächsten Aufgabe fort.

    >**Hinweis:** In dieser Aufgabe wurde das virtuelle Netzwerk im Zuge der Erstellung des virtuellen Computers erstellt. Sie können aber auch die virtuelle Netzwerkinfrastruktur erstellen und dann die virtuellen Computer hinzufügen. 

## Aufgabe 2: Erstellen eines virtuellen Computers in einem anderen virtuellen Netzwerk

In dieser Aufgabe wird ein virtuelles Netzwerk mit einem virtuellen Computer für Fertigungsdienste erstellt. 

1. Suchen Sie im Azure-Portal nach **Virtuelle Computer**, und navigieren Sie dorthin.

1. Wählen Sie auf der Seite für virtuelle Computer die Option **Erstellen** und anschließend **Virtueller Azure-Computer** aus.

1. Verwenden Sie die folgenden Informationen, um das Formular auf der Registerkarte „Grundeinstellungen“ auszufüllen, und wählen Sie anschließend die folgende Option aus: **Weiter: Datenträger >**. Behalten Sie bei allen Einstellungen, die nicht angegeben sind, den jeweiligen Standardwert bei.
 
    | Einstellung | Wert | 
    | --- | --- |
    | Abonnement |  *Ihr Abonnement* |
    | Ressourcengruppe |  `az104-rg5` |
    | Name des virtuellen Computers |    `ManufacturingVM` |
    | Region | **(USA) USA, Osten** |
    | Sicherheitstyp | **Standard** |
    | Verfügbarkeitsoptionen | Keine Infrastrukturredundanz erforderlich |
    | Abbildung | **Windows Server 2019 Datacenter: x64 Gen2** |
    | Größe | **Standard_DS2_v3** | 
    | Benutzername | `localadmin` | 
    | Kennwort | **Geben Sie ein komplexes Kennwort an.** |
    | Öffentliche Eingangsports | **Keine** |

1. Übernehmen Sie auf der Registerkarte **Datenträger** die Standardwerte, und wählen Sie **Weiter: Netzwerk >** aus.

1. Wählen Sie auf der Registerkarte „Netzwerk“ unter „Virtuelles Netzwerk“ die Option **Neu erstellen** aus.

1. Verwenden Sie die folgenden Informationen, um das virtuelle Netzwerk zu konfigurieren, und wählen Sie anschließend **OK** aus.  Entfernen oder ersetzen Sie den vorhandenen Adressbereich bei Bedarf.

    | Einstellung | Wert | 
    | --- | --- |
    | Name | `ManufacturingVnet` |
    | Adressbereich | `172.16.0.0/16`  |
    | Subnetzname | `Manufacturing` |
    | Subnetzadressbereich | `172.16.0.0/24` |

1. Klicken Sie auf die Registerkarte **Überwachung**. Wählen Sie unter „Startdiagnose“ die Option **Deaktivieren** aus.

1. Klicken Sie auf **Review + Create** (Überprüfen und erstellen) und dann auf **Create** (Erstellen).

## Aufgabe 3: Testen der Verbindung zwischen virtuellen Computern mithilfe von Network Watcher 


In dieser Aufgabe wird überprüft, ob Ressourcen in mittels Peering verknüpften virtuellen Netzwerken miteinander kommunizieren können. Zum Testen der Verbindung wird Network Watcher verwendet. Vergewissern Sie sich, dass beide virtuellen Computer bereitgestellt wurden und ausgeführt werden, bevor Sie fortfahren. 

1. Suchen Sie im Azure-Portal nach `Network Watcher`, und wählen Sie die entsprechende Option aus.

1. Wählen Sie in Network Watcher im Menü mit den Netzwerkdiagnosetools die Option **Problembehandlung für Verbindung** aus.

1. Füllen Sie die Felder auf der Seite **Problembehandlung für Verbindung** mit den folgenden Informationen aus.

    | Feld | Wert | 
    | --- | --- |
    | Quelltyp           | **Virtueller Computer**   |
    | Virtueller Computer       | **CoreServicesVM**    | 
    | Zieltyp      | **Virtueller Computer**   |
    | Virtueller Computer       | **ManufacturingVM**   | 
    | Bevorzugte IP-Version  | **Beide**              | 
    | Protokoll              | **TCP**               |
    | Zielport      | `3389`                |  
    | Quellport           | *Leer*         |
    | Diagnosetests      | *Defaults*      |

    ![Azure-Portal mit Einstellungen für die Behandlung von Verbindungsproblemen](../media/az104-lab05-connection-troubleshoot.png)

1. Wählen Sie **Diagnosetests ausführen** aus.

    >**Hinweis:** Es kann einige Minuten dauern, bis die Ergebnisse zurückgegeben werden. Die Auswahloptionen auf dem Bildschirm werden abgeblendet, während die Ergebnisse gesammelt werden. Beachten Sie, dass für **Konnektivitätstest** der Wert **Nicht erreichbar** angezeigt wird. Das ist zu erwarten, da sich die virtuellen Computer in verschiedenen virtuellen Netzwerken befinden. 

 
## Aufgabe 4: Konfigurieren von Peerings virtueller Netzwerke zwischen virtuellen Netzwerken

In dieser Aufgabe wird ein Peering virtueller Netzwerke erstellt, um die Kommunikation zwischen Ressourcen in den virtuellen Netzwerken zu ermöglichen. 

1. Wählen Sie im Azure-Portal das virtuelle Netzwerk `CoreServicesVnet` aus.

1. Wählen Sie in „CoreServicesVnet“ unter **Einstellungen** die Option **Peerings** aus.

1. Wählen Sie in „CoreServicesVnet“ unter „Peerings“ die Option **+ Hinzufügen** aus. Wenn nichts angegeben ist, übernehmen Sie die Standardeinstellung. 

    | **Parameter**                                    | **Wert**                             |
    | --------------------------------------------- | ------------------------------------- |                                
    | Name des Peeringlinks                             | `CoreServicesVnet-to-ManufacturingVnet` |
    | Virtuelles Netzwerk    | **ManufacturingVM-net (az104-rg5)**  |
    | ManufacturingVNet den Zugriff auf CoreServicesVNet erlauben  | Aktiviert (Standardeinstellung) |
    | Zulassen, dass ManufacturingVNet weitergeleiteten Datenverkehr von CoreServicesVNet empfangen kann | Ausgewählt  |
    | Name des Peeringlinks                             | `ManufacturingVnet-to-CoreServicesVnet` |
    | Ermöglichen Sie CoreServicesVNet den Zugriff auf das mittels Peering verknüpfte virtuelle Netzwerk            | Aktiviert (Standardeinstellung) |
    | Lassen Sie für CoreServicesVNet den Empfang von weitergeleitetem Datenverkehr aus dem mittels Peering verknüpften virtuellen Netzwerk zu | Ausgewählt |

4. Klicken Sie auf **Hinzufügen**.

5. Überprüfen Sie in „CoreServicesVnet“ unter „Peerings“, ob das Peering **CoreServicesVnet-to-ManufacturingVnet** aufgeführt ist. Aktualisieren Sie die Seite, um sicherzustellen, dass für **Peeringstatus** der Wert **Verbunden** angezeigt wird.

6. Wechseln Sie zu **ManufacturingVnet**, und vergewissern Sie sich, dass das Peering **ManufacturingVnet-to-CoreServicesVnet** aufgeführt ist. Vergewissern Sie sich, dass unter **Peeringstatus** der Wert **Verbunden** angezeigt wird. Möglicherweise müssen Sie auf **Aktualisieren** klicken, um die Seite zu aktualisieren. 

## Aufgabe 5: Testen der Verbindung zwischen virtuellen Computern mithilfe von Azure PowerShell

In dieser Aufgabe wird die Verbindung zwischen den virtuellen Computern in verschiedenen virtuellen Netzwerken getestet. 

### Überprüfen der privaten IP-Adresse von CoreServicesVM

1. Suchen Sie im Azure-Portal nach dem virtuellen Computer `CoreServicesVM`, und wählen Sie ihn aus.

1. Notieren Sie sich die private IP-Adresse des Computers, die auf dem Blatt **Übersicht** im Abschnitt **Netzwerk** unter **Private IP-Adresse** angegeben ist. Diese Information wird zum Testen der Verbindung benötigt.
   
### Testen Sie die Verbindung mit CoreServicesVM von **ManufacturingVM** aus.

>**Schon gewusst?** Verbindungen können auf unterschiedliche Weise überprüft werden. In dieser Aufgabe wird **Befehl ausführen** verwendet. Sie können aber auch weiterhin Network Watcher verwenden. Oder verwenden Sie eine [Remotedesktopverbindung](https://learn.microsoft.com/azure/virtual-machines/windows/connect-rdp#connect-to-the-virtual-machine), um auf den virtuellen Computer zuzugreifen. Verwenden Sie nach der Verbindungsherstellung **test-connection**. Wenn Sie Zeit haben, probieren Sie RDP aus. 

1. Wechseln Sie zum virtuellen Computer `ManufacturingVM`.

1. Wählen Sie auf dem Blatt **Vorgänge** das Blatt **Befehl ausführen** aus.

1. Wählen Sie **RunPowerShellScript** aus, und führen Sie anschließend den Befehl **Test-NetConnection** aus. Achten Sie darauf, dass Sie die private IP-Adresse von **CoreServicesVM** verwenden.

    ```Powershell
    Test-NetConnection <CoreServicesVM private IP address> -port 3389
    ```
1. Es kann einige Minuten dauern, bis für das Skript ein Timeout auftritt. Oben auf der Seite wird die Informationsmeldung *Skript wird ausgeführt...* angezeigt.

   
1. Die Testverbindung sollte erfolgreich hergestellt werden, da Peering konfiguriert wurde. Computername und Remoteadresse unterscheiden sich in dieser Grafik ggf. von Ihren Werten. 
   
   ![PowerShell-Fenster mit erfolgreicher Ausführung von „Test-NetConnection“](../media/az104-lab05-success.png)

## Aufgabe 6: Erstellen einer benutzerdefinierten Route 

In dieser Aufgabe möchten Sie den Netzwerkdatenverkehr zwischen dem Umkreissubnetz und dem internen Subnetz der grundlegenden Dienste steuern. Im Subnetz der grundlegenden Dienste wird eine virtuelle Netzwerkappliance installiert, und der gesamte Datenverkehr soll dorthin weitergeleitet werden. 

1. Suchen Sie nach `CoreServicesVnet`, und wählen Sie die entsprechende Option aus.

1. Klicken Sie auf **Subnetze** und anschließend auf **+ Subnetz**. Vergewissern Sie sich, dass Sie **Hinzufügen** wählen, um Ihre Änderungen zu speichern. 

    | Einstellung | Wert | 
    | --- | --- |
    | Name | `perimeter` |
    | Subnetzadressbereich | `10.0.1.0/24`  |

   
1. Suchen Sie im Azure-Portal nach `Route tables`, wählen Sie die entsprechende Option aus, wählen Sie **Überprüfen + Erstellen** und anschließend **Erstellen** aus. 

    | Einstellung | Wert | 
    | --- | --- |
    | Abonnement | Ihr Abonnement |
    | Ressourcengruppe | `az104-rg5`  |
    | Region | **USA, Osten** |
    | Name | `rt-CoreServices` |
    | Gatewayrouten verteilen | **Nein** |

1. Nachdem die Routingtabelle bereitgestellt wurde, suchen Sie nach **Routingtabellen** und wählen diese aus.
   
1. Aktivieren der Ressource (nicht das Kontrollkästchen) **rt-CoreServices**

1. Erweitern Sie **Einstellungen** und wählen Sie **Routen** und dann **Hinzufügen** aus. Erstellen Sie eine Route von einer zukünftigen Network Virtual Appliance (NVA) zum virtuellen CoreServices-Netzwerk. 

    | Einstellung | Wert | 
    | --- | --- |
    | Routenname | `PerimetertoCore` |
    | Zieltyp | **IP-Adressen** |
    | Ziel-IP-Adressen | `10.0.0.0/16` (virtuelles Netzwerk für die grundlegenden Dienste) |
    | Typ des nächsten Hops | **Virtuelle Appliance** (Beachten Sie die anderen verfügbaren Optionen.) |
    | Adresse des nächsten Hops | `10.0.1.7` (zukünftige NVA) |

1. Wählen Sie **+ Hinzufügen**. Im letzten Schritt wird die Route mit dem Subnetz verknüpft.

1. Wählen Sie **Subnetze** und anschließend **+ Zuordnen** aus. Schließen Sie die Konfiguration ab.

    | Einstellung | Wert | 
    | --- | --- |
    | Virtuelles Netzwerk | **CoreServicesVnet** |
    | Subnetz | **Core** |    

>**Hinweis:** Sie haben eine benutzerdefinierte Route erstellt, um Datenverkehr von der DMZ zur neuen NVA zu leiten.  

## Bereinigen Ihrer Ressourcen

Wenn Sie mit **Ihrem eigenen Abonnement** arbeiten, nehmen Sie sich eine Minute Zeit, um die Labressourcen zu löschen. Dadurch wird sichergestellt, dass Ressourcen freigegeben und Kosten minimiert werden. Die einfachste Möglichkeit zum Löschen der Labressourcen besteht darin, die Ressourcengruppe des Labs zu löschen. 

+ Wählen Sie im Azure-Portal die Ressourcengruppe und dann **Ressourcengruppe löschen** aus, **geben Sie den Ressourcengruppennamen ein**, und klicken Sie dann auf **Löschen**.
+ Bei Verwendung von Azure PowerShell: `Remove-AzResourceGroup -Name resourceGroupName`.
+ Bei Verwendung der Befehlszeilenschnittstelle: `az group delete --name resourceGroupName`.

## Erweitern Ihrer Lernerfahrung mit Copilot
Copilot kann Sie beim Erlernen der Verwendung von Azure-Skripttools unterstützen. Copilot kann Sie auch in Bereichen unterstützen, die nicht im Lab behandelt werden oder in denen Sie weitere Informationen benötigen. Öffnen Sie einen Edge-Browser, und wählen Sie „Copilot“ (rechts oben) aus, oder navigieren Sie zu *copilot.microsoft.com*. Nehmen Sie sich einige Minuten Zeit, um diese Prompts auszuprobieren.

+ Wie kann ich Azure PowerShell- oder Azure CLI-Befehle verwenden, um ein Peering virtueller Netzwerke zwischen vnet1 und vnet2 einzurichten?
+ Erstelle eine Tabelle mit den verschiedenen Überwachungstools von Azure und Drittanbietern, die in Azure unterstützt werden. Gib dabei an, wann welches Tool verwendet werden sollte. 
+ Wann sollte ich in Azure eine benutzerdefinierte Netzwerkroute erstellen?

## Weiterlernen im eigenen Tempo

+ [Verteilen von Diensten in virtuellen Azure-Netzwerken und Integrieren dieser Dienste über Peering virtueller Netzwerke](https://learn.microsoft.com/en-us/training/modules/integrate-vnets-with-vnet-peering/): Mithilfe des Peerings virtueller Netzwerke können Sie eine sichere und einfache Kommunikation über virtuelle Netzwerke hinweg ermöglichen.
+ [Verwalten und Steuern des Datenverkehrsflusses mit Routen in Ihrer Azure-Bereitstellung](https://learn.microsoft.com/training/modules/control-network-traffic-flow-with-routes/): Hier erfahren Sie, wie der Netzwerkdatenverkehr von virtuellen Azure-Netzwerken mithilfe von benutzerdefinierten Routen gesteuert wird.


## Wichtige Erkenntnisse

Herzlichen Glückwunsch zum erfolgreichen Abschluss des Labs. Hier sind die wichtigsten Erkenntnisse für dieses Lab. 

+ Standardmäßig können Ressourcen in unterschiedlichen virtuellen Netzwerken nicht miteinander kommunizieren.
+ Durch das Peering virtueller Netzwerke können Sie zwei oder mehr virtuelle Netzwerke nahtlos in Azure verbinden.
+ Mittels Peering verknüpfte virtuelle Netzwerke werden für Verbindungszwecke als einzelnes Element angezeigt.
+ Der Datenverkehr zwischen virtuellen Computern in virtuellen Netzwerken mit Peering erfolgt über die Microsoft-Backboneinfrastruktur.
+ Für jedes Subnetz in einem virtuellen Netzwerk werden automatisch systemseitig definierte Routen erstellt. Benutzerdefinierte Routen setzen standardmäßige Systemrouten außer Kraft oder ergänzen sie. 
+ Azure Network Watcher bietet eine Reihe von Tools für die Überwachung, Diagnose und Anzeige von Metriken sowie Protokolle für Azure IaaS-Ressourcen.
