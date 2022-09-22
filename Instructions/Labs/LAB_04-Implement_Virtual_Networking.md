---
lab:
  title: '04: Implementieren von virtuellen Netzwerken'
  module: Administer Virtual Networking
---

# <a name="lab-04---implement-virtual-networking"></a>Lab 04: Implementieren von virtuellen Netzwerken

# <a name="student-lab-manual"></a>Lab-Handbuch für Kursteilnehmer

## <a name="lab-scenario"></a>Labszenario

You need to explore Azure virtual networking capabilities. To start, you plan to create a virtual network in Azure that will host a couple of Azure virtual machines. Since you intend to implement network-based segmentation, you will deploy them into different subnets of the virtual network. You also want to make sure that their private and public IP addresses will not change over time. To comply with Contoso security requirements, you need to protect public endpoints of Azure virtual machines accessible from Internet. Finally, you need to implement DNS name resolution for Azure virtual machines both within the virtual network and from Internet.

## <a name="objectives"></a>Ziele

Dieses Lab deckt Folgendes ab:

+ Aufgabe 1: Erstellen und Konfigurieren eines virtuellen Netzwerks
+ Aufgabe 2: Bereitstellen von VMs in virtuellen Netzwerken
+ Aufgabe 3: Konfigurieren privater und öffentlicher IP-Adressen von Azure-VMs
+ Aufgabe 4: Konfigurieren von Netzwerksicherheitsgruppen
+ Aufgabe 5: Konfigurieren von Azure DNS für interne Namensauflösung
+ Aufgabe 6: Konfigurieren von Azure DNS für externe Namensauflösung

## <a name="estimated-timing-40-minutes"></a>Geschätzte Zeitdauer: 40 Minuten

## <a name="architecture-diagram"></a>Architekturdiagramm

![image](../media/lab04.png)

## <a name="instructions"></a>Anweisungen

### <a name="exercise-1"></a>Übung 1

#### <a name="task-1-create-and-configure-a-virtual-network"></a>Aufgabe 1: Erstellen und Konfigurieren eines virtuellen Netzwerks

In dieser Aufgabe erstellen Sie ein virtuelles Netzwerk mit mehreren Subnetzen, indem Sie das Azure-Portal verwenden.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Suchen Sie im Azure-Portal nach **Virtuelle Netzwerke**, und wählen Sie diese Option aus. Klicken Sie dann auf dem Blatt **Virtuelle Netzwerke** auf **+ Erstellen**.

1. Erstellen Sie ein virtuelles Netzwerk mit den folgenden Einstellungen (übernehmen Sie für andere Einstellungen die Standardwerte):

    | Einstellung | Wert |
    | --- | --- |
    | Subscription | Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden. |
    | Ressourcengruppe | Der Name einer **neuen** Ressourcengruppe **az104-04-rg1**. |
    | Name | **az104-04-vnet1** |
    | Region | Der Name einer beliebigen Azure-Region, die in dem Abonnement verfügbar ist, das Sie in diesem Lab verwenden. |

1. Klicken Sie auf **Weiter: IP-Adressen**, und geben Sie die folgenden Werte ein.

    | Einstellung | Wert |
    | --- | --- |
    | IPv4-Adressraum | **10.40.0.0/20** |

1. Klicken Sie auf **Subnetz hinzufügen**, geben Sie die folgenden Werte ein, und klicken Sie dann auf **Hinzufügen**.

    | Einstellung | Wert |
    | --- | --- |
    | Subnetzname | **subnet0** |
    | Subnetzadressbereich | **10.40.0.0/24** |

1. Accept the defaults and click <bpt id="p1">**</bpt>Review and Create<ept id="p1">**</ept>. Let validation occur, and hit <bpt id="p1">**</bpt>Create<ept id="p1">**</ept> again to submit your deployment.

    ><bpt id="p1">**</bpt>Note:<ept id="p1">**</ept> Wait for the virtual network to be provisioned. This should take less than a minute.

1. Klicken Sie auf **Zu Ressource wechseln**.

1. Klicken Sie auf dem Blatt **az104-04-vnet1** des virtuellen Netzwerks auf **Subnetze** und dann auf **+ Subnetz**.

1. Erstellen Sie ein Subnetz mit den folgenden Einstellungen (übernehmen Sie für andere Einstellungen die Standardwerte):

    | Einstellung | Wert |
    | --- | --- |
    | Name | **subnet1** |
    | Adressbereich (CIDR-Block) | **10.40.1.0/24** |
    | Netzwerksicherheitsgruppe | **None** |
    | Routingtabelle | **None** |

1. Klicken Sie unten auf der Seite auf **Speichern**.

#### <a name="task-2-deploy-virtual-machines-into-the-virtual-network"></a>Aufgabe 2: Bereitstellen von VMs in virtuellen Netzwerken

In dieser Aufgabe stellen Sie Azure-VMs mithilfe einer ARM-Vorlage in verschiedenen Subnetzen des virtuellen Netzwerks bereit.

1. Öffnen Sie **Azure Cloud Shell** im Azure-Portal, indem Sie auf das Symbol oben rechts im Azure-Portal klicken.

1. Wenn Sie aufgefordert werden, entweder **Bash** oder **PowerShell** auszuwählen, wählen Sie **PowerShell** aus.

    >**Hinweis**: Wenn Sie **Cloud Shell** zum ersten Mal starten und die Meldung **You have no storage mounted** (Es ist kein Speicher eingebunden) angezeigt wird, wählen Sie das Abonnement aus, das Sie in diesem Lab verwenden, und klicken Sie dann auf **Create storage** (Speicher erstellen).

1. Klicken Sie in der Symbolleiste des Cloud Shell-Bereichs auf das Symbol **Dateien hochladen/herunterladen**, klicken Sie im Dropdownmenü auf **Hochladen**, und laden Sie die Dateien **\\Allfiles\\Labs\\04\\az104-04-vms-loop-template.json** und **\\Allfiles\\Labs\\04\\az104-04-vms-loop-parameters.json** in das Cloud Shell-Basisverzeichnis hoch.

    >**Hinweis**: Möglicherweise müssen Sie jede Datei separat hochladen.

1. Edit the Parameters file, and change the password. If you need help editing the file in the Shell please ask your instructor for assistance. As a best practice, secrets, like passwords, should be more securely stored in the Key Vault. 

1. Führen Sie im Bereich „Cloud Shell“ Folgendes aus, um zwei VMs mithilfe der Vorlagen- und Parameterdateien bereitzustellen:

   ```powershell
   $rgName = 'az104-04-rg1'

   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-04-vms-loop-template.json `
      -TemplateParameterFile $HOME/az104-04-vms-loop-parameters.json
   ```

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: This method of deploying ARM templates uses Azure PowerShell. You can perform the same task by running the equivalent Azure CLI command <bpt id="p1">**</bpt>az deployment create<ept id="p1">**</ept> (for more information, refer to <bpt id="p2">[</bpt>Deploy resources with Resource Manager templates and Azure CLI<ept id="p2">](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/deploy-cli)</ept>.

    >Sie müssen die Funktionen virtueller Azure-Netzwerke untersuchen.

    >**Hinweis:** Wenn Sie einen Fehler erhalten haben, der besagt, dass die VM-Größe nicht verfügbar ist, bitten Sie Ihren Kursleiter um Hilfe, und versuchen Sie diese Schritte:
    > 1. Klicken Sie in Ihrer Cloud Shell-Instanz auf die Schaltfläche `{}`. Wählen Sie auf der linken Randleiste die Datei **az104-04-vms-loop-parameters.json** aus, und notieren Sie sich den Wert des Parameters `vmSize`.
    > 1. Zunächst möchten Sie ein virtuelles Netzwerk in Azure erstellen, das einige Azure-VMs hostet.
    > 1. Da Sie netzwerkbasierte Segmentierung implementieren möchten, stellen Sie sie in verschiedenen Subnetzen des virtuellen Netzwerks bereit.
    > 1. Ersetzen Sie den Wert des Parameters `vmSize` durch einen der Werte, die vom zuletzt ausgeführten Befehl zurückgegeben wurden.
    > 1. Sie möchten auch sicherstellen, dass sich ihre privaten und öffentlichen IP-Adressen im Laufe der Zeit nicht ändern.

1. Schließen Sie den Cloud Shell-Bereich.

#### <a name="task-3-configure-private-and-public-ip-addresses-of-azure-vms"></a>Aufgabe 3: Konfigurieren privater und öffentlicher IP-Adressen von Azure-VMs

In dieser Aufgabe konfigurieren Sie die statische Zuweisung öffentlicher und privater IP-Adressen, die Netzwerkschnittstellen von Azure-VMs zugewiesen sind.

   >**Hinweis**: Private und öffentliche IP-Adressen werden tatsächlich den Netzwerkschnittstellen zugewiesen, die wiederum an Azure-VMs angefügt sind. Es ist jedoch üblich, stattdessen auf IP-Adressen zu verweisen, die Azure-VMs zugewiesen sind.

1. Suchen Sie im Azure-Portal nach **Ressourcengruppen**, und klicken Sie auf dem Blatt **Ressourcengruppen** auf **az104-04-rg1**.

1. Klicken Sie auf dem Ressourcengruppenblatt **az104-04-rg1** in der Liste der zugehörigen Ressourcen auf **az104-04-vnet1**.

1. Überprüfen Sie auf dem Blatt **az104-04-vnet1** des virtuellen Netzwerks den Abschnitt **Verbundene Geräte**, und überprüfen Sie, ob zwei Netzwerkschnittstellen (**az104-04-nic0** und **az104-04-nic1**) an das virtuelle Netzwerk angefügt sind.

1. Klicken Sie auf **az104-04-nic0** und auf dem Blatt **az104-04-nic0** auf **IP-Konfigurationen**.

    >**Hinweis**: Überprüfen Sie, ob **ipconfig1** derzeit mit einer dynamischen privaten IP-Adresse eingerichtet ist.

1. Klicken Sie in der Liste der IP-Konfigurationen auf **ipconfig1**.

1. Wählen Sie auf dem Blatt **ipconfig1** im Abschnitt **Einstellungen für öffentliche IP-Adressen** die Option **Zuordnen** aus, klicken Sie auf **+ Neu erstellen**, geben Sie die folgenden Einstellungen an, und klicken Sie dann auf **OK**:

    | Einstellung | Wert |
    | --- | --- |
    | Name | **az104-04-pip0** |
    | SKU | **Standard** |

1. Legen Sie auf dem Blatt **ipconfig1** die Option **Zuweisung** auf **Statisch** fest, und übernehmen Sie den Standardwert **10.40.0.4** der **IP-Adresse**.

1. Um die Sicherheitsanforderungen von Contoso zu erfüllen, müssen Sie öffentliche Endpunkte von Azure-VMs schützen, auf die über das Internet zugegriffen werden kann.

1. Navigieren Sie zurück zum Blatt **az104-04-vnet1**.

1. Klicken Sie auf **az104-04-nic1** und auf dem Blatt von **az104-04-nic1** auf **IP-Konfigurationen**.

    >**Hinweis**: Überprüfen Sie, ob **ipconfig1** derzeit mit einer dynamischen privaten IP-Adresse eingerichtet ist.

1. Klicken Sie in der Liste der IP-Konfigurationen auf **ipconfig1**.

1. Wählen Sie auf dem Blatt **ipconfig1** im Abschnitt **Einstellungen für öffentliche IP-Adressen** die Option **Zuordnen** aus, klicken Sie auf **+ Neu erstellen**, geben Sie die folgenden Einstellungen an, und klicken Sie dann auf **OK**:

    | Einstellung | Wert |
    | --- | --- |
    | Name | **az104-04-pip1** |
    | SKU | **Standard** |

1. Legen Sie auf dem Blatt **ipconfig1** die Option **Zuweisung** auf **Statisch** fest, und übernehmen Sie den Standardwert **10.40.1.4** der **IP-Adresse**.

1. Speichern Sie die Änderungen auf dem Blatt **ipconfig1**.

1. Navigieren Sie zurück zum Blatt der Ressourcengruppe **az104-04-rg1**. Klicken Sie in der Liste der zugehörigen Ressourcen auf **az104-04-vm0**, und notieren Sie sich aus dem Blatt der VM **az104-04-vm0** den Eintrag für die öffentliche IP-Adresse.

1. Navigieren Sie zurück zum Blatt der Ressourcengruppe **az104-04-rg1**. Klicken Sie in der Liste der zugehörigen Ressourcen auf **az104-04-vm1**, und notieren Sie sich aus dem Blatt der VM **az104-04-vm1** den Eintrag für die öffentliche IP-Adresse.

    >**Hinweis**: Sie benötigen beide IP-Adressen in der letzten Aufgabe dieses Labs.

#### <a name="task-4-configure-network-security-groups"></a>Aufgabe 4: Konfigurieren von Netzwerksicherheitsgruppen

In dieser Aufgabe konfigurieren Sie Netzwerksicherheitsgruppen, um eingeschränkte Konnektivität mit Azure-VMs zu ermöglichen.

1. Navigieren Sie im Azure-Portal zurück zum Blatt der Ressourcengruppe **az104-04-rg1**, und klicken Sie in der Liste der zugehörigen Ressourcen auf **az104-04-vm0**.

1. Klicken Sie auf dem Übersichtsblatt von **az104-04-vm0** auf **Verbinden**, klicken Sie im Dropdownmenü auf **RDP**, klicken Sie auf dem Blatt **Verbinden mit RDP** auf **RDP-Datei herunterladen** unter Verwendung der öffentlichen IP-Adresse, und befolgen Sie die Anweisungen, um die Remotedesktopsitzung zu starten.

1. Beachten Sie, dass der Verbindungsversuch fehlschlägt.

    >Schließlich müssen Sie DNS-Namensauflösung für Azure-VMs sowohl innerhalb des virtuellen Netzwerks als auch aus dem Internet implementieren.

1. Beenden Sie die die VMs **az104-04-vm0** und **az104-04-vm1**.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: This is done for lab expediency. If the virtual machines are running when a network security group is attached to their network interface, it can can take over 30 minutes for the attachment to take effect. Once the network security group has been created and attached, the virtual machines will be restarted, and the attachment will be in effect immediately.

1. Suchen Sie im Azure-Portal nach **Netzwerksicherheitsgruppen**, und wählen Sie sie aus, und klicken Sie dann auf dem Blatt **Netzwerksicherheitsgruppen** auf **+ Erstellen**.

1. Erstellen Sie eine Netzwerksicherheitsgruppe mit den folgenden Einstellungen (übernehmen Sie für andere Einstellungen die Standardwerte):

    | Einstellung | Wert |
    | --- | --- |
    | Subscription | Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden. |
    | Ressourcengruppe | **az104-04-rg1** |
    | Name | **az104-04-nsg01** |
    | Region | Der Name der Azure-Region, in der Sie alle anderen Ressourcen in diesem Lab bereitgestellt haben. |

1. Click <bpt id="p1">**</bpt>Review and Create<ept id="p1">**</ept>. Let validation occur, and hit <bpt id="p1">**</bpt>Create<ept id="p1">**</ept> to submit your deployment.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Wait for the deployment to complete. This should take about 2 minutes.

1. Klicken Sie auf dem Blatt „Bereitstellung“ auf **Zu Ressource** wechseln, um das Blatt für die Netzwerksicherheitsgruppe **az104-04-nsg01** zu öffnen.

1. Klicken Sie auf dem Blatt der Netzwerksicherheitsgruppe **az104-04-nsg01** im Abschnitt **Einstellungen** auf **Eingangssicherheitsregeln**.

1. Fügen Sie eine Eingangsregel mit den folgenden Einstellungen hinzu (übernehmen Sie für andere Einstellungen die Standardwerte):

    | Einstellung | Wert |
    | --- | --- |
    | `Source` | **Alle** |
    | Source port ranges | * |
    | Destination | **Alle** |
    | Dienst | **RDP** |
    | Aktion | **Zulassen** |
    | Priorität | **300** |
    | Name | **AllowRDPInBound** |

1. Klicken Sie auf dem Blatt der Netzwerksicherheitsgruppe **az104-04-nsg01** im Abschnitt **Einstellungen** auf **Netzwerkschnittstellen**, und klicken Sie dann auf **+ Verknüpfen**.

1. Verknüpfen Sie die Netzwerksicherheitsgruppe **az104-04-nsg01** mit den **Netzwerkschnittstellen az104-04-nic0** und **az104-04-nic1**.

    >**Hinweis**: Es kann bis zu fünf Minuten dauern, bis die Regeln aus der neu erstellten Netzwerksicherheitsgruppe auf die Netzwerkschnittstellenkarte angewendet werden.

1. Starten Sie die die VMs **az104-04-vm0** und **az104-04-vm1**.

1. Navigieren Sie zurück zum Blatt der VM **az104-04-vm0**.

    >**Hinweis**: In den folgenden Schritten überprüfen Sie, ob Sie sich erfolgreich mit der Ziel-VM verbinden können.

1. Klicken Sie auf dem Blatt von **az104-04-vm0** auf **Verbinden**, klicken Sie auf **RDP**, klicken Sie auf dem Blatt **Verbinden mit RDP** auf **RDP-Datei herunterladen** unter Verwendung der öffentlichen IP-Adresse, und befolgen Sie die Anweisungen, um die Remotedesktopsitzung zu starten.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: This step refers to connecting via Remote Desktop from a Windows computer. On a Mac, you can use Remote Desktop Client from the Mac App Store and on Linux computers you can use an open source RDP client software.

    >**Hinweis**: Sie können Warnungseingabeaufforderungen ignorieren, wenn Sie eine Verbindung mit den Ziel-VMs herstellen.

1. Wenn Sie dazu aufgefordert werden, melden Sie sich mit dem Benutzernamen und Kennwort in der Parameterdatei an.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Leave the Remote Desktop session open. You will need it in the next task.

#### <a name="task-5-configure-azure-dns-for-internal-name-resolution"></a>Aufgabe 5: Konfigurieren von Azure DNS für interne Namensauflösung

In dieser Aufgabe konfigurieren Sie DNS-Namensauflösung innerhalb eines virtuellen Netzwerks mit privaten Azure DNS-Zonen.

1. Suchen Sie im Azure-Portal nach **Private DNS Zonen**, wählen Sie diese Option aus, und klicken Sie dann auf dem Blatt **Private DNS Zonen** auf **+ Erstellen**.

1. Erstellen Sie eine private DNS-Zone mit den folgenden Einstellungen (übernehmen Sie für andere Einstellungen die Standardwerte):

    | Einstellung | Wert |
    | --- | --- |
    | Subscription | Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden. |
    | Ressourcengruppe | **az104-04-rg1** |
    | Name | **contoso.org** |

1. Click <bpt id="p1">**</bpt>Review and Create<ept id="p1">**</ept>. Let validation occur, and hit <bpt id="p1">**</bpt>Create<ept id="p1">**</ept> again to submit your deployment.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Wait for the private DNS zone to be created. This should take about 2 minutes.

1. Klicken Sie auf **Zu Ressource wechseln**, um das Blatt für die private DNS-Zone von **contoso.org** zu öffnen.

1. Klicken Sie auf dem Blatt der privaten DNS-Zone von **contoso.org** im Abschnitt **Einstellungen** auf **Verknüpfungen mit virtuellen Netzwerken**.

1. Klicken Sie auf **Hinzufügen**, um eine Verknüpfung mit einem virtuellen Netzwerk mit den folgenden Einstellungen zu erstellen (übernehmen Sie für andere Einstellungen die Standardwerte):

    | Einstellung | Wert |
    | --- | --- |
    | Linkname | **az104-04-vnet1-link** |
    | Subscription | Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden. |
    | Virtuelles Netzwerk | **az104-04-vnet1** |
    | Automatische Registrierung aktivieren | enabled |

1. Klicken Sie auf **OK**.

    ><bpt id="p1">**</bpt>Note:<ept id="p1">**</ept> Wait for the virtual network link to be created. This should take less than 1 minute.

1. Klicken Sie Blatt für die private DNS-Zone von **contoso.org** in der Randleiste auf **Übersicht**.

1. Überprüfen Sie, ob die DNS-Einträge für **az104-04-vm0** und **az104-04-vm1** in der Liste der Datensatzgruppen als **Automatisch registriert** angezeigt werden.

    >**Hinweis**: Möglicherweise müssen Sie einige Minuten warten und die Seite aktualisieren, wenn die Datensatzgruppen nicht aufgeführt werden.

1. Wechseln Sie in der Remotedesktopsitzung zu **az104-04-vm0**, klicken Sie mit der rechten Maustaste auf die Schaltfläche **Start**, und klicken Sie im Kontextmenü auf **Windows PowerShell (Administrator)** .

1. Führen Sie im Windows PowerShell-Konsolenfenster Folgendes aus, um die interne Namensauflösung in der neu erstellten privaten DNS-Zone zu testen:

   ```powershell
   nslookup az104-04-vm0.contoso.org
   nslookup az104-04-vm1.contoso.org
   ```

1. Überprüfen Sie, ob die Ausgabe des Befehls die private IP-Adresse von **az104-04-vm1** (**10.40.1.4**) enthält.

#### <a name="task-6-configure-azure-dns-for-external-name-resolution"></a>Aufgabe 6: Konfigurieren von Azure DNS für externe Namensauflösung

In dieser Aufgabe konfigurieren Sie externe DNS-Namensauflösung mithilfe öffentlicher Azure DNS-Zonen.

1. Öffnen Sie in einem Webbrowser eine neue Registerkarte, und navigieren Sie zu <https://www.godaddy.com/domains/domain-name-search>.

1. Verwenden Sie die Domänennamensuche, um einen Domänennamen zu identifizieren, der nicht verwendet wird.

1. Suchen Sie im Azure-Portal nach **DNS Zonen**, wählen Sie diese Option aus, und klicken Sie dann auf dem Blatt **DNS Zonen** auf **+ Erstellen**.

1. Erstellen Sie eine DNS-Zone mit den folgenden Einstellungen (übernehmen Sie für andere Einstellungen die Standardwerte):

    | Einstellung | Wert |
    | --- | --- |
    | Subscription | Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden. |
    | Ressourcengruppe | **az104-04-rg1** |
    | Name | Der DNS-Domänenname, den Sie zuvor in dieser Aufgabe identifiziert haben. |

1. Click <bpt id="p1">**</bpt>Review and Create<ept id="p1">**</ept>. Let validation occur, and hit <bpt id="p1">**</bpt>Create<ept id="p1">**</ept> again to submit your deployment.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Wait for the DNS zone to be created. This should take about 2 minutes.

1. Klicken Sie auf **Zu Ressource wechseln**, um das Blatt der neu erstellten DNS-Zone zu öffnen.

1. Klicken Sie auf dem Blatt „DNS-Zone“ auf **+ Datensatzgruppe**.

1. Fügen Sie eine Datensatzgruppe mit den folgenden Einstellungen hinzu (übernehmen Sie für andere Einstellungen die Standardwerte):

    | Einstellung | Wert |
    | --- | --- |
    | Name | **az104-04-vm0** |
    | Typ | **A** |
    | Alias-Ressourceneintragssatz | **Nein** |
    | TTL | **1** |
    | TTL-Einheit | **Stunden** |
    | IP address (IP-Adresse) | Die öffentliche IP-Adresse von **az104-04-vm0**, die Sie in der dritten Übung dieses Labs identifiziert haben. |

1. Klicken Sie auf **OK**

1. Klicken Sie auf dem Blatt „DNS-Zone“ auf **+ Datensatzgruppe**.

1. Fügen Sie eine Datensatzgruppe mit den folgenden Einstellungen hinzu (übernehmen Sie für andere Einstellungen die Standardwerte):

    | Einstellung | Wert |
    | --- | --- |
    | Name | **az104-04-vm1** |
    | Typ | **A** |
    | Alias-Ressourceneintragssatz | **Nein** |
    | TTL | **1** |
    | TTL-Einheit | **Stunden** |
    | IP address (IP-Adresse) | Die öffentliche IP-Adresse von **az104-04-vm1**, die Sie in der dritten Übung dieses Labs identifiziert haben. |

1. Klicken Sie auf **OK**

1. Notieren Sie sich aus dem Blatt „DNS-Zone“ den Namen des Eintrags **Namensserver 1**.

1. Öffnen Sie im Azure-Portal die **PowerShell**-Sitzung in **Cloud Shell**, indem Sie auf das Symbol oben rechts im Azure-Portal klicken.

1. Führen Sie im Bereich Cloud Shell Folgendes aus, um die Namensauflösung der DNS-Datensatzgruppe **az104-04-vm0** in der neu erstellten DNS-Zone zu testen (ersetzen Sie den Platzhalter `[Name server 1]` durch den Namen des **Namensservers 1**, den Sie sich zuvor in dieser Aufgabe notiert haben, und den Platzhalter `[domain name]` durch den Namen der DNS-Domäne, die Sie zuvor in dieser Aufgabe erstellt haben):

   ```powershell
   nslookup az104-04-vm0.[domain name] [Name server 1]
   ```

1. Überprüfen Sie, ob die Ausgabe des Befehls die öffentliche IP-Adresse von **az104-04-vm0** enthält.

1. Führen Sie im Bereich Cloud Shell Folgendes aus, um die Namensauflösung der DNS-Datensatzgruppe **az104-04-vm1** in der neu erstellten DNS-Zone zu testen (ersetzen Sie den Platzhalter `[Name server 1]` durch den Namen des **Namensservers 1**, den Sie sich zuvor in dieser Aufgabe notiert haben, und den Platzhalter `[domain name]` durch den Namen der DNS-Domäne, die Sie zuvor in dieser Aufgabe erstellt haben):

   ```powershell
   nslookup az104-04-vm1.[domain name] [Name server 1]
   ```

1. Überprüfen Sie, ob die Ausgabe des Befehls die öffentliche IP-Adresse von **az104-04-vm1** enthält.

#### <a name="clean-up-resources"></a>Bereinigen von Ressourcen

 > <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Remember to remove any newly created Azure resources that you no longer use. Removing unused resources ensures you will not see unexpected charges.

 > <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>:  Don't worry if the lab resources cannot be immediately removed. Sometimes resources have dependencies and take a longer time to delete. It is a common Administrator task to monitor resource usage, so just periodically review your resources in the Portal to see how the cleanup is going. 

1. Öffnen Sie im Azure-Portal im Bereich **Cloud Shell** die **PowerShell**-Sitzung.

1. Listen Sie alle Ressourcengruppen auf, die während der Labs in diesem Modul erstellt wurden, indem Sie den folgenden Befehl ausführen:

   ```powershell
   Get-AzResourceGroup -Name 'az104-04*'
   ```

1. Löschen Sie alle Ressourcengruppen, die Sie während der praktischen Übungen in diesem Modul erstellt haben, indem Sie den folgenden Befehl ausführen:

   ```powershell
   Get-AzResourceGroup -Name 'az104-04*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Hinweis**: Der Befehl wird (wie über den Parameter „-AsJob“ festgelegt) asynchron ausgeführt. Dies bedeutet, dass Sie zwar direkt im Anschluss einen weiteren PowerShell-Befehl in derselben PowerShell-Sitzung ausführen können, es jedoch einige Minuten dauert, bis die Ressourcengruppen tatsächlich entfernt werden.

#### <a name="review"></a>Überprüfung

In diesem Lab haben Sie die folgenden Aufgaben ausgeführt:

+ Erstellen und Konfigurieren eines virtuellen Netzwerks
+ Bereitstellen von VMs im virtuellen Netzwerk
+ Konfigurieren privater und öffentlicher IP-Adressen von Azure-VMs
+ Konfigurieren von Netzwerksicherheitsgruppen
+ Konfigurieren von Azure DNS für interne Namensauflösung
+ Konfigurieren von Azure DNS für externe Namensauflösung
