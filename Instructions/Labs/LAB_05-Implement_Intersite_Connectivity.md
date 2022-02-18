---
lab:
  title: 05 – Implementieren von Konnektivität zwischen Standorten
  module: Module 05 - Intersite Connectivity
ms.openlocfilehash: ed67186af743d7733e0f106aecf239fce15aa45f
ms.sourcegitcommit: c360d3abaa6e09814f051b2568340e80d0d0e953
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2022
ms.locfileid: "138356667"
---
# <a name="lab-05---implement-intersite-connectivity"></a>Übung 05 – Implementieren von Konnektivität zwischen Standorten
# <a name="student-lab-manual"></a>Lab-Handbuch für Kursteilnehmer

## <a name="lab-scenario"></a>Labszenario

Contoso verfügt über Rechenzentren in Boston, New York und Seattle, die über ein Netz aus WAN-Verbindungen mit vollständiger Konnektivität verbunden sind. Sie müssen eine Laborumgebung implementieren, die die Topologie der lokalen Netzwerke von Contoso widerspiegelt, und deren Funktionalität überprüfen.

## <a name="objectives"></a>Ziele

Dieses Lab deckt Folgendes ab:

+ Aufgabe 1: Bereitstellen der Laborumgebung
+ Aufgabe 2: Konfigurieren des Peerings lokaler und globaler virtueller Netzwerke
+ Aufgabe 3: Testen der Konnektivität zwischen den Standorten

## <a name="estimated-timing-30-minutes"></a>Geschätzte Zeit: 30 Minuten

## <a name="architecture-diagram"></a>Architekturdiagramm

![image](../media/lab05.png)

### <a name="instructions"></a>Anweisungen

#### <a name="task-1-provision-the-lab-environment"></a>Aufgabe 1: Bereitstellen der Laborumgebung

In dieser Aufgabe stellen Sie drei VMs in separaten virtuellen Netzwerken bereit. Zwei davon befinden sich in derselben und das dritte in einer anderen Azure-Region.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Öffnen Sie **Azure Cloud Shell** im Azure-Portal, indem Sie auf das Symbol oben rechts im Azure-Portal klicken.

1. Wenn Sie aufgefordert werden, entweder **Bash** oder **PowerShell** auszuwählen, wählen Sie **PowerShell** aus.

    >**Hinweis**: Wenn Sie **Cloud Shell** zum ersten Mal starten und die Meldung **Für Sie wurde kein Speicher bereitgestellt** angezeigt wird, wählen Sie das Abonnement aus, das Sie in diesem Lab verwenden, und klicken Sie dann auf **Speicher erstellen**.

1. Klicken Sie in der Symbolleiste des Cloud Shell-Bereichs auf das Symbol **Dateien hochladen/herunterladen**, klicken Sie im Dropdownmenü auf **Hochladen**, und laden Sie die Dateien **\\Allfiles\\Labs\\05\\az104-05-vnetvm-loop-template.json** und **\\Allfiles\\Labs\\05\\az104-05-vnetvm-loop-parameters.json** in das Cloud Shell-Basisverzeichnis hoch.

1. Bearbeiten Sie die **Parameterdatei**, die Sie gerade hochgeladen haben, und ändern Sie das Kennwort. Wenn Sie Hilfe bei der Bearbeitung der Datei in der Shell benötigen, bitten Sie Ihren Dozenten um Unterstützung. Als bewährte Methode sollten Geheimnisse, z. B. Kennwörter, sicherer in Key Vault gespeichert werden. 

1. Führen Sie im Cloud Shell-Bereich den folgenden Befehl aus, um die Ressourcengruppe zu erstellen, in der die Laborumgebung gehostet werden soll. Die ersten beiden virtuellen Netzwerke und ein VM-Paar werden in `[Azure_region_1]` bereitgestellt. Das dritte virtuelle Netzwerk und die dritte VM werden in derselben Ressourcengruppe, aber in einer anderen Region, `[Azure_region_2]`, bereitgestellt. (Ersetzen Sie die Platzhalter `[Azure_region_1]` und `[Azure_region_2]` durch die Namen zweier verschiedener Azure-Regionen, in denen Sie diese Azure-VMs bereitstellen möchten.)

   ```powershell
   $location1 = '[Azure_region_1]'

   $location2 = '[Azure_region_2]'

   $rgName = 'az104-05-rg1'

   New-AzResourceGroup -Name $rgName -Location $location1
   ```

   >**Hinweis**: Um Azure-Regionen zu identifizieren, führen Sie in einer PowerShell-Sitzung in Cloud Shell den Befehl **(Get-AzLocation).Location** aus.

1. Führen Sie im Cloud Shell-Bereich folgenden Befehl aus, um mithilfe der hochgeladenen Vorlagen- und Parameterdateien die drei virtuellen Netzwerke zu erstellen und VMs darin bereitzustellen:

   ```powershell
   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-05-vnetvm-loop-template.json `
      -TemplateParameterFile $HOME/az104-05-vnetvm-loop-parameters.json `
      -location1 $location1 `
      -location2 $location2
   ```

    >**Hinweis**: Warten Sie, bis die Bereitstellung abgeschlossen ist, bevor Sie mit dem nächsten Schritt fortfahren. Dieser Vorgang dauert etwa zwei Minuten.

1. Schließen Sie den Cloud Shell-Bereich.

#### <a name="task-2-configure-local-and-global-virtual-network-peering"></a>Aufgabe 2: Konfigurieren des Peerings lokaler und globaler virtueller Netzwerke

In dieser Aufgabe konfigurieren Sie das lokale und globale Peering zwischen den virtuellen Netzwerken, die Sie in den vorherigen Aufgaben bereitgestellt haben.

1. Suchen Sie im Azure-Portal nach der Option **Virtuelle Netzwerke** und wählen Sie sie aus.

1. Überprüfen Sie die virtuellen Netzwerke, die Sie in der vorherigen Aufgabe erstellt haben, und stellen Sie sicher, dass sich die ersten beiden in derselben Azure-Region und das dritte in einer anderen Azure-Region befinden.

    >**Hinweis**: Die Vorlage, die Sie für die Bereitstellung der drei virtuellen Netzwerke verwendet haben, stellt sicher, dass sich die IP-Adressbereiche der drei virtuellen Netzwerke nicht überschneiden.

1. Klicken Sie in der Liste virtueller Netzwerke auf **az104-05-vnet0**.

1. Klicken Sie auf dem Blatt des virtuellen Netzwerks **az104-05-vnet0** im Abschnitt **Einstellungen** auf **Peerings** und dann auf **+ Hinzufügen**.

1. Fügen Sie ein Peering mit den folgenden Einstellungen hinzu (übernehmen Sie für andere Einstellungen die Standardwerte), und klicken Sie auf **Hinzufügen**:

    | Einstellung | Wert|
    | --- | --- |
    | Dieses virtuelle Netzwerk: Name des Peeringlinks | **az104-05-vnet0_to_az104-05-vnet1** |
    | Dieses virtuelle Netzwerk: Datenverkehr zum virtuellen Remotenetzwerk | **Zulassen (Standard)** |
    | Dieses virtuelle Netzwerk: Vom virtuellen Remotenetzwerk weitergeleiteter Datenverkehr | **Von außerhalb dieses virtuellen Netzwerks stammenden Datenverkehr blockieren** |
    | Gateway des virtuellen Netzwerks | **None** |
    | Virtuelles Remotenetzwerk: Name des Peeringlinks | **az104-05-vnet1_to_az104-05-vnet0** |
    | Bereitstellungsmodell für das virtuelle Netzwerk | **Resource Manager** |
    | Ich kenne meine Ressourcen-ID | Nicht ausgewählt |
    | Subscription | Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden. |
    | Virtuelles Netzwerk | **az104-05-vnet1** |
    | Datenverkehr zum virtuellen Remotenetzwerk | **Zulassen (Standard)** |
    | Traffic forwarded from remote virtual network (Vom virtuellen Remotenetzwerk weitergeleiteter Datenverkehr) | **Von außerhalb dieses virtuellen Netzwerks stammenden Datenverkehr blockieren** |
    | Gateway des virtuellen Netzwerks | **None** |

    >**Hinweis**: In diesem Schritt werden zwei lokale Peerings eingerichtet: eines zwischen „az104-05-vnet0“ und „az104-05-vnet1“ und das andere zwischen „az104-05-vnet1“ und „az104-05-vnet0“.

    >**Hinweis**: Falls die in der vorherigen Aufgabe erstellten virtuellen Netzwerke nicht in der Oberfläche des Azure-Portals angezeigt werden, können Sie das Peering konfigurieren, indem Sie über Cloud Shell die folgenden PowerShell-Befehle ausführen:
    
   ```powershell
   $rgName = 'az104-05-rg1'

   $vnet0 = Get-AzVirtualNetwork -Name 'az104-05-vnet0' -ResourceGroupName $rgname

   $vnet1 = Get-AzVirtualNetwork -Name 'az104-05-vnet1' -ResourceGroupName $rgname

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet0_to_az104-05-vnet1' -VirtualNetwork $vnet0 -RemoteVirtualNetworkId $vnet1.Id

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet1_to_az104-05-vnet0' -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet0.Id
   ``` 

1. Klicken Sie auf dem Blatt des virtuellen Netzwerks **az104-05-vnet0** im Abschnitt **Einstellungen** auf **Peerings** und dann auf **+ Hinzufügen**.

1. Fügen Sie ein Peering mit den folgenden Einstellungen hinzu (übernehmen Sie für andere Einstellungen die Standardwerte), und klicken Sie auf **Hinzufügen**:

    | Einstellung | Wert|
    | --- | --- |
    | Dieses virtuelle Netzwerk: Name des Peeringlinks | **az104-05-vnet0_to_az104-05-vnet2** |
    | Dieses virtuelle Netzwerk: Datenverkehr zum virtuellen Remotenetzwerk | **Zulassen (Standard)** |
    | Dieses virtuelle Netzwerk: Vom virtuellen Remotenetzwerk weitergeleiteter Datenverkehr | **Von außerhalb dieses virtuellen Netzwerks stammenden Datenverkehr blockieren** |
    | Gateway des virtuellen Netzwerks | **None** |
    | Virtuelles Remotenetzwerk: Name des Peeringlinks | **az104-05-vnet2_to_az104-05-vnet0** |
    | Bereitstellungsmodell für das virtuelle Netzwerk | **Resource Manager** |
    | Ich kenne meine Ressourcen-ID | Nicht ausgewählt |
    | Subscription | Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden. |
    | Virtuelles Netzwerk | **az104-05-vnet2** |
    | Datenverkehr zum virtuellen Remotenetzwerk | **Zulassen (Standard)** |
    | Traffic forwarded from remote virtual network (Vom virtuellen Remotenetzwerk weitergeleiteter Datenverkehr) | **Von außerhalb dieses virtuellen Netzwerks stammenden Datenverkehr blockieren** |
    | Gateway des virtuellen Netzwerks | **None** |

    >**Hinweis**: In diesem Schritt werden zwei globale Peerings eingerichtet: eines zwischen „az104-05-vnet0“ und „az104-05-vnet2“ und das andere zwischen „az104-05-vnet2“ und „az104-05-vnet0“.

    >**Hinweis**: Falls die in der vorherigen Aufgabe erstellten virtuellen Netzwerke nicht in der Oberfläche des Azure-Portals angezeigt werden, können Sie das Peering konfigurieren, indem Sie über Cloud Shell die folgenden PowerShell-Befehle ausführen:
    
   ```powershell
   $rgName = 'az104-05-rg1'

   $vnet0 = Get-AzVirtualNetwork -Name 'az104-05-vnet0' -ResourceGroupName $rgname

   $vnet2 = Get-AzVirtualNetwork -Name 'az104-05-vnet2' -ResourceGroupName $rgname

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet0_to_az104-05-vnet2' -VirtualNetwork $vnet0 -RemoteVirtualNetworkId $vnet2.Id

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet2_to_az104-05-vnet0' -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet0.Id
   ``` 

1. Wechseln Sie zurück zum Blatt **Virtuelle Netzwerke**, und klicken Sie in der Liste virtueller Netzwerke auf **az104-05-vnet1**.

1. Klicken Sie auf dem Blatt des virtuellen Netzwerks **az104-05-vnet1** im Abschnitt **Einstellungen** auf **Peerings** und dann auf **+ Hinzufügen**.

1. Fügen Sie ein Peering mit den folgenden Einstellungen hinzu (übernehmen Sie für andere Einstellungen die Standardwerte), und klicken Sie auf **Hinzufügen**:

    | Einstellung | Wert|
    | --- | --- |
    | Dieses virtuelle Netzwerk: Name des Peeringlinks | **az104-05-vnet1_to_az104-05-vnet2** |
    | Dieses virtuelle Netzwerk: Datenverkehr zum virtuellen Remotenetzwerk | **Zulassen (Standard)** |
    | Dieses virtuelle Netzwerk: Vom virtuellen Remotenetzwerk weitergeleiteter Datenverkehr | **Von außerhalb dieses virtuellen Netzwerks stammenden Datenverkehr blockieren** |
    | Gateway des virtuellen Netzwerks | **None** |
    | Virtuelles Remotenetzwerk: Name des Peeringlinks | **az104-05-vnet2_to_az104-05-vnet1** |
    | Bereitstellungsmodell für das virtuelle Netzwerk | **Resource Manager** |
    | Ich kenne meine Ressourcen-ID | Nicht ausgewählt |
    | Subscription | Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden. |
    | Virtuelles Netzwerk | **az104-05-vnet2** |
    | Datenverkehr zum virtuellen Remotenetzwerk | **Zulassen (Standard)** |
    | Traffic forwarded from remote virtual network (Vom virtuellen Remotenetzwerk weitergeleiteter Datenverkehr) | **Von außerhalb dieses virtuellen Netzwerks stammenden Datenverkehr blockieren** |
    | Gateway des virtuellen Netzwerks | **None** |

    >**Hinweis**: In diesem Schritt werden zwei globale Peerings eingerichtet: eines zwischen „az104-05-vnet1“ und „az104-05-vnet2“ und das andere zwischen „az104-05-vnet2“ und „az104-05-vnet1“.

    >**Hinweis**: Falls die in der vorherigen Aufgabe erstellten virtuellen Netzwerke nicht in der Oberfläche des Azure-Portals angezeigt werden, können Sie das Peering konfigurieren, indem Sie über Cloud Shell die folgenden PowerShell-Befehle ausführen:
    
   ```powershell
   $rgName = 'az104-05-rg1'

   $vnet1 = Get-AzVirtualNetwork -Name 'az104-05-vnet1' -ResourceGroupName $rgname

   $vnet2 = Get-AzVirtualNetwork -Name 'az104-05-vnet2' -ResourceGroupName $rgname

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet1_to_az104-05-vnet2' -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet2.Id

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet2_to_az104-05-vnet1' -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet1.Id
   ``` 

#### <a name="task-3-test-intersite-connectivity"></a>Aufgabe 3: Testen der Konnektivität zwischen den Standorten

In dieser Aufgabe testen Sie die Konnektivität zwischen VMs in den drei virtuellen Netzwerken, die Sie in der vorherigen Aufgabe über lokales und globales Peering verbunden haben.

1. Suchen Sie im Azure-Portal nach **Virtuelle Computer**, und klicken Sie darauf.

1. Klicken Sie in der Liste der VMs auf **az104-05-vm0**.

1. Klicken Sie auf dem Blatt von **az104-05-vm0** auf **Verbinden**, klicken Sie im Dropdownmenü auf **RDP**, klicken Sie auf dem Blatt **Verbinden mit RDP** auf **RDP-Datei herunterladen**, und befolgen Sie die Anweisungen, um die Remotedesktopsitzung zu starten.

    >**Hinweis**: Dieser Schritt bezieht sich auf das Herstellen einer Verbindung über Remotedesktop von einem Windows-Computer aus. Auf einem Mac können Sie einen Remotedesktopclient aus dem Mac App Store verwenden. Auf Linux-Computern können Sie Open-Source-RDP-Clientsoftware verwenden.

    >**Hinweis**: Sie können Warnungseingabeaufforderungen ignorieren, wenn Sie eine Verbindung mit den Ziel-VMs herstellen.

1. Wenn Sie dazu aufgefordert werden, melden Sie sich mit dem Benutzernamen **Student** und dem Kennwort in Ihrer Parameterdatei an. 

1. Klicken Sie in der Remotedesktopsitzung mit **az104-05-vm0** mit der rechten Maustaste auf die Schaltfläche **Start**, und klicken Sie im Kontextmenü auf **Windows PowerShell (Administrator)** .

1. Führen Sie im Windows PowerShell-Konsolenfenster den folgenden Befehl aus, um die Konnektivität mit **az104-05-vm1** (mit der privaten IP-Adresse **10.51.0.4**) über TCP-Port 3389 zu testen:

   ```powershell
   Test-NetConnection -ComputerName 10.51.0.4 -Port 3389 -InformationLevel 'Detailed'
   ```

    >**Hinweis**: Beim Test wird TCP 3389 verwendet, weil dieser Port standardmäßig von der Betriebssystemfirewall zugelassen wird.

1. Überprüfen Sie die Ausgabe des Befehls, und stellen Sie sicher, dass die Verbindung erfolgreich war.

1. Führen Sie im Windows PowerShell-Konsolenfenster den folgenden Befehl aus, um die Konnektivität mit **az104-05-vm2** (mit der privaten IP-Adresse **10.52.0.4**) zu testen:

   ```powershell
   Test-NetConnection -ComputerName 10.52.0.4 -Port 3389 -InformationLevel 'Detailed'
   ```

1. Wechseln Sie auf Ihrem Labcomputer zurück zum Azure-Portal, und kehren Sie zum Blatt **VMs** zurück.

1. Klicken Sie in der Liste der VMs auf **az104-05-vm1**.

1. Klicken Sie auf dem Blatt von **az104-05-vm1** auf **Verbinden**, klicken Sie im Dropdownmenü auf **RDP**, klicken Sie auf dem Blatt **Verbinden mit RDP** auf **RDP-Datei herunterladen**, und befolgen Sie die Anweisungen, um die Remotedesktopsitzung zu starten.

    >**Hinweis**: Dieser Schritt bezieht sich auf das Herstellen einer Verbindung über Remotedesktop von einem Windows-Computer aus. Auf einem Mac können Sie einen Remotedesktopclient aus dem Mac App Store verwenden. Auf Linux-Computern können Sie Open-Source-RDP-Clientsoftware verwenden.

    >**Hinweis**: Sie können Warnungseingabeaufforderungen ignorieren, wenn Sie eine Verbindung mit den Ziel-VMs herstellen.

1. Wenn Sie dazu aufgefordert werden, melden Sie sich mit dem Benutzernamen **Student** und dem Kennwort in Ihrer Parameterdatei an. 

1. Klicken Sie in der Remotedesktopsitzung mit **az104-05-vm1** mit der rechten Maustaste auf die Schaltfläche **Start**, und klicken Sie im Kontextmenü auf **Windows PowerShell (Administrator)** .

1. Führen Sie im Windows PowerShell-Konsolenfenster den folgenden Befehl aus, um die Konnektivität mit **az104-05-vm2** (mit der privaten IP-Adresse **10.52.0.4**) über TCP-Port 3389 zu testen:

   ```powershell
   Test-NetConnection -ComputerName 10.52.0.4 -Port 3389 -InformationLevel 'Detailed'
   ```

    >**Hinweis**: Beim Test wird TCP 3389 verwendet, weil dieser Port standardmäßig von der Betriebssystemfirewall zugelassen wird.

1. Überprüfen Sie die Ausgabe des Befehls, und stellen Sie sicher, dass die Verbindung erfolgreich war.

#### <a name="clean-up-resources"></a>Bereinigen von Ressourcen

>**Hinweis**: Denken Sie daran, alle neu erstellten Azure-Ressourcen zu entfernen, die Sie nicht mehr verwenden. Durch das Entfernen nicht verwendeter Ressourcen wird sichergestellt, dass keine unerwarteten Kosten anfallen.

>**Hinweis**: Machen Sie sich keine Sorgen, wenn die Labressourcen nicht sofort entfernt werden können. Mitunter haben Ressourcen Abhängigkeiten, sodass der Löschvorgang länger dauert. Es gehört zu den üblichen Administratoraufgaben, die Ressourcennutzung zu überwachen. Überprüfen Sie also regelmäßig Ihre Ressourcen im Portal darauf, wie es um die Bereinigung bestellt ist. 

1. Öffnen Sie im Azure-Portal im Bereich **Cloud Shell** die **PowerShell**-Sitzung.

1. Listen Sie alle Ressourcengruppen auf, die während der praktischen Übungen in diesem Modul erstellt wurden, indem Sie den folgenden Befehl ausführen:

   ```powershell
   Get-AzResourceGroup -Name 'az104-05*'
   ```

1. Löschen Sie alle Ressourcengruppen, die Sie während der praktischen Übungen in diesem Modul erstellt haben, indem Sie den folgenden Befehl ausführen:

   ```powershell
   Get-AzResourceGroup -Name 'az104-05*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Hinweis**: Der Befehl wird (wie über den Parameter „-AsJob“ festgelegt) asynchron ausgeführt. Dies bedeutet, dass Sie zwar direkt im Anschluss einen weiteren PowerShell-Befehl in derselben PowerShell-Sitzung ausführen können, es jedoch einige Minuten dauert, bis die Ressourcengruppen tatsächlich entfernt werden.

#### <a name="review"></a>Überprüfung

In diesem Lab haben Sie die folgenden Aufgaben ausgeführt:

+ Bereitstellen der Laborumgebung
+ Konfigurieren des Peerings lokaler und globaler virtueller Netzwerke
+ Testen der Konnektivität zwischen Standorten
