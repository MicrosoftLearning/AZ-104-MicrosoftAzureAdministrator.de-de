---
lab:
  title: '11: Implementieren von Überwachung'
  module: Administer Monitoring
---

# <a name="lab-11---implement-monitoring"></a>Lab 11: Implementieren von Überwachung
# <a name="student-lab-manual"></a>Lab-Handbuch für Kursteilnehmer

## <a name="lab-scenario"></a>Labszenario

You need to evaluate Azure functionality that would provide insight into performance and configuration of Azure resources, focusing in particular on Azure virtual machines. To accomplish this, you intend to examine the capabilities of Azure Monitor, including Log Analytics.

Um eine Vorschau dieses Labs im interaktiven Format anzuzeigen, **[klicken Sie hier](https://mslabs.cloudguides.com/en-us/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2017)** .

## <a name="objectives"></a>Ziele

Dieses Lab deckt Folgendes ab:

+ Aufgabe 1: Bereitstellen der Laborumgebung
+ Aufgabe 2: Registrieren der Ressourcenanbieter Microsoft.Insights und Microsoft.AlertsManagement
+ Aufgabe 3: Erstellen und Konfigurieren eines Azure Log Analytics-Arbeitsbereichs und von auf Azure Automation basierenden Lösungen
+ Aufgabe 4: Überprüfen der Standardüberwachungseinstellungen von Azure-VMs
+ Aufgabe 5: Konfigurieren von Diagnoseeinstellungen für Azure-VMs
+ Aufgabe 6: Überprüfen der Azure Monitor-Funktionalität
+ Aufgabe 7: Überprüfen der Azure Log Analytics-Funktionalität

## <a name="estimated-timing-45-minutes"></a>Geschätzte Zeitdauer: 45 Minuten

## <a name="instructions"></a>Anweisungen

### <a name="exercise-1"></a>Übung 1

#### <a name="task-1-provision-the-lab-environment"></a>Aufgabe 1: Bereitstellen der Laborumgebung

In dieser Aufgabe stellen Sie eine VM bereit, die zum Testen von Überwachungsszenarien verwendet wird.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Öffnen Sie **Azure Cloud Shell** im Azure-Portal, indem Sie auf das Symbol oben rechts im Azure-Portal klicken.

1. Wenn Sie aufgefordert werden, entweder **Bash** oder **PowerShell** auszuwählen, wählen Sie **PowerShell** aus.

    >**Hinweis**: Wenn Sie **Cloud Shell** zum ersten Mal starten und die Meldung **You have no storage mounted** (Es ist kein Speicher eingebunden) angezeigt wird, wählen Sie das Abonnement aus, das Sie in diesem Lab verwenden, und klicken Sie dann auf **Create storage** (Speicher erstellen).

1. Klicken Sie in der Symbolleiste des Cloud Shell-Bereichs auf das Symbol **Dateien hochladen/herunterladen**, klicken Sie im Dropdownmenü auf **Hochladen**, und laden Sie die Dateien **\\Allfiles\\Labs\\11\\az104-11-vm-template.json** und **\\Allfiles\\Labs\\11\\az104-11-vm-parameters.json** in das Cloud Shell-Basisverzeichnis hoch.

1. Edit the Parameters file you just uploaded and change the password. If you need help editing the file in the Shell please ask your instructor for assistance. As a best practice, secrets, like passwords, should be more securely stored in the Key Vault. 

1. Führen Sie im Cloud Shell-Bereich Folgendes aus, um die Ressourcengruppe zu erstellen, die die VMs hostet (ersetzen Sie den Platzhalter `[Azure_region]` durch den Namen einer Azure-Region, in der Sie Azure-VMs bereitstellen möchten):

    >**Hinweis**: Stellen Sie sicher, dass Sie eine der Regionen auswählen, die in der [Dokumentation zu Arbeitsbereichszuordnungen](https://docs.microsoft.com/en-us/azure/automation/how-to/region-mappings) als **Log Analytics-Arbeitsbereichsregion** aufgeführt werden.

   ```powershell
   $location = '[Azure_region]'

   $rgName = 'az104-11-rg0'

   New-AzResourceGroup -Name $rgName -Location $location
   ```

1. Führen Sie im Cloud Shell-Bereich Folgendes aus, um das erste virtuelle Netzwerk zu erstellen und mithilfe der hochgeladenen Vorlagen- und Parameterdateien eine VM darin bereitzustellen:

   ```powershell
   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-11-vm-template.json `
      -TemplateParameterFile $HOME/az104-11-vm-parameters.json `
      -AsJob
   ```

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Do not wait for the deployment to complete but instead proceed to the next task. The deployment should take about 3 minutes.

#### <a name="task-2-register-the-microsoftinsights-and-microsoftalertsmanagement-resource-providers"></a>Aufgabe 2: Registrieren der Microsoft.Insights- und Microsoft.AlertsManagement-Ressourcenanbieter.

1. Führen Sie im Cloud Shell-Bereich Folgendes aus, um die Microsoft.Insights- und Microsoft.AlertsManagement-Ressourcenanbieter zu registrieren.

   ```powershell
   Register-AzResourceProvider -ProviderNamespace Microsoft.Insights

   Register-AzResourceProvider -ProviderNamespace Microsoft.AlertsManagement
   ```

1. Minimieren Sie den Cloud Shell-Bereich (schließen Sie ihn jedoch nicht).

#### <a name="task-3-create-and-configure-an-azure-log-analytics-workspace-and-azure-automation-based-solutions"></a>Aufgabe 3: Erstellen und Konfigurieren eines Azure Log Analytics-Arbeitsbereichs und von auf Azure Automation basierenden Lösungen

In dieser Aufgabe erstellen und konfigurieren Sie einen Azure Log Analytics-Arbeitsbereich und auf Azure Automation basierende Lösungen.

1. Suchen Sie im Azure-Portal nach **Log Analytics-Arbeitsbereiche**, wählen Sie diese Option aus, und klicken Sie auf dem Blatt **Log Analytics-Arbeitsbereiche** auf **+ Erstellen**.

1. Geben Sie auf dem Blatt **Log Analytics-Arbeitsbereich erstellen** auf der Registerkarte **Grundlagen** die folgenden Einstellungen ein, klicken Sie auf **Überprüfen und erstellen** und dann auf **Erstellen**:

    | Einstellungen | Wert |
    | --- | --- |
    | Subscription | Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden |
    | Resource group | Der Name einer neuen Ressourcengruppe **az104-11-rg1**. |
    | Log Analytics-Arbeitsbereich | Ein beliebiger eindeutiger Name |
    | Region | Der Name der Azure-Region, in der Sie die VM in der vorherigen Aufgabe bereitgestellt haben. |

    >**Hinweis**: Stellen Sie sicher, dass Sie dieselbe Region angeben, in der Sie in der vorherigen Aufgabe VMs bereitgestellt haben.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Wait for the deployment to complete. The deployment should take about 1 minute.

1. Suchen Sie im Azure-Portal nach **Automation-Konten**, wählen Sie diese Option aus, und klicken Sie auf dem Blatt **Automation-Konten** auf **+ Erstellen**.

1. Geben Sie auf dem Blatt **Automation-Konto erstellen** die folgenden Einstellungen an, und klicken Sie auf **Überprüfen und erstellen**. Klicken Sie nach der Überprüfung auf **Erstellen**:

    | Einstellungen | Wert |
    | --- | --- |
    | Name des Automation-Kontos | Ein beliebiger eindeutiger Name |
    | Subscription | Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden |
    | Resource group | **az104-11-rg1** |
    | Region | Der Name der Azure-Region, der basierend auf der [Dokumentation zu Arbeitsbereichszuordnungen](https://docs.microsoft.com/en-us/azure/automation/how-to/region-mappings) bestimmt wird. |

    >**Hinweis**: Stellen Sie sicher, dass Sie die Azure-Region basierend auf der [Dokumentation zu Arbeitsbereichszuordnungen](https://docs.microsoft.com/en-us/azure/automation/how-to/region-mappings) angeben.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Wait for the deployment to complete. The deployment might take about 3 minutes.

1. Klicken Sie auf **Zu Ressource wechseln**.

1. Klicken Sie auf dem Blatt „Automation-Konto“ im Abschnitt **Konfigurationsverwaltung** auf **Bestand**.

1. Wählen Sie im Bereich **Bestand** in der Dropdownliste **Log Analytics-Arbeitsbereich** den Log Analytics-Arbeitsbereich aus, den Sie zuvor in dieser Aufgabe erstellt haben, und klicken Sie dann auf **Aktivieren**.

    >Sie müssen Azure-Funktionen auswerten, die Einblicke in die Leistung und Konfiguration von Azure-Ressourcen bereitstellen, wobei Sie sich insbesondere auf Azure-VMs konzentrieren.

    >**Hinweis**: Dadurch wird automatisch auch die Lösung für **Änderungsnachverfolgung** installiert.

1. Klicken Sie auf dem Blatt „Automatisierungskonto“ im Abschnitt **Updateverwaltung** auf **Updateverwaltung** und dann auf **Aktivieren**.

    >Um dies zu erreichen, möchten Sie die Funktionen von Azure Monitor untersuchen, einschließlich Log Analytics.

#### <a name="task-4-review-default-monitoring-settings-of-azure-virtual-machines"></a>Aufgabe 4: Überprüfen der Standardüberwachungseinstellungen von Azure-VMs

In dieser Aufgabe überprüfen Sie die Standardüberwachungseinstellungen von Azure-VMs.

1. Suchen Sie im Azure-Portal nach **Virtuelle Computer**, und wählen Sie diese Option aus. Klicken Sie dann auf dem Blatt **Virtuelle Computer** auf **az104-11-vm0**.

1. Klicken Sie auf dem Blatt **az104-11-vm0** im Abschnitt **Überwachung** auf **Metriken**.

1. Beachten Sie auf dem Blatt **az104-11-vm0 \| Metriken** im Standarddiagramm, dass der einzige verfügbare **Metriknamespace** **VM-Host** ist.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: This is expected, since no guest-level diagnostic settings have been configured yet. You do have, however, the option of enabling guest memory metrics directly from the <bpt id="p1">**</bpt>Metrics Namespace<ept id="p1">**</ept> drop down-list. You will enable it later in this exercise.

1. Überprüfen Sie in der Dropdownliste **Metrik** die Liste der verfügbaren Metriken.

    >**Hinweis**: Die Liste enthält eine Reihe von CPU-, datenträger- und netzwerkbezogenen Metriken, die vom VM-Host erfasst werden können, ohne Zugriff auf Metriken auf Gastebene zu besitzen.

1. Wählen Sie in der Dropdownliste **Metrik** die Option **CPU-Prozentsatz** aus, wählen Sie in der Dropdownliste **Aggregation** die Option **Durchschnitt** aus, und überprüfen Sie das sich ergebende Diagramm.

#### <a name="task-5-configure-azure-virtual-machine-diagnostic-settings"></a>Aufgabe 5: Konfigurieren von Diagnoseeinstellungen für Azure-VMs

In dieser Aufgabe konfigurieren Sie Diagnoseeinstellungen für Azure-VMs.

1. Klicken Sie auf dem Blatt **az104-11-vm0** im Abschnitt **Überwachung** auf **Diagnoseeinstellungen**.

1. Klicken Sie auf der Registerkarte **Übersicht** des Blatts **az104-11-vm0 \| Diagnoseeinstellungen** auf **Überwachung auf Gastebene aktivieren**.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Wait for the operation to take effect. This might take about 3 minutes.

1. Wechseln Sie zur Registerkarte **Leistungsindikatoren** auf dem Blatt **az104-11-vm0 \| Diagnoseeinstellungen**, und überprüfen Sie die verfügbaren Leistungsindikatoren.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: By default, CPU, memory, disk, and network counters are enabled. You can switch to the <bpt id="p1">**</bpt>Custom<ept id="p1">**</ept> view for more detailed listing.

1. Wechseln Sie auf dem Blatt **az104-11-vm0 \| Diagnoseeinstellungen** zur Registerkarte **Protokolle**, und überprüfen Sie die verfügbaren Optionen für die Ereignisprotokollsammlung.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: By default, log collection includes critical, error, and warning entries from the Application Log and System log, as well as Audit failure entries from the Security log. Here as well you can switch to the <bpt id="p1">**</bpt>Custom<ept id="p1">**</ept> view for more detailed configuration settings.

1. Klicken Sie auf dem Blatt **az104-11-vm0** im Abschnitt **Überwachung** auf **Log Analytics-Agent** und dann auf **Aktivieren**.

1. Stellen Sie auf dem Blatt **az104-11-vm0 - Protokolle** sicher, dass der Log Analytics-Arbeitsbereich, den Sie zuvor in diesem Lab erstellt haben, in der Dropdownliste **Log Analytics-Arbeitsbereich auswählen** ausgewählt ist, und klicken Sie dann auf **Aktivieren**.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Do not wait for the operation to complete but instead proceed to the next step. The operation might take about 5 minutes.

1. Klicken Sie auf dem Blatt **az104-11-vm0 \| Protokolle** im Abschnitt **Überwachung** auf **Metriken**.

1. Beachten Sie auf dem Blatt **az104-11-vm0 \| Metriken** im Standarddiagramm, dass die Dropdownliste **Metrikennamespace** zu diesem Zeitpunkt neben dem Eintrag **VM-Host** auch den Eintrag **Gast (klassisch)** enthält.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: This is expected, since you enabled guest-level diagnostic settings. You also have the option to <bpt id="p1">**</bpt>Enable new guest memory metrics<ept id="p1">**</ept>.

1. Wählen Sie in der Dropdownliste **Metrikennamespace** den Eintrag **Gast (klassisch)** aus.

1. Überprüfen Sie in der Dropdownliste **Metrik** die Liste der verfügbaren Metriken.

    >**Hinweis**: Die Liste enthält zusätzliche Metriken auf Gastebene, die bei ausschließlicher Überwachung auf Hostebene nicht verfügbar sind.

1. Wählen Sie in der Dropdownliste **Metrik** die Option **Speicher\\Verfügbare Bytes** aus, wählen Sie in der Dropdownliste **Aggregation** die Option **Max.** aus, und überprüfen Sie dann das sich ergebende Diagramm.

#### <a name="task-6-review-azure-monitor-functionality"></a>Aufgabe 6: Überprüfen der Azure Monitor-Funktionalität

1. Suchen Sie im Azure-Portal nach **Überwachen**, und wählen Sie diese Option aus. Klicken Sie dann auf dem Blatt **Überwachen \| Übersicht** auf **Metriken**.

1. Navigieren Sie auf dem Blatt **Bereich auswählen** auf der Registerkarte **Durchsuchen** zur Ressourcengruppe **az104-11-rg0**, erweitern Sie sie, aktivieren Sie das Kontrollkästchen neben dem VM-Eintrag **az104-11-vm0** innerhalb dieser Ressourcengruppe, und klicken Sie auf **Übernehmen**.

    >**Hinweis**: Dadurch erhalten Sie die gleiche Ansicht und die gleichen Optionen, die auf dem Blatt **az104-11-vm0 - Metriken** verfügbar sind.

1. Wählen Sie in der Dropdownliste **Metrik** die Option **CPU-Prozentsatz** aus, wählen Sie in der Dropdownliste **Aggregation** die Option **Durchschnitt** aus, und überprüfen Sie das sich ergebende Diagramm.

1. Klicken Sie auf dem Blatt **Überwachen \| Metriken** im Bereich **Durchschnittlicher Prozentsatz CPU für az104-11-vm0** auf **Neue Warnungsregel**.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Creating an alert rule from Metrics is not supported for metrics from the Guest (classic) metric namespace. This can be accomplished by using Azure Resource Manager templates, as described in the document <bpt id="p1">[</bpt>Send Guest OS metrics to the Azure Monitor metric store using a Resource Manager template for a Windows virtual machine<ept id="p1">](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/collect-custom-metrics-guestos-resource-manager-vm)</ept>

1. Klicken Sie auf dem Blatt **Warnungsregel erstellen** im Abschnitt **Bedingung** auf den Eintrag für die vorhandene Bedingung.

1. Geben Sie auf dem Blatt **Signallogik konfigurieren** in der Liste der Signale im Abschnitt **Warnungslogik** die folgenden Einstellungen an (übernehmen Sie die Standardwerte für andere Einstellungen), und klicken Sie auf **Fertig**:

    | Einstellungen | Wert |
    | --- | --- |
    | Schwelle | **Statisch** |
    | Betreiber | **Größer als** |
    | Aggregationstyp | **Average** |
    | Schwellenwert | **2** |
    | Aggregationsgranularität (Zeitraum) | **1 Minute** |
    | Häufigkeit der Auswertung | **Jede Minute** |

1. Klicken Sie auf **Weiter: Aktionen >** und dann auf dem Blatt **Warnungsregel erstellen** im Abschnitt **Aktionsgruppe** auf die Schaltfläche **+ Aktionsgruppe erstellen**.

1. Geben Sie auf der Registerkarte **Grundeinstellungen** des Blatts **Aktionsgruppe erstellen** die folgenden Einstellungen an (übernehmen Sie die Standardwerte für andere Einstellungen), und wählen Sie **Weiter: Benachrichtigungen >** aus:

    | Einstellungen | Wert |
    | --- | --- |
    | Subscription | Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden |
    | Resource group | **az104-11-rg1** |
    | Aktionsgruppenname | **az104-11-ag1** |
    | Anzeigename | **az104-11-ag1** |

1. On the <bpt id="p1">**</bpt>Notifications<ept id="p1">**</ept> tab of the <bpt id="p2">**</bpt>Create an action group<ept id="p2">**</ept> blade, in the <bpt id="p3">**</bpt>Notification type<ept id="p3">**</ept> drop-down list, select <bpt id="p4">**</bpt>Email/SMS message/Push/Voice<ept id="p4">**</ept>. In the <bpt id="p1">**</bpt>Name<ept id="p1">**</ept> text box, type <bpt id="p2">**</bpt>admin email<ept id="p2">**</ept>. Click the <bpt id="p1">**</bpt>Edit details<ept id="p1">**</ept> (pencil) icon.

1. Aktivieren Sie auf dem Blatt **E-Mail/SMS/Push/Voice** das Kontrollkästchen **E-Mail**, geben Sie Ihre E-Mail-Adresse in das Textfeld **E-Mail** ein, übernehmen Sie die Standardwerte für andere Einstellungen, klicken Sie auf **OK**, und klicken Sie auf der Registerkarte **Benachrichtigungen** des Blatts **Aktionsgruppe erstellen** auf **Weiter: Aktionen >** .

1. Überprüfen Sie auf der Registerkarte **Aktionen** des Blatts **Aktionsgruppe erstellen** die in der Dropdownliste **Aktionstyp** verfügbaren Elemente, ohne Änderungen vorzunehmen, und wählen Sie dann **Überprüfen und erstellen** aus.

1. Wählen Sie auf der Registerkarte **Überprüfen und erstellen** des Blatts **Aktionsgruppe erstellen** die Option **Erstellen** aus.

1. Zurück auf dem Blatt **Warnungsregel erstellen** klicken Sie auf **Weiter: Details >** . Legen Sie im Abschnitt **Details zur Warnungsregel** die folgenden Einstellungen fest (übernehmen Sie die Standardwerte für andere Einstellungen):

    | Einstellungen | Wert |
    | --- | --- |
    | Name der Warnungsregel | **CPU-Prozentsatz über dem Testschwellenwert** |
    | Beschreibung der Warnungsregel | **CPU-Prozentsatz über dem Testschwellenwert** |
    | severity | **Schweregrad 3** |
    | Beim Erstellen aktivieren | **Ja** |

1. Klicken Sie auf **Überprüfen und erstellen** und dann auf der Registerkarte **Überprüfen und erstellen** auf **Erstellen**.

    >**Hinweis**: Die Aktivierung einer Metrikwarnungsregel kann bis zu 10 Minuten dauern.

1. Suchen Sie im Azure-Portal nach **Virtuelle Computer**, und wählen Sie diese Option aus. Klicken Sie dann auf dem Blatt **Virtuelle Computer** auf **az104-11-vm0**.

1. Klicken Sie auf dem Blatt von **az104-11-vm0** auf **Verbinden**, klicken Sie im Dropdownmenü auf **RDP**, klicken Sie auf dem Blatt **Verbinden mit RDP** auf **RDP-Datei herunterladen**, und befolgen Sie die Anweisungen, um die Remotedesktopsitzung zu starten.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: This step refers to connecting via Remote Desktop from a Windows computer. On a Mac, you can use Remote Desktop Client from the Mac App Store and on Linux computers you can use an open source RDP client software.

    >**Hinweis**: Sie können Warnungseingabeaufforderungen ignorieren, wenn Sie eine Verbindung mit den Ziel-VMs herstellen.

1. Wenn Sie dazu aufgefordert werden, melden Sie sich mit dem Benutzernamen **Student** und dem Kennwort in der Parameterdatei an.

1. Klicken Sie in der Remotedesktopsitzung auf **Start**, erweitern Sie den Ordner **Windows System**, und klicken Sie auf **Eingabeaufforderung**.

1. Führen Sie an der Eingabeaufforderung Folgendes aus, um eine erhöhte CPU-Auslastung auf der Azure-VM **az104-11-vm0** auszulösen:

   ```sh
   for /l %a in (0,0,1) do echo a
   ```

    >**Hinweis**: Dadurch wird die Endlosschleife initiiert, die die CPU-Auslastung über den Schwellenwert der neu erstellten Warnungsregel erhöhen soll.

1. Lassen Sie die Remotedesktopsitzung geöffnet, und wechseln Sie zurück zum Browserfenster, in dem das Azure-Portal auf Ihrem Lab-Computer angezeigt wird.

1. Navigieren Sie im Azure-Portal zurück zum Blatt **Überwachen**, und klicken Sie auf **Warnungen**.

1. Notieren Sie sich die Anzahl der Warnungen mit **Schweregrad 3**, und klicken Sie dann auf die Zeile **Schweregrad 3**.

    >**Hinweis**: Möglicherweise müssen Sie einige Minuten warten und auf **Aktualisieren** klicken.

1. Überprüfen Sie auf dem Blatt **Alle Warnungen** die generierten Warnungen.

#### <a name="task-7-review-azure-log-analytics-functionality"></a>Aufgabe 7: Überprüfen der Azure Log Analytics-Funktionalität

1. Navigieren Sie im Azure-Portal zurück zum Blatt **Überwachen**, und klicken Sie auf **Protokolle**.

    >**Hinweis**: Möglicherweise müssen Sie auf **Erste Schritte** klicken, wenn Sie zum ersten Mal auf Log Analytics zugreifen.

1. Klicken Sie bei Bedarf auf **Bereich auswählen**, wählen Sie auf dem Blatt **Bereich auswählen** die Registerkarte **Zuletzt verwendet** aus, wählen Sie **az104-11-vm0** aus, und klicken Sie auf **Übernehmen**.

1. Fügen Sie im Abfragefenster die folgende Abfrage ein, klicken Sie auf **Ausführen**, und überprüfen Sie das sich ergebende Diagramm:

   ```sh
   // Virtual Machine available memory
   // Chart the VM's available memory over the last hour.
   InsightsMetrics
   | where TimeGenerated > ago(1h)
   | where Name == "AvailableMB"
   | project TimeGenerated, Name, Val
   | render timechart
   ```

    > <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: The query should not have any errors (indicated by red blocks on the right scroll bar). If the query will not paste without errors directly from the instructions, paste the query code into a text editor such as Notepad, and then copy and paste it into the query window from there.


1. Klicken Sie auf der Symbolleiste auf **Abfragen**. Wechseln Sie im Bereich **Abfragen** zur Kachel **VM-Verfügbarkeit nachverfolgen**. Doppelklicken Sie darauf, um das Abfragefenster auszufüllen. Klicken Sie auf der Kachel auf die Befehlsschaltfläche **Ausführen**, und überprüfen Sie die Ergebnisse.

1. Wählen Sie auf der Registerkarte **Neue Abfrage 1** den Header **Tabellen** aus, und überprüfen Sie die Liste der Tabellen im Abschnitt **Virtuelle Computer**.

    >**Hinweis**: Die Namen mehrerer Tabellen entsprechen den Lösungen, die Sie zuvor in diesem Lab installiert haben.

1. Zeigen Sie mit der Maus auf den Eintrag **VMComputer**, und klicken Sie auf das Symbol **Vorschaudaten anzeigen**.

1. Wenn Daten verfügbar sind, klicken Sie im Bereich **Aktualisieren** auf **Im Editor verwenden**.

    >**Hinweis**: Möglicherweise müssen Sie einige Minuten warten, bis die aktualisierten Daten verfügbar sind.

#### <a name="clean-up-resources"></a>Bereinigen von Ressourcen

><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Remember to remove any newly created Azure resources that you no longer use. Removing unused resources ensures you will not see unexpected charges.

><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>:  Don't worry if the lab resources cannot be immediately removed. Sometimes resources have dependencies and take a longer time to delete. It is a common Administrator task to monitor resource usage, so just periodically review your resources in the Portal to see how the cleanup is going. 

1. Öffnen Sie im Azure-Portal im Bereich **Cloud Shell** die **PowerShell**-Sitzung.

1. Listen Sie alle Ressourcengruppen auf, die während der Labs in diesem Modul erstellt wurden, indem Sie den folgenden Befehl ausführen:

   ```powershell
   Get-AzResourceGroup -Name 'az104-11*'
   ```

1. Löschen Sie alle Ressourcengruppen, die Sie während der praktischen Übungen in diesem Modul erstellt haben, indem Sie den folgenden Befehl ausführen:

   ```powershell
   Get-AzResourceGroup -Name 'az104-11*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Hinweis**: Der Befehl wird (wie über den Parameter „-AsJob“ festgelegt) asynchron ausgeführt. Dies bedeutet, dass Sie zwar direkt im Anschluss einen weiteren PowerShell-Befehl in derselben PowerShell-Sitzung ausführen können, es jedoch einige Minuten dauert, bis die Ressourcengruppen tatsächlich entfernt werden.

#### <a name="review"></a>Überprüfung

In diesem Lab haben Sie die folgenden Aufgaben ausgeführt:

+ Bereitstellen der Laborumgebung
+ Erstellen und Konfigurieren eines Azure Log Analytics-Arbeitsbereichs und von auf Azure Automation basierenden Lösungen
+ Überprüfen der Standardüberwachungseinstellungen von Azure-VMs
+ Konfigurieren von Diagnoseeinstellungen für Azure-VMs
+ Überprüfen der Azure Monitor-Funktionalität
+ Überprüfen der Azure Log Analytics-Funktionalität
