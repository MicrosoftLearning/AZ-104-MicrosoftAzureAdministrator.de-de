---
lab:
  title: '06 : Implementieren von Datenverkehrsverwaltung'
  module: Module 06 - Network Traffic Management
ms.openlocfilehash: 6e082988d8b86ab4548171c24d6af6e7b004d06e
ms.sourcegitcommit: 0d47b9c4ded01643654314d8e615045c4e8692bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2022
ms.locfileid: "141588487"
---
# <a name="lab-06---implement-traffic-management"></a>Lab 06 : Implementieren von Datenverkehrsverwaltung
# <a name="student-lab-manual"></a>Lab-Handbuch für Kursteilnehmer

## <a name="lab-scenario"></a>Labszenario

Sie wurden damit beauftragt, die Verwaltung des Netzwerkdatenverkehrs für Azure-VMs in der Hub-and-Spoke-Netzwerktopologie zu testen, die Contoso in seiner Azure-Umgebung ggf. implementieren möchte (anstatt die Meshtopologie zu erstellen, die Sie im vorherigen Lab getestet haben). Diese Tests müssen die Implementierung der Konnektivität zwischen Spokes umfassen, indem benutzerdefinierte Routen verwendet werden, die den Datenverkehr über den Hub erzwingen, sowie die Verteilung des Datenverkehrs auf VMs mithilfe von Lastenausgleichsmodulen der Ebene 4 und der Ebene 7. Zu diesem Zweck möchten Sie Azure Load Balancer (Ebene 4) und Azure Application Gateway (Ebene 7) verwenden.

>**Hinweis**: Dieses Lab erfordert standardmäßig insgesamt 8 vCPUs, die in der Standard_Dsv3-Serie in der von Ihnen für die Bereitstellung ausgewählten Region verfügbar sind, da es die Bereitstellung von vier Azure-VMs der Standard_D2s_v3-SKU beinhaltet. Wenn Ihre Kursteilnehmer Testkonten verwenden, die auf 4 vCPUs beschränkt sind, können Sie eine VM-Größe verwenden, die nur eine vCPU erfordert (z. B. Standard_B1s).

## <a name="objectives"></a>Ziele

Dieses Lab deckt Folgendes ab:

+ Aufgabe 1: Bereitstellen der Laborumgebung
+ Aufgabe 2: Konfigurieren der Hub-and-Spoke-Netzwerktopologie
+ Aufgabe 3: Testen der Transitivität des Peerings virtueller Netzwerke
+ Aufgabe 4: Konfigurieren des Routings in der Hub-and-Spoke-Topologie
+ Aufgabe 5: Implementieren von Azure Load Balancer
+ Aufgabe 6: Implementieren von Azure Application Gateway

## <a name="estimated-timing-60-minutes"></a>Geschätzte Zeit: 60 Minuten

## <a name="architecture-diagram"></a>Architekturdiagramm

![image](../media/lab06.png)


## <a name="instructions"></a>Anweisungen

### <a name="exercise-1"></a>Übung 1

#### <a name="task-1-provision-the-lab-environment"></a>Aufgabe 1: Bereitstellen der Laborumgebung

In dieser Aufgabe stellen Sie vier VMs in derselben Azure-Region bereit. Die ersten beiden VMs befinden sich in einem virtuellen Hubnetzwerk, während sich jede der beiden verbleibenden VMs in einem separaten virtuellen Spokenetzwerk befindet.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Öffnen Sie **Azure Cloud Shell** im Azure-Portal, indem Sie oben rechts im Azure-Portal auf das entsprechende Symbol klicken.

1. Wenn Sie aufgefordert werden, entweder **Bash** oder **PowerShell** auszuwählen, wählen Sie **PowerShell** aus.

    >**Hinweis**: Wenn Sie **Cloud Shell** zum ersten Mal starten und die Meldung **Für Sie wurde kein Speicher bereitgestellt** angezeigt wird, wählen Sie das Abonnement aus, das Sie in diesem Lab verwenden, und klicken Sie dann auf **Speicher erstellen**.

1. Klicken Sie in der Symbolleiste des Cloud Shell-Bereichs auf das Symbol **Dateien hochladen/herunterladen**, klicken Sie im Dropdownmenü auf **Hochladen**, und laden Sie die Dateien **\\Allfiles\\Labs\\06\\az104-06-vms-loop-template.json** und **\\Allfiles\\Labs\\06\\az104-06-vms-loop-parameters.json** in das Cloud Shell-Basisverzeichnis hoch.

1. Bearbeiten Sie die **Parameterdatei**, die Sie gerade hochgeladen haben, und ändern Sie das Kennwort. Wenn Sie Hilfe bei der Bearbeitung der Datei in der Shell benötigen, bitten Sie Ihren Dozenten um Unterstützung. Als bewährte Methode sollten Geheimnisse, z. B. Kennwörter, sicherer in Key Vault gespeichert werden. 

1. Führen Sie im Cloud Shell-Bereich Folgendes aus, um die erste Ressourcengruppe zu erstellen, in der die Labumgebung gehostet wird (ersetzen Sie den Platzhalter [Azure_region] durch den Namen einer Azure-Region, in der Sie Azure-VMs bereitstellen möchten) (Sie können mithilfe des Cmdlets (Get-AzLocation).Location die Regionsliste abrufen):

    ```powershell 
    $location = '[Azure_region]'
    ```
    
    Nun der Name der Ressourcengruppe:
    ```powershell
    $rgName = 'az104-06-rg1'
    ```
    
    Erstellen Sie schließlich die Ressourcengruppe am gewünschten Speicherort:
    ```powershell
    New-AzResourceGroup -Name $rgName -Location $location
    ```


1. Führen Sie im Cloud-Shell Bereich Folgendes aus, um die drei virtuellen Netzwerke und vier Azure-VMs in ihnen mithilfe der hochgeladenen Vorlagen- und Parameterdateien zu erstellen:

   ```powershell
   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-06-vms-loop-template.json `
      -TemplateParameterFile $HOME/az104-06-vms-loop-parameters.json
   ```

    >**Hinweis**: Warten Sie, bis die Bereitstellung abgeschlossen ist, bevor Sie mit dem nächsten Schritt fortfahren. Dieser Vorgang dauert etwa fünf Minuten.

1. Führen Sie im Cloud Shell-Bereich Folgendes aus, um die Erweiterung Network Watcher auf den im vorherigen Schritt bereitgestellten Azure-VMs zu installieren:

   ```powershell
   $rgName = 'az104-06-rg1'
   $location = (Get-AzResourceGroup -ResourceGroupName $rgName).location
   $vmNames = (Get-AzVM -ResourceGroupName $rgName).Name

   foreach ($vmName in $vmNames) {
     Set-AzVMExtension `
     -ResourceGroupName $rgName `
     -Location $location `
     -VMName $vmName `
     -Name 'networkWatcherAgent' `
     -Publisher 'Microsoft.Azure.NetworkWatcher' `
     -Type 'NetworkWatcherAgentWindows' `
     -TypeHandlerVersion '1.4'
   }
   ```

    >**Hinweis**: Warten Sie, bis die Bereitstellung abgeschlossen ist, bevor Sie mit dem nächsten Schritt fortfahren. Dieser Vorgang dauert etwa fünf Minuten.

1. Schließen Sie den Cloud Shell-Bereich.

#### <a name="task-2-configure-the-hub-and-spoke-network-topology"></a>Aufgabe 2: Konfigurieren der Hub-and-Spoke-Netzwerktopologie

In dieser Aufgabe konfigurieren Sie lokales Peering zwischen den virtuellen Netzwerken, die Sie in den vorherigen Aufgaben bereitgestellt haben, um eine Hub-and-Spoke-Netzwerktopologie zu erstellen.

1. Suchen Sie im Azure-Portal nach der Option **Virtuelle Netzwerke** und wählen Sie sie aus.

1. Überprüfen Sie die virtuellen Netzwerke, die Sie in der vorherigen Aufgabe erstellt haben.

    >**Hinweis**: Die Vorlage, die Sie für die Bereitstellung der drei virtuellen Netzwerke verwendet haben, stellt sicher, dass sich die IP-Adressbereiche der drei virtuellen Netzwerke nicht überschneiden.

1. Wählen Sie in der Liste der virtuellen Netzwerke **az104-06-vnet2** aus.

1. Wählen Sie auf dem Blatt **az104-06-vnet2** die Option **Eigenschaften** aus. 

1. Notieren Sie sich aus dem Blatt **az104-06-vnet2 \| Eigenschaften** den Wert der Eigenschaft **Ressourcen-ID**.

1. Navigieren Sie zurück zur Liste der virtuellen Netzwerke, und wählen Sie **az104-06-vnet3** aus.

1. Wählen Sie auf dem Blatt **az104-06-vnet3** die Option **Eigenschaften** aus. 

1. Notieren Sie sich aus dem Blatt **az104-06-vnet3 \| Eigenschaften** den Wert der Eigenschaft **Ressourcen-ID**.

    >**Hinweis**: Sie benötigen die Werte der ResourceID-Eigenschaft für beide virtuellen Netzwerke später in dieser Aufgabe.

    >**Hinweis**: Dies ist eine Problemumgehung, die das Problem behebt, dass das Azure-Portal gelegentlich das neu bereitgestellte virtuelle Netzwerk nicht anzeigt, wenn Peerings virtueller Netzwerke erstellt werden.

1. Klicken Sie in der Liste der virtuellen Netzwerke auf **az104-06-vnet01**.

1. Klicken Sie auf dem Blatt des virtuellen Netzwerks **az104-06-vnet01** im Abschnitt **Einstellungen** auf **Peerings** und dann auf **+ Hinzufügen**.

1. Fügen Sie ein Peering mit den folgenden Einstellungen hinzu (übernehmen Sie für andere Einstellungen die Standardwerte), und klicken Sie auf **Hinzufügen**:

    | Einstellung | Wert |
    | --- | --- |
    | Dieses virtuelle Netzwerk: Name des Peeringlinks | **az104-06-vnet01_to_az104-06-vnet2** |
    | Datenverkehr zum virtuellen Remotenetzwerk | **Zulassen (Standard)** |
    | Traffic forwarded from remote virtual network (Vom virtuellen Remotenetzwerk weitergeleiteter Datenverkehr) | **Von außerhalb dieses virtuellen Netzwerks stammenden Datenverkehr blockieren** |
    | Gateway des virtuellen Netzwerks | **Keine (Standard)** |
    | Virtuelles Remotenetzwerk: Name des Peeringlinks | **az104-06-vnet2_to_az104-06-vnet01** |
    | Bereitstellungsmodell für das virtuelle Netzwerk | **Resource Manager** |
    | Ich kenne meine Ressourcen-ID | enabled |
    | Ressourcen-ID | Der Wert des resourceID-Parameters von **az104-06-vnet2**, den Sie sich zuvor in dieser Aufgabe notiert haben. |
    | Datenverkehr zum virtuellen Remotenetzwerk | **Zulassen (Standard)** |
    | Traffic forwarded from remote virtual network (Vom virtuellen Remotenetzwerk weitergeleiteter Datenverkehr) | **Zulassen (Standard)** |
    | Gateway des virtuellen Netzwerks | **Keine (Standard)** |

    >**Hinweis**: Warten Sie, bis der Vorgang abgeschlossen wurde.

    >**Hinweis**: In diesem Schritt werden zwei lokale Peerings eingerichtet: eines von az104-06-vnet01 bis az104-06-vnet2 und das andere von az104-06-vnet2 bis az104-06-vnet01.

    >**Hinweis**: **Weitergeleiteten Datenverkehr zulassen** muss aktiviert sein, um das Routing zwischen virtuellen Spokenetzwerken zu ermöglichen, das Sie später in diesem Lab implementieren werden.

1. Klicken Sie auf dem Blatt des virtuellen Netzwerks **az104-06-vnet01** im Abschnitt **Einstellungen** auf **Peerings** und dann auf **+ Hinzufügen**.

1. Fügen Sie ein Peering mit den folgenden Einstellungen hinzu (übernehmen Sie für andere Einstellungen die Standardwerte), und klicken Sie auf **Hinzufügen**:

    | Einstellung | Wert |
    | --- | --- |
    | Dieses virtuelle Netzwerk: Name des Peeringlinks | **az104-06-vnet01_to_az104-06-vnet3** |
    | Datenverkehr zum virtuellen Remotenetzwerk | **Zulassen (Standard)** |
    | Traffic forwarded from remote virtual network (Vom virtuellen Remotenetzwerk weitergeleiteter Datenverkehr) | **Von außerhalb dieses virtuellen Netzwerks stammenden Datenverkehr blockieren** |
    | Gateway des virtuellen Netzwerks | **Keine (Standard)** |
    | Virtuelles Remotenetzwerk: Name des Peeringlinks | **az104-06-vnet3_to_az104-06-vnet01** |
    | Bereitstellungsmodell für das virtuelle Netzwerk | **Resource Manager** |
    | Ich kenne meine Ressourcen-ID | enabled |
    | Ressourcen-ID | Der Wert des resourceID-Parameters von **az104-06-vnet3**, den Sie sich zuvor in dieser Aufgabe notiert haben. |
    | Datenverkehr zum virtuellen Remotenetzwerk | **Zulassen (Standard)** |
    | Traffic forwarded from remote virtual network (Vom virtuellen Remotenetzwerk weitergeleiteter Datenverkehr) | **Zulassen (Standard)** |
    | Gateway des virtuellen Netzwerks | **Keine (Standard)** |

    >**Hinweis**: In diesem Schritt werden zwei lokale Peerings eingerichtet: eines von az104-06-vnet01 bis az104-06-vnet3 und das andere von az104-06-vnet3 bis az104-06-vnet01. Dies schließt die Einrichtung der Hub-and-Spoke-Topologie (mit zwei virtuellen Spokenetzwerken) ab.

    >**Hinweis**: **Weitergeleiteten Datenverkehr zulassen** muss aktiviert sein, um das Routing zwischen virtuellen Spokenetzwerken zu ermöglichen, das Sie später in diesem Lab implementieren werden.

#### <a name="task-3-test-transitivity-of-virtual-network-peering"></a>Aufgabe 3: Testen der Transitivität des Peerings virtueller Netzwerke

In dieser Aufgabe testen Sie die Transitivität des Peerings virtueller Netzwerke mithilfe von Network Watcher.

1. Suchen Sie im Azure-Portal nach dem **Network Watcher**, und wählen Sie diese Option aus.

1. Erweitern Sie auf dem Blatt **Network Watcher** die Liste der Azure-Regionen. Prüfen Sie, ob der Dienst in der von Ihnen genutzten Region aktiviert ist. 

1. Navigieren Sie auf dem Blatt **Network Watcher** zu **Problembehandlung für Verbindung**.

1. Führen Sie auf dem Blatt **Network Watcher - Problembehandlung für Verbindung** eine Überprüfung mit den folgenden Einstellungen aus (übernehmen Sie die Standardwerte für andere Einstellungen):

    | Einstellung | Wert |
    | --- | --- |
    | Subscription | Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden |
    | Resource group | **az104-06-rg1** |
    | Quellentyp | **Virtueller Computer** |
    | Virtueller Computer | **az104-06-vm0** |
    | Destination | **Manuell angeben** |
    | URI, FQDN oder IPv4 | **10.62.0.4** |
    | Protokoll | **TCP** |
    | Zielport | **3389** |

    > **Hinweis**: **10.62.0.4** stellt die private IP-Adresse von **az104-06-vm2** dar.

1. Klicken Sie auf **Überprüfen**, und warten Sie, bis Ergebnisse der Konnektivitätsprüfung zurückgegeben werden. Vergewissern Sie sich, dass der Status als **Erreichbar** angegeben wird. Überprüfen Sie den Netzwerkpfad, und beachten Sie, dass die Verbindung direkt war, ohne Zwischenhops zwischen den VMs.

    > **Hinweis**: Dies ist zu erwarten, da das virtuelle Hubnetzwerk direkt mithilfe von Peering mit dem ersten virtuellen Spokenetzwerk verbunden wird.

1. Führen Sie auf dem Blatt **Network Watcher - Problembehandlung für Verbindung** eine Überprüfung mit den folgenden Einstellungen aus (übernehmen Sie die Standardwerte für andere Einstellungen):

    | Einstellung | Wert |
    | --- | --- |
    | Subscription | Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden |
    | Resource group | **az104-06-rg1** |
    | Quellentyp | **Virtueller Computer** |
    | Virtueller Computer | **az104-06-vm0** |
    | Destination | **Manuell angeben** |
    | URI, FQDN oder IPv4 | **10.63.0.4** |
    | Protokoll | **TCP** |
    | Zielport | **3389** |

    > **Hinweis**: **10.63.0.4** stellt die private IP-Adresse von **az104-06-vm3** dar.

1. Klicken Sie auf **Überprüfen**, und warten Sie, bis Ergebnisse der Konnektivitätsprüfung zurückgegeben werden. Vergewissern Sie sich, dass der Status als **Erreichbar** angegeben wird. Überprüfen Sie den Netzwerkpfad, und beachten Sie, dass die Verbindung direkt war, ohne Zwischenhops zwischen den VMs.

    > **Hinweis**: Dies ist zu erwarten, da das virtuelle Hubnetzwerk direkt mithilfe von Peering mit dem zweiten virtuellen Spokenetzwerk verbunden wird.

1. Führen Sie auf dem Blatt **Network Watcher - Problembehandlung für Verbindung** eine Überprüfung mit den folgenden Einstellungen aus (übernehmen Sie die Standardwerte für andere Einstellungen):

    | Einstellung | Wert |
    | --- | --- |
    | Subscription | Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden |
    | Resource group | **az104-06-rg1** |
    | Quellentyp | **Virtueller Computer** |
    | Virtueller Computer | **az104-06-vm2** |
    | Destination | **Manuell angeben** |
    | URI, FQDN oder IPv4 | **10.63.0.4** |
    | Protokoll | **TCP** |
    | Zielport | **3389** |

1. Klicken Sie auf **Überprüfen**, und warten Sie, bis Ergebnisse der Konnektivitätsprüfung zurückgegeben werden. Beachten Sie, dass der Status als **Nicht erreichbar** angegeben wird.

    > **Hinweis**: Dies ist zu erwarten, da die beiden virtuellen Spokenetzwerke nicht mithilfe von Peering miteinander verbunden sind (Peering virtueller Netzwerke ist nicht transitiv).

#### <a name="task-4-configure-routing-in-the-hub-and-spoke-topology"></a>Aufgabe 4: Konfigurieren des Routings in der Hub-and-Spoke-Topologie

In dieser Aufgabe konfigurieren und testen Sie das Routing zwischen den beiden virtuellen Spokenetzwerken, indem Sie IP-Weiterleitung für die Netzwerkschnittstelle der VM **az104-06-vm0** aktivieren, Routing innerhalb ihres Betriebssystems aktivieren und benutzerdefinierte Routen im virtuellen Spokenetzwerk konfigurieren.

1. Suchen Sie im Azure-Portal nach **Virtuelle Computer**, und wählen Sie diese Option aus.

1. Klicken Sie auf dem Blatt **Virtuelle Computer** in der Liste der VMs auf **az104-06-vm0**.

1. Klicken Sie auf dem Blatt der VM **az104-06-vm0** im Abschnitt **Einstellungen** auf **Netzwerk**.

1. Klicken Sie auf den Link **az104-06-nic0** neben der Bezeichnung **Netzwerkschnittstelle**, und klicken Sie dann auf dem Blatt der Netzwerkschnittstelle **az104-06-nic0** im Abschnitt **Einstellungen** auf **IP-Konfigurationen**.

1. Legen Sie **IP-Weiterleitung** auf **Aktiviert** fest, und speichern Sie die Änderung.

   > **Hinweis**: Diese Einstellung ist erforderlich, damit **az104-06-vm0** als Router funktioniert, der Datenverkehr zwischen zwei virtuellen Spokenetzwerken weiterleitet.

   > **Hinweis**: Nun müssen Sie das Betriebssystem der VM **az104-06-vm0** konfigurieren, um Routing zu unterstützen.

1. Navigieren Sie im Azure-Portal zurück zum Blatt der Azure-VM **az104-06-vm0**, und klicken Sie auf **Übersicht.**

1. Klicken Sie auf dem Blatt **az104-06-vm0** im Abschnitt **Vorgänge** auf **Skriptausführung**, und klicken Sie in der Liste der Befehle **auf RunPowerShellScript**.

1. Geben Sie auf dem Blatt **Skriptausführung** Folgendes ein, und klicken Sie auf **Ausführen**, um die Windows Server-Rolle „Remotezugriff“ zu installieren.

   ```powershell
   Install-WindowsFeature RemoteAccess -IncludeManagementTools
   ```

   > **Hinweis**: Warten Sie auf die Bestätigung, dass der Befehl erfolgreich abgeschlossen wurde.

1. Geben Sie auf dem Blatt **Skriptausführung** Folgendes ein, und klicken Sie auf **Ausführen**, um den die Rolle „Routing“ zu installieren.

   ```powershell
   Install-WindowsFeature -Name Routing -IncludeManagementTools -IncludeAllSubFeature

   Install-WindowsFeature -Name "RSAT-RemoteAccess-Powershell"

   Install-RemoteAccess -VpnType RoutingOnly

   Get-NetAdapter | Set-NetIPInterface -Forwarding Enabled
   ```

   > **Hinweis**: Warten Sie auf die Bestätigung, dass der Befehl erfolgreich abgeschlossen wurde.

   > **Hinweis**: Jetzt müssen Sie benutzerdefinierte Routen in den virtuellen Spokenetzwerken erstellen und konfigurieren.

1. Suchen Sie im Azure-Portal nach **Routentabellen**, und wählen Sie diese Option aus. Klicken Sie dann auf dem Blatt **Routentabellen** auf **+ Erstellen**.

1. Erstellen Sie eine Routentabelle mit den folgenden Einstellungen (übernehmen Sie für andere Einstellungen die Standardwerte):

    | Einstellung | Wert |
    | --- | --- |
    | Subscription | Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden |
    | Resource group | **az104-06-rg1** |
    | Location | Der Name der Azure-Region, in der Sie die virtuellen Netzwerke erstellt haben. |
    | Name | **az104-06-rt23** |
    | Gatewayrouten verteilen | **Nein** |

1. Klicken Sie auf **Überprüfen und erstellen**. Führen Sie die Überprüfung aus, und klicken Sie auf **Erstellen**, um Ihre Bereitstellung zu übermitteln.

   > **Hinweis**: Warten Sie, bis die Routentabelle erstellt wurde. Dieser Vorgang dauert etwa drei Minuten.

1. Klicken Sie auf **Zu Ressource wechseln**.

1. Klicken Sie auf dem Blatt der Routentabelle **az104-06-rt23** im Abschnitt **Einstellungen** auf **Routen** und dann auf **+ Hinzufügen**.

1. Fügen Sie eine neue Route mit den folgenden Einstellungen hinzu:

    | Einstellung | Wert |
    | --- | --- |
    | Routenname | **az104-06-route-vnet2-to-vnet3** |
    | Adresspräfix | **10.63.0.0/20** |
    | Typ des nächsten Hops | **Virtuelles Gerät** |
    | Adresse des nächsten Hops | **10.60.0.4** |

1. Klicken Sie auf **OK**

1. Klicken Sie auf dem Blatt der Routentabelle **az104-06-rt23** im Abschnitt **Einstellungen** auf **Subnetze** und dann auf **+ Zuordnen**.

1. Ordnen Sie die Routentabelle **az104-06-rt23** dem folgenden Subnetz zu:

    | Einstellung | Wert |
    | --- | --- |
    | Virtuelles Netzwerk | **az104-06-vnet2** |
    | Subnet | **subnet0** |

1. Klicken Sie auf **OK**

1. Navigieren Sie zurück zum Blatt **Routentabellen**, und klicken Sie auf **+ Erstellen**.

1. Erstellen Sie eine Routentabelle mit den folgenden Einstellungen (übernehmen Sie für andere Einstellungen die Standardwerte):

    | Einstellung | Wert |
    | --- | --- |
    | Subscription | Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden |
    | Resource group | **az104-06-rg1** |
    | Region | Der Name der Azure-Region, in der Sie die virtuellen Netzwerke erstellt haben. |
    | Name | **az104-06-rt32** |
    | Gatewayrouten verteilen | **Nein** |

1. Klicken Sie auf „Überprüfen und erstellen“. Führen Sie die Überprüfung aus, und klicken Sie auf „Erstellen“, um Ihre Bereitstellung zu übermitteln.

   > **Hinweis**: Warten Sie, bis die Routentabelle erstellt wurde. Dieser Vorgang dauert etwa drei Minuten.

1. Klicken Sie auf **Zu Ressource wechseln**.

1. Klicken Sie auf dem Blatt der Routentabelle **az104-06-rt32** im Abschnitt **Einstellungen** auf **Routen** und dann auf **+ Hinzufügen**.

1. Fügen Sie eine neue Route mit den folgenden Einstellungen hinzu:

    | Einstellung | Wert |
    | --- | --- |
    | Routenname | **az104-06-route-vnet3-to-vnet2** |
    | Adresspräfix | **10.62.0.0/20** |
    | Typ des nächsten Hops | **Virtuelles Gerät** |
    | Adresse des nächsten Hops | **10.60.0.4** |

1. Klicken Sie auf **OK**

1. Klicken Sie auf dem Blatt der Routentabelle **az104-06-rt32** im Abschnitt **Einstellungen** auf **Subnetze** und dann auf **+ Zuordnen**.

1. Ordnen Sie die Routentabelle **az104-06-rt32** dem folgenden Subnetz zu:

    | Einstellung | Wert |
    | --- | --- |
    | Virtuelles Netzwerk | **az104-06-vnet3** |
    | Subnet | **subnet0** |

1. Klicken Sie auf **OK**

1. Navigieren Sie im Azure-Portal zurück zum Blatt **Network Watcher - Problembehandlung für Verbindung**.

1. Führen Sie auf dem Blatt **Network Watcher - Problembehandlung für Verbindung** eine Überprüfung mit den folgenden Einstellungen aus (übernehmen Sie die Standardwerte für andere Einstellungen):

    | Einstellung | Wert |
    | --- | --- |
    | Subscription | Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden |
    | Resource group | **az104-06-rg1** |
    | Quellentyp | **Virtueller Computer** |
    | Virtueller Computer | **az104-06-vm2** |
    | Destination | **Manuell angeben** |
    | URI, FQDN oder IPv4 | **10.63.0.4** |
    | Protokoll | **TCP** |
    | Zielport | **3389** |

1. Klicken Sie auf **Überprüfen**, und warten Sie, bis Ergebnisse der Konnektivitätsprüfung zurückgegeben werden. Vergewissern Sie sich, dass der Status als **Erreichbar** angegeben wird. Überprüfen Sie den Netzwerkpfad, und beachten Sie, dass der Datenverkehr über **10.60.0.4** weitergeleitet wurde (Netzwerkadapter **az104-06-nic0** zugewiesen). Wenn der Status **Nicht erreichbar** lautet, sollten Sie „az104-06-vm0“ beenden und dann neu starten.

    > **Hinweis**: Dies ist zu erwarten, da der Datenverkehr zwischen virtuellen Spokenetzwerken jetzt über die VM im virtuellen Hubnetzwerk weitergeleitet wird, der als Router fungiert.

    > **Hinweis**: Sie können **Network Watcher** verwenden, um die Topologie des Netzwerks anzuzeigen.

#### <a name="task-5-implement-azure-load-balancer"></a>Aufgabe 5: Implementieren von Azure Load Balancer

In dieser Aufgabe implementieren Sie einen Azure Load Balancer vor den beiden Azure-VMs im virtuellen Hubnetzwerk.

1. Suchen Sie im Azure-Portal nach **Lastenausgleichsmodule**, und wählen Sie diese Option aus. Klicken Sie auf dem Blatt **Lastenausgleich** auf **+ Erstellen**.

1. Erstellen Sie einen Lastenausgleich mit den folgenden Einstellungen (übernehmen Sie für andere Einstellungen die Standardwerte), und klicken Sie auf **Weiter : Front-End-IP-Konfiguration**:

    | Einstellung | Wert |
    | --- | --- |
    | Subscription | Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden |
    | Resource group | **az104-06-rg1** |
    | Name | **az104-06-lb4** |
    | Region| Der Name der Azure-Region, in der Sie alle anderen Ressourcen in diesem Lab bereitgestellt haben. |
    | SKU | **Standard** |
    | Typ | **Public** |
    
1. Klicken Sie auf der Registerkarte **Front-End-IP-Konfiguration** und dann auf **Front-End-IP-Konfiguration hinzufügen**. Wählen Sie die folgende Einstellung, ehe Sie auf **Hinzufügen** klicken.   
     
    | Einstellung | Wert |
    | --- | --- |
    | Name | Ein beliebiger eindeutiger Name |
    | Öffentliche IP-Adresse | **Neu erstellen** |
    | Name der öffentlichen IP-Adresse | **az104-06-pip4** |

1. Klicken Sie auf **Überprüfen und erstellen**. Führen Sie die Überprüfung aus, und klicken Sie auf **Erstellen**, um Ihre Bereitstellung zu übermitteln.

    > **Hinweis**: Warten Sie, bis der Azure Load Balancer bereitgestellt wurde. Dieser Vorgang dauert etwa zwei Minuten.

1. Klicken Sie auf dem Blatt „Bereitstellung“ auf **Zu Ressource wechseln**.

1. Klicken Sie auf dem Blatt des Lastenausgleichs **az104-06-lb4** im Abschnitt **Einstellungen** auf **Back-End-Pools** und dann auf **+ Hinzufügen**.

1. Fügen Sie den Back-End-Pool mit den folgenden Einstellungen hinzu (übernehmen Sie für andere Einstellungen die Standardwerte):

    | Einstellung | Wert |
    | --- | --- |
    | Name | **az104-06-lb4-be1** |
    | Virtuelles Netzwerk | **az104-06-vnet01** |
    | IP-Version | **IPv4** |
    | Virtueller Computer | **az104-06-vm0** |
    | IP-Adresse der VM | **ipconfig1 (10.60.0.4)** |
    | Virtueller Computer | **az104-06-vm1** |
    | IP-Adresse der VM | **ipconfig1 (10.60.1.4)** |

1. Klicken Sie auf **Hinzufügen**.

1. Warten Sie, bis der Back-End-Pool erstellt wurde. Klicken Sie im Abschnitt **Einstellungen** auf **Integritätstest**, und klicken Sie dann auf **+ Hinzufügen**.

1. Fügen Sie einen Integritätstest mit den folgenden Einstellungen hinzu:

    | Einstellung | Wert |
    | --- | --- |
    | Name | **az104-06-lb4-hp1** |
    | Protokoll | **TCP** |
    | Port | **80** |
    | Intervall | **5** |
    | Fehlerhafter Schwellenwert | **2** |

1. Klicken Sie auf **Hinzufügen**.

1. Warten Sie, bis der Integritätstest erstellt wurde. Klicken Sie im Abschnitt **Einstellungen** auf **Lastenausgleichsregeln**, und klicken Sie dann auf **+ Hinzufügen**.

1. Fügen Sie eine Lastenausgleichsregel mit den folgenden Einstellungen hinzu (übernehmen Sie für andere Einstellungen die Standardwerte):

    | Einstellung | Wert |
    | --- | --- |
    | Name | **az104-06-lb4-lbrule1** |
    | IP-Version | **IPv4** |
    | Front-End-IP-Adresse | **Auswählen von LoadBalancerFrontEnd aus der Dropdownliste**
    | Protokoll | **TCP** |
    | Port | **80** |
    | Back-End-Port | **80** |
    | Back-End-Pool | **az104-06-lb4-be1** |
    | Integritätstest | **az104-06-lb4-hp1** |
    | Sitzungspersistenz | **None** |
    | Leerlaufzeitüberschreitung (Minuten) | **4** |
    | TCP-Zurücksetzung | **Disabled** |
    | Floating IP (Direct Server Return) | **Disabled** |

1. Klicken Sie auf **Hinzufügen**.

1. Warten Sie, bis die Lastenausgleichsregel erstellt wurde. Klicken Sie im Abschnitt **Einstellungen** auf **Front-End-IP-Konfiguration**, und notieren Sie sich den Wert für **Öffentliche IP-Adresse**.

1. Öffnen Sie ein weiteres Browserfenster, und navigieren Sie zu der IP-Adresse, die Sie im vorherigen Schritt identifiziert haben.

1. Vergewissern Sie sich, dass im Browserfenster die Meldung **Hello World from az104-06-vm0** oder **Hello World from az104-06-vm1** angezeigt wird.

1. Öffnen Sie ein weiteres Browserfenster, verwenden Sie dieses Mal jedoch den InPrivate-Modus, und überprüfen Sie, ob sich die Ziel-VM ändert (wie in der Meldung angegeben).

    > **Hinweis**: Möglicherweise müssen Sie das Browserfenster aktualisieren oder im InPrivate-Modus erneut öffnen.

#### <a name="task-6-implement-azure-application-gateway"></a>Aufgabe 6: Implementieren von Azure Application Gateway

In dieser Aufgabe implementieren Sie eine Azure Application Gateway-Instanz vor den beiden Azure-VMs im virtuellen Spokenetzwerk.

1. Suchen Sie im Azure-Portal nach **Virtuelle Netzwerke**, und wählen Sie diese Option aus.

1. Klicken Sie auf dem Blatt **Virtuelle Netzwerke** in der Liste der virtuellen Netzwerke auf **az104-06-vnet01**.

1. Klicken Sie auf dem Blatt des virtuellen Netzwerks **az104-06-vnet01** im Abschnitt **Einstellungen** auf **Subnetze** und dann auf **+ Subnetz**.

1. Fügen Sie ein Subnetz mit den folgenden Einstellungen hinzu (übernehmen Sie für andere Einstellungen die Standardwerte):

    | Einstellung | Wert |
    | --- | --- |
    | Name | **subnet-appgw** |
    | Subnetzadressbereich | **10.60.3.224/27** |

1. Klicken Sie unten auf der Seite auf **Speichern**.

    > **Hinweis**: Dieses Subnetz wird von den Azure Application Gateway-Instanzen verwendet, die Sie später in dieser Aufgabe bereitstellen. Das Application Gateway erfordert ein dediziertes Subnetz der Größe /27 oder höher.

1. Suchen Sie im Azure-Portal nach **Application Gateways**, und wählen Sie diese Option aus. Klicken Sie auf dem Blatt **Application Gateways** auf **+ Erstellen**.

1. Geben Sie auf der Registerkarte **Grundeinstellungen** des Blatts **Anwendungsgateway erstellen** die folgenden Einstellungen an (übernehmen Sie die Standardwerte für andere Einstellungen):

    | Einstellung | Wert |
    | --- | --- |
    | Subscription | Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden |
    | Resource group | **az104-06-rg1** |
    | Name des Anwendungsgateways | **az104-06-appgw5** |
    | Region | Der Name der Azure-Region, in der Sie alle anderen Ressourcen in diesem Lab bereitgestellt haben. |
    | Tarif | **Standard V2** |
    | Aktivieren der automatischen Skalierung | **Nein** |
    | HTTP2 | **Disabled** |
    | Virtuelles Netzwerk | **az104-06-vnet01** |
    | Subnet | **subnet-appgw** |

1. Klicken Sie auf **Weiter: Front-Ends >** , und klicken Sie auf der Registerkarte **Front-Ends** des Blatts **Anwendungsgateway erstellen** auf **Neu hinzufügen**. Geben Sie die folgenden Einstellungen an (übernehmen Sie die Standardwerte für andere Einstellungen):

    | Einstellung | Wert |
    | --- | --- |
    | Typ der Front-End-IP-Adresse | **Public** |
    | Öffentliche IP-Adresse| Der Name einer neuen öffentlichen IP-Adresse **az104-06-pip5** |

1. Klicken Sie auf **Weiter: Back-Ends >** . Klicken Sie auf der Registerkarte **Back-Ends** des Blatts **Anwendungsgateway erstellen** auf **Back-End-Pool hinzufügen**, und geben Sie auf dem Blatt **Back-End-Pool hinzufügen** die folgenden Einstellungen an (übernehmen Sie die Standardwerte für andere Einstellungen):

    | Einstellung | Wert |
    | --- | --- |
    | Name | **az104-06-appgw5-be1** |
    | Hinzufügen eines Back-End-Pools ohne Ziele | **Nein** |
    | Zieltyp | **IP-Adresse oder FQDN** |
    | Ziel | **10.62.0.4** |
    | Zieltyp | **IP-Adresse oder FQDN** |
    | Ziel | **10.63.0.4** |

    > **Hinweis**: Die Ziele stellen die privaten IP-Adressen von VMs in den virtuellen Spokenetzwerken **az104-06-vm2** und **az104-06-vm3** dar.

1. Klicken Sie auf **Hinzufügen** und dann auf **Weiter: Konfiguration >** . Klicken Sie auf der Registerkarte **Konfiguration** des Blatts **Anwendungsgateway erstellen** auf **+ Routingregel hinzufügen**.

1. Geben Sie auf dem Blatt **Routingregel hinzufügen** auf der Registerkarte **Listener** die folgenden Einstellungen an:

    | Einstellung | Wert |
    | --- | --- |
    | Regelname | **az104-06-appgw5-rl1** |
    | Name des Listeners | **az104-06-appgw5-rl1l1** |
    | Front-End-IP | **Public** |
    | Protocol | **HTTP** |
    | Port | **80** |
    | Listenertyp | **Grundlegend** |
    | URL zur Fehlerseite | **Nein** |

1. Navigieren Sie zur Registerkarte **Back-End-Ziele** auf dem Blatt **Routingregel hinzufügen**, und geben Sie die folgenden Einstellungen an (übernehmen Sie die Standardwerte für andere Einstellungen):

    | Einstellung | Wert |
    | --- | --- |
    | Zieltyp | **Back-End-Pool** |
    | Back-End-Ziel | **az104-06-appgw5-be1** |

1. Klicken Sie unter dem **Textfeld HTTP-Einstellungen** auf **Neu hinzufügen**, und geben Sie auf dem Blatt **HTTP-Einstellung** hinzufügen die folgenden Einstellungen an (übernehmen Sie die Standardwerte für andere Einstellungen):

    | Einstellung | Wert |
    | --- | --- |
    | Name der HTTP-Einstellungen | **az104-06-appgw5-http1** |
    | Back-End-Protokoll | **HTTP** |
    | Back-End-Port | **80** |
    | Cookiebasierte Affinität | **Deaktivieren** |
    | Verbindungsausgleich | **Deaktivieren** |
    | Anforderungstimeout (Sekunden) | **20** |

1. Klicken Sie auf dem Blatt **HTTP-Einstellung hinzufügen** auf **Hinzufügen**, und klicken Sie auf dem Blatt **Routingregel hinzufügen** auf **Hinzufügen**.

1. Klicken Sie auf **Weiter: Tags >** , gefolgt von **Weiter: Überprüfen und erstellen >** , und klicken Sie dann auf **Erstellen**.

    > **Hinweis**: Warten Sie, bis die Application Gateway-Instanz erstellt wurde. Dies kann etwa acht Minuten dauern.

1. Suchen Sie im Azure-Portal nach **Application Gateways**, und wählen Sie diese Option aus. Klicken Sie auf dem Blatt **Application Gateways** auf **az104-06-appgw5**.

1. Notieren Sie sich aus dem Blatt des Anwendungsgateways **az104-06-appgw5** den Wert für die **Öffentliche IP-Adresse des Front-Ends**.

1. Öffnen Sie ein weiteres Browserfenster, und navigieren Sie zu der IP-Adresse, die Sie im vorherigen Schritt identifiziert haben.

1. Vergewissern Sie sich, dass im Browserfenster die Meldung **Hello World from az104-06-vm2** oder **Hello World from az104-06-vm3** angezeigt wird.

1. Öffnen Sie ein weiteres Browserfenster, verwenden Sie dieses Mal jedoch den InPrivate-Modus, und überprüfen Sie, ob sich die Ziel-VM ändert (basierend auf der Meldung, die auf der Webseite angezeigt wird).

    > **Hinweis**: Möglicherweise müssen Sie das Browserfenster aktualisieren oder im InPrivate-Modus erneut öffnen.

    > **Hinweis**: Die Verwendung von VMs in mehreren virtuellen Netzwerken als Ziel ist keine übliche Konfiguration, aber sie soll verdeutlichen, dass Application Gateway in der Lage ist, auf VMs in mehreren virtuellen Netzwerken (sowie Endpunkte in anderen Azure-Regionen oder sogar außerhalb von Azure) abzuzielen, im Gegensatz zu Azure Load Balancer, der einen Lastausgleich für VMs im selben virtuellen Netzwerk vornimmt.

#### <a name="clean-up-resources"></a>Bereinigen von Ressourcen

>**Hinweis**: Denken Sie daran, alle neu erstellten Azure-Ressourcen zu entfernen, die Sie nicht mehr verwenden. Durch das Entfernen nicht verwendeter Ressourcen wird sichergestellt, dass keine unerwarteten Kosten anfallen.

>**Hinweis**: Machen Sie sich keine Sorgen, wenn die Labressourcen nicht sofort entfernt werden können. Mitunter haben Ressourcen Abhängigkeiten, sodass der Löschvorgang länger dauert. Es gehört zu den üblichen Administratoraufgaben, die Ressourcennutzung zu überwachen. Überprüfen Sie also regelmäßig Ihre Ressourcen im Portal darauf, wie es um die Bereinigung bestellt ist. 

1. Öffnen Sie im Azure-Portal im Bereich **Cloud Shell** die **PowerShell**-Sitzung.

1. Listen Sie alle Ressourcengruppen auf, die während der praktischen Übungen in diesem Modul erstellt wurden, indem Sie den folgenden Befehl ausführen:

   ```powershell
   Get-AzResourceGroup -Name 'az104-06*'
   ```

1. Löschen Sie alle Ressourcengruppen, die Sie während der praktischen Übungen in diesem Modul erstellt haben, indem Sie den folgenden Befehl ausführen:

   ```powershell
   Get-AzResourceGroup -Name 'az104-06*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Hinweis**: Der Befehl wird (wie über den Parameter „-AsJob“ festgelegt) asynchron ausgeführt. Dies bedeutet, dass Sie zwar direkt im Anschluss einen weiteren PowerShell-Befehl in derselben PowerShell-Sitzung ausführen können, es jedoch einige Minuten dauert, bis die Ressourcengruppen tatsächlich entfernt werden.

#### <a name="review"></a>Überprüfung

In diesem Lab haben Sie die folgenden Aufgaben ausgeführt:

+ Bereitstellen der Laborumgebung
+ Konfigurieren der Hub-and-Spoke-Netzwerktopologie
+ Testen der Transitivität des Peerings virtueller Netzwerke
+ Aufgabe 4: Konfigurieren des Routings in der Hub-and-Spoke-Topologie
+ Aufgabe 5: Implementieren von Azure Load Balancer
+ Aufgabe 6: Implementieren von Azure Application Gateway
