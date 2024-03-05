---
lab:
  title: 'Lab 10: Implementieren des Schutzes von Daten'
  module: Administer Data Protection
---

# Lab 10: Implementieren des Schutzes von Daten

## Einführung in das Lab    

In diesem Lab erfahren Sie mehr über die Sicherung und Wiederherstellung von Azure-VMs. Sie lernen, wie Sie einen Wiederherstellungsdiensttresor und eine Sicherungsrichtlinie für Azure-VMs erstellen. Sie erfahren mehr über die Notfallwiederherstellung mit Azure Site Recovery. 

Für dieses Lab wird ein Azure-Abonnement benötigt. Ihr Abonnementtyp kann sich auf die Verfügbarkeit von Features in diesem Lab auswirken. Sie können die Regionen ändern, aber in den Schritten werden die Regionen **USA, Osten** und **USA, Westen** verwendet.

## Geschätzte Zeit: 50 Minuten

## Labszenario

Ihre Organisation prüft, wie Azure-VMs vor versehentlichem oder böswilligem Datenverlust gesichert und wiederhergestellt werden können. Darüber hinaus möchte die Organisation die Verwendung von Azure Site Recovery für Notfallwiederherstellungsszenarien untersuchen. 

## Interaktive Labsimulation

Für dieses Thema steht eine hilfreiche interaktive Labsimulation zur Verfügung. In der Simulation können Sie sich in Ihrem eigenen Tempo durch ein ähnliches Szenario klicken. Es gibt zwar Unterschiede zwischen der interaktiven Simulation und diesem Lab, aber viele der Kernkonzepte sind identisch. Ein Azure-Abonnement ist nicht erforderlich.

+ **[Sichern von VMs und lokalen Dateien.](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2016)** Sie erstellen einen Wiederherstellungsdiensttresor und implementieren eine Sicherung einer Azure-VM. Sie implementieren die lokale Datei- und Ordnersicherung mithilfe des Microsoft Azure Recovery Services-Agents. Lokale Sicherungen werden in diesem Lab nicht behandelt, es kann jedoch hilfreich sein, sich diese Schritte anzusehen. 

## Stellenqualifikationen

+ Aufgabe 1: Verwenden einer Vorlage zum Bereitstellen einer Infrastruktur
+ Aufgabe 2: Erstellen und Konfigurieren eines Recovery Services-Tresors
+ Aufgabe 3: Konfigurieren der Sicherung auf Ebene der Azure-VM
+ Aufgabe 4: Überwachen von Azure Backup
+ Aufgabe 5: Aktivieren der VM-Replikation 

## Geschätzte Zeitdauer: 40 Minuten

## Architekturdiagramm

![Diagramm der Architekturaufgaben](../media/az104-lab10-architecture.png)

## Aufgabe 1: Verwenden einer Vorlage zum Bereitstellen einer Infrastruktur

In dieser Aufgabe verwenden Sie eine Vorlage zum Bereitstellen einer VM. Die VM wird verwendet, um verschiedene Sicherungsszenarien zu testen.

1. Laden Sie die Labdateien für **\\Allfiles\\Lab10\\** herunter.

1. Melden Sie sich beim **Azure-Portal** - `https://portal.azure.com` an.

1. Suchen Sie nach `Deploy a custom template`, und wählen Sie diese Option aus.

1. Wählen Sie auf der Seite für die benutzerdefinierte Bereitstellung die Option **Erstellen Ihrer eigene Vorlage im Editor** aus.

1. Wählen Sie auf der Seite „Vorlage bearbeiten“ die Option **Datei laden** aus.

1. Suchen Sie die Datei**\\Allfiles\\Lab10\\az104-10-vms-edge-template.json**, und wählen Sie diese Datei und dann **Öffnen** aus.

   >**Hinweis:** Nehmen Sie sich einen Moment Zeit, um die Vorlage zu überprüfen. Wir stellen ein virtuelles Netzwerk und eine VM bereit, damit wir Sicherung und Wiederherstellung demonstrieren können. 

1. **Speichern** Sie die Änderungen.

1. Wählen Sie **Parameter bearbeiten** und dann **Datei laden** aus.

1. Laden Sie die Datei **\\Allfiles\\Lab10\\az104-10-vms-edge-parameters.json**, und wählen Sie sie aus.

1. **Speichern** Sie die Änderungen.

1. Füllen Sie die Felder der benutzerdefinierten Bereitstellung mit den folgenden Informationen aus, und behalten Sie bei den anderen Feldern die Standardwerte bei:

    | Einstellung       | Wert         | 
    | ---           | ---           |
    | Subscription  | Ihr Azure-Abonnement |
    | Resource group| `az104-rg-region1` (Wählen Sie bei Bedarf **Neu erstellen** aus.)
    | Region        | **USA, Osten**   |
    | Username      | **localadmin**   |
    | Kennwort      | Geben Sie ein komplexes Kennwort an. |

1. Wählen Sie **Überprüfen + erstellen** und dann **Erstellen** aus.

    >**Hinweis:** Warten Sie, bis die Vorlage bereitgestellt wurde, und wählen Sie dann **Zu Ressource wechseln** aus. Sie sollten über eine VM in einem virtuellen Netzwerk verfügen. 

## Aufgabe 2: Erstellen und Konfigurieren von Recovery Services-Tresoren

In dieser Aufgabe erstellen Sie einen Recovery Services-Tresor. Ein Recovery Services-Tresor stellt Speicher für die VM-Daten bereit. 

1. Suchen Sie im Azure-Portal nach `Recovery Services vaults`, und wählen Sie diese Option aus. Klicken Sie auf dem Blatt **Recovery Services-Tresore** auf **+ Erstellen**.

1. Geben Sie auf dem Blatt **Recovery Services-Tresor erstellen** die folgenden Einstellungen an:

    | Einstellungen | Wert |
    | --- | --- |
    | Abonnement | der Name Ihres Azure-Abonnements |
    | Resource group | `az104-rg-region1`  |
    | Tresorname | `az104-rsv-region1` |
    | Region | **USA, Osten** |

    >**Hinweis**: Stellen Sie sicher, dass Sie dieselbe Region angeben, in der Sie in der vorherigen Aufgabe VMs bereitgestellt haben.

    ![Screenshot des Recovery Services-Tresors.](../media/az104-lab10-create-rsv.png)

1. Klicken Sie auf **Überprüfen + erstellen**, stellen Sie sicher, dass die Überprüfung erfolgreich war, und klicken Sie dann auf **Erstellen**.

    >**Hinweis**: Warten Sie, bis die Bereitstellung abgeschlossen ist. Die Bereitstellung dürfte einige Minuten dauern. 

1. Klicken Sie nach Abschluss der Bereitstellung auf **Zu Ressource wechseln**.

1. Klicken Sie auf dem Blatt des Recovery Services-Tresors im Abschnitt **Einstellungen** auf **Eigenschaften**.

1. Wählen Sie unter der Bezeichnung **Sicherungskonfiguration** den Link **Aktualisieren** aus.

1. Überprüfen Sie auf dem Blatt **Sicherungskonfiguration** die Auswahlmöglichkeiten für **Speicherreplikationstyp**. Übernehmen Sie die Standardeinstellung **Georedundant**, und schließen Sie das Blatt.

    >**Hinweis**: Diese Einstellung kann nur konfiguriert werden, wenn keine Sicherungselemente vorhanden sind.
    
    >**Schon gewusst?** Die Option „Regionsübergreifende Wiederherstellung“ ermöglicht es Ihnen, Daten in einer sekundären [gekoppelten Azure-Region](https://learn.microsoft.com/azure/backup/backup-create-recovery-services-vault#set-cross-region-restore) wiederherzustellen. 

1. Kehren Sie zum Blatt des Recovery Services-Tresors zurück, und klicken Sie unter **Sicherheitseinstellungen > Vorläufiges Löschen und Sicherheitseinstellungen** auf den Link **Aktualisieren**.

1. Beachten Sie auf dem Blatt **Sicherheitseinstellungen**, dass **Vorläufiges Löschen (für Workloads, die in Azure ausgeführt werden)** auf **Aktiviert** festgelegt ist. Beachten Sie, dass der **Aufbewahrungszeitraum für vorläufiges Löschen** auf **14** Tage festgelegt ist. 

1. Kehren Sie zum Blatt des Recovery Services-Tresors zurück, und wählen Sie das Blatt **Übersicht** aus.

>**Schon gewusst?** In Azure gibt es zwei Arten von Tresoren: Recovery Services-Tresore und Sicherungstresore. Der Hauptunterschied liegt in den Datenquellen, die gesichert werden können. Weitere Informationen zu den Unterschieden finden Sie [hier](https://learn.microsoft.com/answers/questions/405915/what-is-difference-between-recovery-services-vault).

## Aufgabe 3: Konfigurieren der Sicherung auf Ebene der Azure-VM

In dieser Aufgabe implementieren Sie Sicherung auf Azure-VM-Ebene. Als Teil einer VM-Sicherung müssen Sie die Sicherungs- und Aufbewahrungsrichtlinie definieren, die für die Sicherung gilt. Unterschiedlichen VMs können unterschiedliche Sicherungs- und Aufbewahrungsrichtlinien zugewiesen sein.

   >**Hinweis**: Bevor Sie mit dieser Aufgabe beginnen, stellen Sie sicher, dass die Bereitstellung, die Sie in der ersten Aufgabe dieses Labs initiiert haben, erfolgreich abgeschlossen wurde.

1. Klicken Sie auf dem Blatt des Recovery Services-Tresors auf **Übersicht** und dann auf **+ Sichern**.

1. Geben Sie auf dem Blatt **Sicherungsziel** die folgenden Einstellungen an:

    | Einstellungen | Wert |
    | --- | --- |
    | Wo wird Ihre Workload ausgeführt? | **Azure** (Beachten Sie die anderen Optionen.) |
    | Was möchten Sie sichern? | **VM** (Beachten Sie die anderen Optionen.) |

1. Wählen Sie **Sichern** aus.

1. Beachten Sie, dass es zwei **Richtlinienuntertypen** gibt: **Erweitert** und **Standard**. Überprüfen Sie die Auswahlmöglichkeiten, und wählen Sie **Standard** aus. 

1. Wählen Sie unter **Sicherungsrichtlinie**die Option **Neue Richtlinie erstellen** aus.

1. Definieren Sie eine neue Sicherungsrichtlinie mit den folgenden Einstellungen (übernehmen Sie für andere Einstellungen die Standardwerte):

    | Einstellung | Wert |
    | ---- | ---- |
    | Richtlinienname | `az104-backup` |
    | Häufigkeit | **Täglich** |
    | Time | **24:00 Uhr** |
    | Zeitzone | Der Name Ihrer lokalen Zeitzone. |
    | Momentaufnahmen für sofortige Wiederherstellung beibehalten für | **12** Tag(e) |

    ![Screenshot der Seite „Sicherungsrichtlinie“.](../media/az104-lab10-backup-policy.png)

1. Klicken Sie auf **OK**, um die Richtlinie zu erstellen, und wählen Sie dann im Abschnitt **Virtual Machines** die Option **Hinzufügen** aus.

1. Wählen Sie auf dem Blatt **VMs auswählen** die VM **az-104-10-vm0** aus, klicken Sie auf **OK**,und klicken Sie dann auf dem Blatt **Sicherung** auf **Sicherung aktivieren**.

    >**Hinweis**: Warten Sie, bis die Sicherung aktiviert wurde. Dies sollte ungefähr 2 Minuten in Anspruch nehmen.

1. Klicken Sie im Abschnitt **Geschützte Elemente** auf **Sicherungselemente** und dann auf den Eintrag **Azure-VM**.

1. Wählen Sie den Link **Details anzeigen** für **az104-10-vm0** aus, und überprüfen Sie die Werte der Einträge **Sicherungsvorüberprüfung** und **Status der letzten Sicherung**.

    >**Hinweis:** Beachten Sie, dass die Sicherung aussteht.
    
1. Wählen Sie **Jetzt sichern** aus, übernehmen Sie den Standardwert in der Dropdownliste **Sicherung beibehalten bis**, und klicken Sie dann auf **OK**.

    >**Hinweis**: Warten Sie nicht, bis die Sicherung abgeschlossen ist, sondern fahren Sie stattdessen mit der nächsten Aufgabe fort.

## Aufgabe 4: Überwachen von Azure Backup

In dieser Aufgabe stellen Sie ein Azure-Speicherkonto bereit. Anschließend konfigurieren Sie den Tresor so, dass die Protokolle und Metriken an das Speicherkonto gesendet werden. Dieses Repository kann dann mit Log Analytics oder anderen Überwachungslösungen von Drittanbietern verwendet werden.

1. Suchen Sie im Azure-Portal nach `Storage accounts`, und wählen Sie die entsprechende Option aus.

1. Wählen Sie auf der Seite „Speicherkonto“ die Option **Erstellen** aus.

1. Verwenden Sie die folgenden Informationen, um das Speicherkonto zu definieren, und wählen Sie dann **Überprüfen** aus.

    | Einstellungen | Wert |
    | --- | --- | 
    | Abonnement          | *Ihr Abonnement*    |
    | Ressourcengruppe        | **az104-rg-region1**        |
    | Speicherkontoname  | Geben Sie einen global eindeutigen Namen an.   |
    | Region                | **USA, Osten**   |

1. Wählen Sie auf der Registerkarte „Überprüfen“ die Option **Erstellen** aus.

    >**Hinweis**: Warten Sie, bis die Bereitstellung abgeschlossen ist. Dieser Vorgang dauert etwa eine Minute.

1. Suchen Sie nach Ihrem Recovery Services-Tresor, und wählen Sie ihn aus.

1. Wählen Sie **Diagnoseeinstellungen** und dann **Diagnoseeinstellung hinzufügen** aus.

1. Nennen Sie die Einstellung `Logs and Metrics to storage`.

1. Setzen Sie ein Häkchen neben die folgenden Protokoll- und Metrikkategorien:

    - **Azure Backup-Berichtsdaten**
    - **Add-On: Azure Backup-Auftragsdaten**
    - **Add-On: Azure Backup-Warnungsdaten**
    - **Azure Site Recovery-Aufträge**
    - **Azure Site Recovery-Ereignisse**
    - **Gesundheit**

1. Setzen Sie in den Zieldetails ein Häkchen neben **In ein Speicherkonto archivieren**.

1. Wählen Sie im Dropdownfeld „Speicherkonto“ das Speicherkonto aus, das Sie zuvor in dieser Aufgabe bereitgestellt haben.

1. Wählen Sie **Speichern**.

1. Kehren Sie zum Recovery Services-Tresor zurück, und wählen Sie auf dem Blatt **Überwachung** die Option **Sicherungsaufträge** aus.

1. Suchen Sie den Sicherungsvorgang für die VM **az104-10-vm0**. 

1. Überprüfen Sie die Details zum Sicherungsauftrag.

## Aufgabe 5: Aktivieren der Replikation der virtuellen Computer

1. Suchen Sie im Azure-Portal nach `Recovery Services vaults`, und wählen Sie diese Option aus. Klicken Sie auf dem Blatt **Recovery Services-Tresore** auf **+ Erstellen**.

1. Geben Sie auf dem Blatt **Recovery Services-Tresor erstellen** die folgenden Einstellungen an:

    | Einstellungen | Wert |
    | --- | --- |
    | Abonnement | der Name Ihres Azure-Abonnements |
    | Resource group | `az104-rg-region2` (Wählen Sie bei Bedarf **Neu erstellen** aus.) |
    | Tresorname | `az104-rsv-region2` |
    | Region | **USA, Westen** |

    >**Hinweis:** Geben Sie unbedingt eine **andere** Region als für die VM angeben.

1. Klicken Sie auf **Überprüfen + erstellen**, stellen Sie sicher, dass die Überprüfung erfolgreich war, und klicken Sie dann auf **Erstellen**.

    >**Hinweis**: Warten Sie, bis die Bereitstellung abgeschlossen ist. Die Bereitstellung dürfte einige Minuten dauern. 

1. Suchen Sie nach der VM `az104-10-vm0`, und wählen Sie sie aus.

1. Wählen Sie auf dem Blatt **Sicherung + Notfallwiederherstellung** die Option **Notfallwiederherstellung** aus. 

1. Wählen Sie **Replikation aktivieren** aus.

1. Beachten Sie auf der Registerkarte **Grundlagen** den Wert für **Zielregion**.

1. Wechseln Sie zur Registerkarte **Erweiterte Einstellungen**. Die Ressourcenauswahl wurde für Sie getroffen. Es ist wichtig, sie zu überprüfen. 

1. Überprüfen Sie die Einstellungen für Abonnement, VM-Ressourcengruppe, virtuelles Netzwerk und Verfügbarkeit (übernehmen Sie hierfür die Standardeinstellung).

1. Wählen Sie unter **Speichereinstellungen** die Option ** Details anzeigen** aus.

    | Einstellung | Wert |
    | ---- | ---- |
    | Churn für die VM | **Normaler Churn**  |
    | Cachespeicherkonto | **(neu) xxx**  |

   >**Hinweis:** Es ist wichtig, dass beide Einstellungen angegeben werden, andernfalls ist die Überprüfung nicht erfolgreich. Wenn keine Werte vorhanden sind, versuchen Sie, die Seite zu aktualisieren. Wenn dies nicht funktioniert, erstellen Sie ein leeres Speicherkonto, und kehren Sie dann zu dieser Seite zurück.

1. Wählen Sie unter **Replikationseinstellungen** die Option ** Details anzeigen** aus. Beachten Sie, dass Ihr Wiederherstellungsressourcentresor in Region 2 automatisch ausgewählt wurde.

1. Wählen Sie **Replikation überprüfen und starten** und dann **Replikation aktivieren** aus.

    >**Hinweis:** Die Aktivierung der Replikation dauert 10 bis 15 Minuten. Sehen Sie sich die Benachrichtigungsmeldungen in der oberen rechten Ecke des Portals an. Während Sie warten, überprüfen Sie ggf. die eigenverantwortlichen Trainingslinks am Ende dieser Seite.
    
1. Nachdem die Replikation abgeschlossen ist, suchen Sie nach dem Recovery Services-Tresor **az104-rsv-region2**. Möglicherweise müssen Sie auf **Aktualisieren** klicken, um die Seite zu aktualisieren. 

1. Wählen Sie im Abschnitt **Geschützte Elemente** die Option **Replizierte Elemente** aus.

1. Überprüfen Sie, ob die Replikationsintegrität der VM als fehlerfrei angezeigt wird. Beachten Sie, dass unter „Status“ der Synchronisierungsstatus (beginnend bei 0 %) und schließlich **Geschützt** angezeigt wird, nachdem die Erstsynchronisierung abgeschlossen wurde.

   ![Screenshot der Seite „Replizierte Elemente“.](../media/az104-lab10-replicated-items.png)

1. Wählen Sie die VM aus, um weitere Details anzuzeigen.
   
>**Schon gewusst?** Es empfiehlt sich, das [Failover einer geschützten VM zu testen](https://learn.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure#run-a-test-failover-for-a-single-vm).

## Bereinigen Ihrer Ressourcen

Wenn Sie mit **Ihrem eigenen Abonnement** arbeiten, nehmen Sie sich eine Minute Zeit, um die Labressourcen zu löschen. Dadurch wird sichergestellt, dass Ressourcen freigegeben und Kosten minimiert werden. Die einfachste Möglichkeit zum Löschen der Labressourcen besteht darin, die Ressourcengruppe des Labs zu löschen. 

+ Wählen Sie im Azure-Portal die Ressourcengruppe und dann **Ressourcengruppe löschen** aus, **geben Sie den Ressourcengruppennamen ein**, und klicken Sie dann auf **Löschen**.
+ Bei Verwendung von Azure PowerShell: `Remove-AzResourceGroup -Name resourceGroupName`.
+ Bei Verwendung der Befehlszeilenschnittstelle: `az group delete --name resourceGroupName`.


## Wichtige Erkenntnisse

Herzlichen Glückwunsch zum erfolgreichen Abschluss des Labs. Hier sind die wichtigsten Erkenntnisse für dieses Lab. 

+ Der Azure Backup-Dienst bietet einfache, sichere und kostengünstige Lösungen zum Sichern und Wiederherstellen Ihrer Daten.
+ Azure Backup kann lokale und cloudbasierte Ressourcen schützen, einschließlich VMs und Dateifreigaben.
+ Azure Backup-Richtlinien konfigurieren die Häufigkeit von Sicherungen und den Aufbewahrungszeitraum für Wiederherstellungspunkte. 
+ Azure Site Recovery ist eine Notfallwiederherstellungslösung, die Schutz für Ihre VMs und Anwendungen bietet.
+ Azure Site Recovery repliziert Ihre Workloads an einem sekundären Standort. So können Sie bei einem Ausfall oder Notfall ein Failover auf den sekundären Standort durchführen und Vorgänge mit minimaler Downtime fortsetzen.
+ Ein Recovery Services-Tresor speichert Ihre Sicherungsdaten und minimiert den Verwaltungsaufwand.

## Weiterlernen im eigenen Tempo

+ [Schützen Ihrer virtuellen Computer mithilfe von Azure Backup](https://learn.microsoft.com/training/modules/protect-virtual-machines-with-azure-backup/) Verwenden Sie Azure Backup, um lokale Server, virtuelle Computer, SQL Server, Azure-Dateifreigaben und andere Workloads zu schützen.
+ [Schützen Ihrer Azure-Infrastruktur  mit Azure Site Recovery](https://learn.microsoft.com/en-us/training/modules/protect-infrastructure-with-site-recovery/). Richten Sie die Notfallwiederherstellung für Ihre Azure-Infrastruktur ein, indem Sie Replikationen, Failover und Failback für Azure-VMs mit Azure Site Recovery anpassen.
