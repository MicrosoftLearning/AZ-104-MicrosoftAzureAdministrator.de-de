---
lab:
  title: 'Lab 11: Implementieren von Überwachung'
  module: Administer Monitoring
---

# Lab 11: Implementieren von Überwachung

## Einführung

In diesem Lab werden Informationen zu Azure Monitor vermittelt. Sie erfahren, wie Sie eine Benachrichtigung erstellen und an eine Aktionsgruppe senden. Außerdem wird hier die Warnung ausgelöst und getestet sowie das Aktivitätsprotokoll überprüft.  

Für dieses Lab wird ein Azure-Abonnement benötigt. Ihr Abonnementtyp kann sich auf die Verfügbarkeit von Features in diesem Lab auswirken. Die Region kann geändert werden. In den Schritten wird allerdings die Region **USA, Osten** verwendet.

## Geschätzte Zeit: 40 Minuten

## Labszenario

Ihre Organisation hat ihre Infrastruktur zu Azure migriert. Es ist wichtig, dass Administratoren über erhebliche Infrastrukturänderungen informiert werden. Sie planen, die Funktionen von Azure Monitor testen – einschließlich Log Analytics.

## Interaktive Labsimulation

Für dieses Thema steht eine hilfreiche interaktive Labsimulation zur Verfügung. In der Simulation können Sie sich in Ihrem eigenen Tempo durch ein ähnliches Szenario klicken. Es gibt zwar Unterschiede zwischen der interaktiven Simulation und diesem Lab, aber viele der Kernkonzepte sind identisch. Ein Azure-Abonnement ist nicht erforderlich.

+ [Implementieren Sie Überwachungsfunktionen.](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2017) Erstellen Sie einen Log Analytics-Arbeitsbereich sowie Azure Automation-Lösungen. Überprüfen Sie die Überwachungs- und Diagnoseeinstellungen für virtuelle Computer. Überprüfen Sie die Funktionen von Azure Monitor und Log Analytics. 

## Architekturdiagramm

![Diagramm der Architekturaufgaben](../media/az104-lab11-architecture.png)

## Stellenqualifikationen

+ Aufgabe 1: Bereitstellen einer Infrastruktur unter Verwendung einer Vorlage
+ Aufgabe 2: Erstellen einer Warnung
+ Aufgabe 3: Konfigurieren von Benachrichtigungen für eine Aktionsgruppe
+ Aufgabe 4: Auslösen einer Warnung und Überprüfen, ob sie funktioniert
+ Aufgabe 5: Konfigurieren einer Warnungsverarbeitungsregel
+ Aufgabe 6: Verwenden von Protokollabfragen in Azure Monitor

## Aufgabe 1: Bereitstellen einer Infrastruktur unter Verwendung einer Vorlage

In dieser Aufgabe stellen Sie eine VM bereit, die zum Testen von Überwachungsszenarien verwendet wird.

1. Laden Sie die Lab-Dateien **\\Allfiles\\Lab11\\az104-11-vm-template.json** auf Ihren Computer herunter.

1. Melden Sie sich beim **Azure-Portal** - `https://portal.azure.com` an.

1. Suchen Sie im Azure-Portal nach `Deploy a custom template`, und wählen Sie die entsprechende Option aus.

1. Wählen Sie auf der Seite für die benutzerdefinierte Bereitstellung die Option **Bilden Sie Ihre eigene Vorlage im Editor.** aus.

1. Wählen Sie im Bereich für die Vorlagenbearbeitung die Option **Datei laden** aus.

1. Navigieren Sie zur Datei **\\Allfiles\\Labs11\\az104-11-vm-template.json**, wählen Sie sie aus, und wählen Sie anschließend **Öffnen** aus.

1. Wählen Sie **Speichern**.

1. Füllen Sie die Felder der benutzerdefinierten Bereitstellung mit den folgenden Informationen aus, und behalten Sie bei den anderen Feldern die Standardwerte bei:

    | Einstellung       | Wert         | 
    | ---           | ---           |
    | Subscription  | Ihr Azure-Abonnement |
    | Resource group| `az104-rg11` (Wählen Sie bei Bedarf **Neu erstellen** aus.)
    | Region        | **USA, Osten**   |
    | Benutzername      | `localadmin`   |
    | Kennwort      | Geben Sie ein komplexes Kennwort an. |
    
1. Wählen Sie **Überprüfen + erstellen** und dann **Erstellen** aus.

1. Warten Sie, bis die Bereitstellung abgeschlossen ist, und klicken Sie dann auf **Zu Ressourcengruppe wechseln**.

1. Überprüfen Sie, welche Ressourcen bereitgestellt wurden. Es sollte ein einzelnes virtuelles Netzwerk mit einem einzelnen virtuellen Computer vorhanden sein.

**Konfigurieren von Azure Monitor für virtuelle Computer (wird in der letzten Aufgabe verwendet)**

1. Suchen Sie im Portal nach **Monitor**, und wählen Sie die entsprechende Option aus.

1. Sehen Sie sich die verfügbaren Erkenntnisse sowie die verfügbaren Erkennungs-, Selektierungs- und Diagnosetools an.

1. Wählen Sie im Feld **VM-Erkenntnisse** die Option **Anzeigen** und anschließend **Einblicke konfigurieren** aus.

1. Wählen Sie **Aktivieren** neben Ihrem virtuellen Computer aus und dann auf dem Blatt **Aktivieren in Azure Monitor – Insights Onboarding**.

1. Übernehmen Sie die Standardeinstellungen für Abonnement und Datensammlungsregeln, und wählen Sie anschließend **Konfigurieren** aus. 

1. Es dauert einige Minuten, bis der VM-Agent installiert und konfiguriert wurde. Fahren Sie mit dem nächsten Schritt fort. 
   
## Aufgabe 2: Erstellen einer Warnung

In dieser Aufgabe wird eine Benachrichtigung für den Fall konfiguriert, dass ein virtueller Computer gelöscht wird. 

1. Wählen Sie auf der Seite **Monitor** die Option **Warnungen** aus. 

1. Wählen Sie **Erstellen +** und anschließend **Warnungsregel** aus. 

1. Markieren Sie das Feld für das Abonnement und wählen Sie dann **Anwenden**. Diese Warnung gilt für alle virtuellen Computer im Abonnement. Alternativ könnten Sie auch einen bestimmten Computer angeben. 

1. Wählen Sie die Registerkarte **Bedingung** und anschließend **Alle Signale anzeigen** aus.

1. Suchen Sie nach **VM löschen (Virtual Machines)**, und wählen Sie diese Option aus. Beachten Sie die anderen integrierten Signale. Wählen Sie **Übernehmen** aus.

1. Scrollen Sie nach unten zum Bereich **Warnungslogik**, und überprüfen Sie die ausgewählten Optionen für **Ereignisebene**. Behalten Sie die Standardeinstellung **Alle ausgewählten Elemente** bei.

1. Überprüfen Sie die ausgewählten Optionen für **Status**. Behalten Sie die Standardeinstellung **Alle ausgewählten Elemente** bei.

1. Lassen Sie den Bereich **Warnungsregel erstellen** für die nächste Aufgabe geöffnet.

## Aufgabe 3: Konfigurieren von Aktionsgruppenbenachrichtigungen

In dieser Aufgabe soll eine E-Mail-Benachrichtigung an das Betriebsteam gesendet werden, wenn die Warnung ausgelöst wird. 

1. Setzen Sie die Arbeit an Ihrer Benachrichtigung fort. Wählen Sie **Aktionsgruppen verwenden** und dann im Blatt **Aktionsgruppe auswählen** die Option **Aktionsgruppe erstellen** aus.

    >**Schon gewusst?** Sie können einer Benachrichtigungsregel bis zu fünf Aktionsgruppen hinzufügen. Aktionsgruppen werden gleichzeitig ohne eine bestimmte Reihenfolge ausgeführt. Mehrere Benachrichtigungsregeln können dieselbe Aktionsgruppe verwenden. 

1. Füllen Sie auf der Registerkarte **Grundlagen** die folgenden Felder für jede Einstellung aus.

    | Einstellung | Wert |
    |---------|---------|
    | **Projektdetails** |
    | Subscription | Ihr Abonnement |
    | Ressourcengruppe | **az104-rg11** |
    | Region | **Global** (Standard) |
    | **Instanzendetails** |
    | Aktionsgruppenname | `Alert the operations team` (muss in der Ressourcengruppe eindeutig sein) |
    | Anzeigename | `AlertOpsTeam` |

1. Klicken Sie auf **Weiter: Benachrichtigungen**, und geben Sie die folgenden Werte für die einzelnen Einstellungen ein.

    | Einstellung | Wert |
    |---------|---------|
    | Benachrichtigungstyp | **E-Mail/SMS-Nachricht/Pushnachricht/Sprachnachricht** auswählen |
    | Name | `VM was deleted` |

1. Klicken Sie auf **E-Mail**, geben Sie im Feld **E-Mail** Ihre E-Mail-Adresse an, und klicken Sie auf **OK**. 

    >**Hinweis:** Sie sollten eine E-Mail-Benachrichtigung mit dem Hinweis erhalten, dass Sie einer Aktionsgruppe hinzugefügt wurden. Es kann ein paar Minuten dauern, aber das ist ein sicheres Zeichen dafür, dass die Regel bereitgestellt wurde.

1. Wählen Sie **Überprüfen + erstellen** und danach **Erstellen** aus.
   
1. Wechseln Sie nach Erstellung der Aktionsgruppe zur Registerkarte **Weiter: Details**, und geben Sie die folgenden Werte für die einzelnen Einstellungen ein.

    | Einstellung | Wert |
    |---------|---------|
    | Name der Warnungsregel | `VM was deleted` |
    | Beschreibung der Warnungsregel | `A VM in your resource group was deleted` |

1. Wählen Sie **Überprüfen und erstellen** aus, um Ihre Eingabe zu überprüfen, und wählen Sie dann **Erstellen** aus.

## Aufgabe 4: Auslösen einer Warnung und Überprüfen, ob sie funktioniert

In dieser Aufgabe wird die Warnung ausgelöst und überprüft, ob eine Benachrichtigung gesendet wird. 

>**Hinweis:** Wenn Sie den virtuellen Computer löschen, bevor die Warnungsregel bereitgestellt wurde, wird sie möglicherweise nicht ausgelöst. 

1. Suchen Sie im Portal nach **Virtuelle Computer**, und klicken Sie darauf.

1. Aktivieren Sie das Kontrollkästchen für den virtuellen Computer **az104-vm0**.

1. Wählen Sie **Löschen** aus der Menüleiste aus.

1. Aktivieren Sie das Kontrollkästchen **Erzwungene Löschung anwenden**. Markieren Sie das Kontrollkästchen unten, um zu bestätigen, dass die Ressourcen gelöscht werden sollen, und klicken Sie auf **Löschen**. 

1. Wählen Sie auf der Titelleiste das Symbol **Benachrichtigungen** aus, und warten Sie, bis **vm0** erfolgreich gelöscht wurde.

1. Sie sollten eine Benachrichtigungs-E-Mail erhalten, die Folgendes besagt: **Wichtiger Hinweis: Die Azure Monitor-Warnung „Die VM wurde gelöscht“ wurde aktiviert...** Öffnen Sie andernfalls Ihr E-Mail-Programm, und suchen Sie nach einer E-Mail von azure-noreply@microsoft.com.

    ![Screenshot: Warnungs-E-Mail.](../media/az104-lab11-alert-email.png)
   
1. Klicken Sie im Ressourcenmenü des Azure-Portals auf **Überwachen**, und wählen Sie dann **Warnungen** im Menü auf der linken Seite aus.

1. Es sollten drei ausführliche Warnungen vorhanden sein, die durch das Löschen von **vm0** generiert wurden.

   >**Hinweis:** Es kann einige Minuten dauern, bis die Benachrichtigungs-E-Mail gesendet wird und die Warnungen im Portal aktualisiert werden. Wenn Sie nicht warten möchten, können Sie mit der nächsten Aufgabe fortfahren und später hierher zurückkehren. 

1. Wählen Sie den Namen einer der Warnungen aus (z. B. **VM wurde gelöscht**). Ein Bereich **Warnungsdetails** wird angezeigt, der weitere Details zum Ereignis bereitstellt.

## Aufgabe 5: Konfigurieren einer Warnungsverarbeitungsregel

In dieser Aufgabe wird eine Warnungsregel erstellt, um Benachrichtigungen während eines Wartungszeitraums zu unterdrücken. 

1. Wählen Sie auf dem Blatt **Warnungen** erst **Warnungsverarbeitungsregeln** und dann **+ Erstellen** aus. 
   
1. Wählen Sie Ihr **Abonnement** und dann **Übernehmen** aus.
   
1. Wählen Sie **Weiter: Regeleinstellungen** und dann **Benachrichtigungen unterdrücken** aus.
   
1. Wählen Sie **Weiter: Planung** aus.
   
1. Die Regel ist standardmäßig immer aktiv, es sei denn, Sie deaktivieren sie oder konfigurieren einen Zeitplan. Hier wird eine Regel definiert, um Benachrichtigungen während nächtlicher Wartungsarbeiten zu unterdrücken.
Geben Sie die folgenden Einstellungen für die Planung der Warnungsverarbeitungsregel ein:

    | Einstellung | Wert |
    |---------|---------|
    | Regel anwenden | Zu einem bestimmten Zeitpunkt |
    | Start | Geben Sie das heutige Datum und 22:00 Uhr ein. |
    | ENDE | Geben Sie das morgige Datum und 7:00 Uhr ein. |
    | Zeitzone | Wählen Sie die lokale Zeitzone aus. |

    ![Screenshot: Planungsabschnitt einer Warnungsverarbeitungsregel](../media/az104-lab11-alert-processing-rule-schedule.png)

1. Wählen Sie **Weiter: Details** aus, und geben Sie diese Einstellungen ein:

    | Einstellung | Wert |
    |---------|---------|
    | Resource group | **az104-rg11** |
    | Regelname | `Planned Maintenance` |
    | Beschreibung | `Suppress notifications during planned maintenance.` |

1. Wählen Sie **Überprüfen und erstellen** aus, um Ihre Eingabe zu überprüfen, und wählen Sie dann **Erstellen** aus.

## Aufgabe 6: Verwenden von Protokollabfragen in Azure Monitor

In dieser Aufgabe wird Azure Monitor verwendet, um die erfassten Daten des virtuellen Computers abzufragen.

1. Suchen Sie im Azure-Portal nach `Monitor`, wählen Sie es aus, und klicken Sie anschließend auf **Protokolle**.

1. Schließen Sie bei Bedarf den Begrüßungsbildschirm. 

1. Wählen Sie bei Bedarf einen Bereich, Ihr **Abonnement** aus. Wählen Sie **Übernehmen**. 

1. Wählen Sie auf der Registerkarte **Abfragen** die Option **Virtuelle Computer** (linker Bereich) aus. Möglicherweise müssen Sie das Blatt erneut öffnen.

    ![Screenshot der Registerkarte „Abfragen“.](../media/az104-lab11-queries.png)

1. Überprüfen Sie die verfügbaren Abfragen. Zeigen Sie auf die Abfrage **Heartbeats zählen**, und führen Sie sie mithilfe der Option **Ausführen** aus.

1. Daraufhin sollten Sie eine Heartbeat-Anzahl für die Zeit erhalten, in der der virtuelle Computer ausgeführt wurde.

1. Wählen Sie auf der rechten Seite des Bildschirms die Dropdownliste neben dem **einfachen Modus** aus, und wählen Sie den **KQL-Modus** aus. Überprüfen Sie die Abfrage. Diese Abfrage verwendet die Tabelle *Heartbeat*.

1. Ersetzen Sie die Abfrage durch diese, und klicken Sie anschließend auf **Ausführen**. Überprüfen Sie das resultierende Diagramm. 

   ```
    InsightsMetrics
    | where TimeGenerated > ago(1h)
    | where Name == "UtilizationPercentage"
    | summarize avg(Val) by bin(TimeGenerated, 5m), Computer //split up by computer
    | render timechart
   ```

    >**Hinweis:** Wenn die Abfrage nicht ordnungsgemäß eingefügt wird, versuchen Sie, sie in Editor einzufügen und dann zu kopieren und erneut in das Abfragefeld einzufügen.

1. Sollten Sie noch Zeit haben, können Sie noch weitere Abfragen überprüfen und ausführen. 

    >**Schon gewusst?**: Für den Fall, dass Sie noch mit anderen Abfragen üben möchten, steht eine [Demoumgebung für Log Analytics](https://learn.microsoft.com/azure/azure-monitor/logs/log-analytics-tutorial#open-log-analytics) zur Verfügung.
    
    >**Schon gewusst?**: Wenn Sie eine Abfrage gefunden haben, die Ihnen gefällt, können Sie auf der Grundlage dieser Abfrage eine Benachrichtigung erstellen. 

## Bereinigen Ihrer Ressourcen

Wenn Sie mit **Ihrem eigenen Abonnement** arbeiten, nehmen Sie sich eine Minute Zeit, um die Labressourcen zu löschen. Dadurch wird sichergestellt, dass Ressourcen freigegeben und Kosten minimiert werden. Die einfachste Möglichkeit zum Löschen der Labressourcen besteht darin, die Ressourcengruppe des Labs zu löschen. 

+ Wählen Sie im Azure-Portal die Ressourcengruppe und dann **Ressourcengruppe löschen** aus, **geben Sie den Ressourcengruppennamen ein**, und klicken Sie dann auf **Löschen**.
+ Bei Verwendung von Azure PowerShell: `Remove-AzResourceGroup -Name resourceGroupName`.
+ Bei Verwendung der Befehlszeilenschnittstelle: `az group delete --name resourceGroupName`.

## Erweitern Ihrer Lernerfahrung mit Copilot
Copilot kann Sie beim Erlernen der Verwendung von Azure-Skripttools unterstützen. Copilot kann Sie auch in Bereichen unterstützen, die nicht im Lab behandelt werden oder in denen Sie weitere Informationen benötigen. Öffnen Sie einen Edge-Browser, und wählen Sie „Copilot“ (rechts oben) aus, oder navigieren Sie zu *copilot.microsoft.com*. Nehmen Sie sich einige Minuten Zeit, um diese Prompts auszuprobieren.

+ Welche grundlegenden Konfigurationsschritte sind erforderlich, um in Azure benachrichtigt zu werden, wenn eine VM heruntergefahren wird?
+ Wie kann ich benachrichtigt werden, wenn eine Azure-Warnung ausgelöst wird?
+ Erstelle eine Azure Monitor-Abfrage, um Informationen zur CPU-Leistung einer VM zu erhalten.

## Weiterlernen im eigenen Tempo

+ [Verbessern der Reaktion auf Incidents mithilfe von Warnungen in Azure](https://learn.microsoft.com/en-us/training/modules/incident-response-with-alerting-on-azure/): Reagieren Sie auf Incidents und Aktivitäten in Ihrer Infrastruktur mithilfe von Warnungsfunktionen in Azure Monitor.
+ [Überwachen Sie Ihre virtuellen Azure-VMs mit Azure Monitor](https://learn.microsoft.com/en-us/training/modules/monitor-azure-vm-using-diagnostic-data/): Hier erfahren Sie, wie Sie Ihre virtuellen Azure-Computer mithilfe von Azure Monitor überwachen, um Metriken und Protokolle von VM-Hosts und -Clients zu sammeln und zu analysieren.

## Wichtige Erkenntnisse

Herzlichen Glückwunsch zum erfolgreichen Abschluss des Labs. Hier sind die wichtigsten Erkenntnisse für dieses Lab. 

+ Warnungen helfen Ihnen dabei, Probleme zu erkennen und zu behandeln, bevor Benutzer bemerken, dass möglicherweise ein Problem mit Ihrer Infrastruktur oder Anwendung vorliegt.
+ Sie können zu jeder Metrik oder Protokolldatenquelle der Azure Monitor-Datenplattform Warnungen erhalten.
+ Eine Warnungsregel überwacht Ihre Daten und erfasst ein Signal, das anzeigt, dass auf der angegebenen Ressource etwas passiert.
+ Eine Warnung wird ausgelöst, wenn die Bedingungen der Warnungsregel erfüllt sind. Es können mehrere Aktionen (E-Mail, SMS, Pushbenachrichtigung, Sprachbenachrichtigung) ausgelöst werden.
+ Aktionsgruppen enthalten Personen, die im Falle einer Warnung benachrichtigt werden sollen.
