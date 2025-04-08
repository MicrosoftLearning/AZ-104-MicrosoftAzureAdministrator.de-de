---
lab:
  title: 'Lab 07: Verwalten von Azure Storage'
  module: Administer Azure Storage
---

# Lab 07: Verwalten von Azure-Speicher

## Einführung in das Lab

In diesem Lab lernen Sie, Speicherkonten für Azure-Blobs und Azure-Dateien zu erstellen. Sie lernen, Blobcontainer zu konfigurieren und zu sichern. Außerdem lernen Sie, den Speicherbrowser zum Konfigurieren und Sichern von Azure-Dateifreigaben zu verwenden. 

Für dieses Lab wird ein Azure-Abonnement benötigt. Ihr Abonnementtyp kann sich auf die Verfügbarkeit von Features in diesem Lab auswirken. Die Region kann geändert werden. In den Schritten wird allerdings die Region **USA, Osten** verwendet.

## Geschätzte Zeit: 50 Minuten

## Labszenario

Ihre Organisation speichert derzeit Daten in lokalen Datenspeichern. Auf die meisten dieser Dateien wird nicht häufig zugegriffen. Sie möchten die Kosten für Speicher minimieren, indem Sie Dateien mit seltenem Zugriff auf kostengünstigeren Speicherebenen platzieren. Außerdem möchten Sie verschiedene Schutzmechanismen untersuchen, die Azure Storage bietet, einschließlich Netzwerkzugriff, Authentifizierung, Autorisierung und Replikation. Schließlich möchten Sie ermitteln, in welchem Umfang Azure Files zum Hosten Ihrer lokalen Dateifreigaben geeignet ist.

## Interaktive Labsimulationen

Für dieses Thema stehen hilfreiche interaktive Labsimulationen zur Verfügung. In der Simulation können Sie sich in Ihrem eigenen Tempo durch ein ähnliches Szenario klicken. Es gibt zwar Unterschiede zwischen der interaktiven Simulation und diesem Lab, aber viele der Kernkonzepte sind identisch. Ein Azure-Abonnement ist nicht erforderlich. 

+ [Erstellen eines Blobspeichers](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%205). Erstellen Sie ein Speicherkonto, verwalten Sie Blobspeicher, und überwachen Sie Speicheraktivitäten. 
  
+ [Verwalten von Azure-Speicher](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2011). Erstellen Sie ein Speicherkonto, und überprüfen Sie die Konfiguration. Verwalten von Blobspeichercontainern Konfigurieren Sie das Speichernetzwerk. 

## Architekturdiagramm

![Diagramm der Aufgaben](../media/az104-lab07-architecture.png)

## Stellenqualifikationen

+ Aufgabe 1: Erstellen und Konfigurieren eines Speicherkontos 
+ Aufgabe 2: Erstellen und Konfigurieren von sicherem Blobspeicher.
+ Aufgabe 3: Erstellen und Konfigurieren von sicherem Azure-Dateispeicher.

## Aufgabe 1: Erstellen und Konfigurieren eines Speicherkontos 

In dieser Aufgabe werden Sie ein Speicherkonto erstellen und konfigurieren. Das Speicherkonto wird georedundanten Speicher verwenden und keinen öffentlichen Zugriff haben. 

1. Melden Sie sich beim **Azure-Portal** - `https://portal.azure.com` an.

1. Suchen Sie nach `Storage accounts` und wählen dies aus, und klicken Sie dann auf **+ Erstellen**.

1. Geben Sie auf dem Blatt **Speicherkonto erstellen** auf der Registerkarte **Grundeinstellungen** die folgenden Einstellungen an (belassen Sie andere auf den Standardwerten):

    | Einstellung | Wert |
    | --- | --- |
    | Abonnement          | der Name Ihres Azure-Abonnements  |
    | Resource group        | **az104-rg7** (neu erstellen) |
    | Speicherkontoname  | Ein beliebiger global eindeutiger Name, der zwischen 3 und 24 Zeichen lang ist und aus Buchstaben und Ziffern besteht. |
    | Region                | **(USA) USA, Osten**  |
    | Leistung           | **Standard** (beachten Sie die Premium-Option) |
    | Redundanz            | **Georedundanter Speicher** (beachten Sie die anderen Optionen)|
    | Stellen Sie bei regionaler Verfügbarkeit Lesezugriff auf die Daten bereit | Aktivieren Sie das Kontrollkästchen. |

    >**Schon gewusst?** Sie sollten die Leistungsstufe „Standard“ für die meisten Anwendungen verwenden. Verwenden Sie die Leistungsstufe „Premium“ für Unternehmens- oder Hochleistungsanwendungen. 

1. Verwenden Sie auf der Registerkarte **Erweitert** die Informationssymbole, um mehr über die Auswahlmöglichkeiten zu erfahren. Standardwerte übernehmen 

1. Wählen Sie auf der Registerkarte **Netzwerke** im Abschnitt **Öffentlicher Netzwerkzugang** die Option **Deaktivieren**. Dadurch wird der eingehende Zugriff beschränkt, während der ausgehende Zugriff zugelassen wird. 

1. Überprüfen Sie die Registerkarte **Datenschutz**. Beachten Sie, dass 7 Tage die standardmäßige Aufbewahrungsrichtlinie für vorläufiges Löschen ist. Beachten Sie, dass Sie die Versionsverwaltung für Blobs aktivieren können. Übernehmen Sie die Standardeinstellungen.

1. Überprüfen Sie die Registerkarte **Verschlüsselung**. Beachten Sie die zusätzlichen Sicherheitsoptionen. Übernehmen Sie die Standardeinstellungen.

1. Wählen Sie **Überprüfen + Erstellen**, warten Sie, bis der Validierungsprozess abgeschlossen ist, und klicken Sie dann auf **Erstellen**.

1. Sobald das Speicherkonto bereitgestellt ist, wählen Sie **Zur Ressource wechseln** aus.

1. Überprüfen Sie das Blatt **Übersicht** und die zusätzlichen Konfigurationen, die geändert werden können. Dies sind globale Einstellungen für das Speicherkonto. Beachten Sie, dass das Speicherkonto für Blobcontainer, Dateifreigaben, Warteschlangen und Tabellen verwendet werden kann.

1. Wählen Sie auf dem Blatt **Sicherheit und Netzwerk** die Option **Netzwerk** aus. Hinweis: Der **öffentliche Netzwerkzugang** ist deaktiviert.

    + Wählen Sie unter **Öffentlicher Netzwerkzugang** die Option **Verwalten** aus.
    + Ändern Sie **Öffentlicher Netzwerkzugang** in **Aktivieren**.
    + Ändern Sie die **Standardaktion** in **Von ausgewählten Netzwerken aktivieren**.
    + Wählen Sie im Abschnitt **IP-Adressen** die Option **IP-Adresse des Clients hinzufügen**.
    + **Speichern** Sie die Änderungen.
  
1. Wählen Sie im Blatt **Datenverwaltung** die Option **Redundanz** aus. Beachten Sie die Informationen zu Ihren primären und sekundären Rechenzentrumsstandorten.

1. Wählen Sie im Blatt **Datenverwaltung** die Option **Lebenszyklusverwaltung** und anschließend die Option **Regel hinzufügen**.

    + **Benennen Sie** die Regel `Movetocool`. Beachten Sie Ihre Optionen zum Einschränken des Geltungsbereichs der Regel.
    
    + *Wenn* die auf der Registerkarte **Basisblobs** basierten Blobs vor mehr als vor der `30 days` geändert wurden, *dann* **wechseln Sie zur kalten Speicherebene**. Es gibt weitere Optionen. 
    
    + Beachten Sie, dass Sie andere Bedingungen konfigurieren können. Wählen Sie **Hinzufügen** aus, wenn Sie mit der Erkundung fertig sind.

    ![Screenshot: Regelbedingungen für wechseln zur kalten Speicherebene.](../media/az104-lab07-movetocool.png)

## Aufgabe 2: Erstellen und Konfigurieren von sicherem Blobspeicher

In dieser Aufgabe erstellen Sie einen Blobcontainer und laden ein Bild herauf. Blobcontainer sind verzeichnisähnliche Strukturen, die unstrukturierte Daten speichern.

### Erstellen eines Blobcontainers und einer zeitbasierten Aufbewahrungsrichtlinie

1. Fahren Sie im Azure-Portal fort, und arbeiten Sie mit Ihrem Speicherkonto.

1. Wählen Sie im Blatt **Datenspeicher** die Option **Container**. 

1. Klicken Sie auf **+ Container**, und **erstellen** Sie einen Container mit den folgenden Einstellungen:

    | Einstellung | Wert |
    | --- | --- |
    | Name | `data`  |
    | Öffentliche Zugriffsebene | Beachten Sie, dass die Zugriffsebene auf „privat“ festgelegt ist |

    ![Screenshot des Erstellens eines Containers.](../media/az104-lab07-create-container.png)

1. Scrollen Sie in Ihrem Container ganz nach rechts zu den Auslassungspunkten (...) und wählen Sie **Zugriffsrichtlinie** aus.

1. Wählen Sie im Bereich **Unveränderlicher Blobspeicher** die Option **Richtlinie hinzufügen** aus.

    | Einstellung | Wert |
    | --- | --- |
    | Richtlinientyp | **Zeitbasierte Aufbewahrung**  |
    | Aufbewahrungszeitraum festlegen für | `180` Tage |

1. Wählen Sie **Speichern**.

### Verwalten von Blobuploads

1. Kehren Sie zur Seite „Container“ zurück, wählen Sie Ihren **Daten**-Container aus, und klicken Sie dann auf **Hochladen**.

1. Erweitern Sie auf dem Blatt **Blob hochladen** den Abschnitt **Erweitert**.

    >**Hinweis:** Suchen Sie eine Datei zum Hochladen. Dies kann ein beliebiger Dateityp sein, aber eine kleine Datei ist am besten geeignet. Eine Beispieldatei kann aus dem Verzeichnis „AllFiles“ heruntergeladen werden. 

    | Einstellung | Wert |
    | --- | --- |
    | Nach Dateien durchsuchen | fügen Sie die von Ihnen ausgewählte Datei zum Hochladen hinzu |
    | Wählen Sie **Erweitert** aus. | |
    | Blobtyp | **Blockblob** |
    | Blockgröße | **4 MiB** |
    | Zugriffsebene | **Heiße Speicherebene**  (beachten Sie die anderen Optionen) |
    | In Ordner hochladen | `securitytest` |
    | Verschlüsselungsbereich | Vorhandenen Standardcontainerbereich verwenden |

1. Klicken Sie auf **Hochladen**.

1. Vergewissern Sie sich, dass Sie über einen neuen Ordner verfügen und Ihre Datei hochgeladen wurde. 

1. Wählen Sie Ihre Uploaddatei aus, und überprüfen Sie die Optionen wie **Herunterladen**, **Löschen**, **Änderungsebene**und **Lease erwerben**.

1. Kopieren Sie die **URL** der Datei (Eigenschaftenblatt) und fügen Sie sie in ein neues **InPrivate**-Browerfenster ein.

1. Es sollte eine XML-formatierte Meldung mit dem Hinweis **ResourceNotFound** oder **PublicAccessNotPermitted** angezeigt werden.

    > **Hinweis**: Dies ist zu erwarten, da für den Container, den Sie erstellt haben, die öffentliche Zugriffsebene auf **Privat (kein anonymer Zugriff)** festgelegt ist.

### Konfigurieren des eingeschränkten Zugriffs auf den Blobspeicher

1. Wählen Sie Ihre hochgeladene Datei und dann auf der Registerkarte **SAS generieren** aus. Sie können auch die Auslassungspunkte (...) ganz rechts verwenden. Geben Sie die folgenden Einstellungen an (belassen Sie andere auf ihren Standardwerten):

    | Einstellung | Wert |
    | --- | --- |
    | Signaturschlüssel | **Schlüssel 1** |
    | Berechtigungen | **Lesen** (beachten Sie Ihre anderen Auswahlmöglichkeiten) |
    | Startdatum | gestriges Datum |
    | Startzeit | Aktuelle Uhrzeit |
    | Ablaufdatum | Datum von morgen |
    | Ablaufzeit | Aktuelle Uhrzeit |
    | Zulässige IP-Adressen | Lassen Sie dieses Feld leer. |

1. Klicken Sie auf **SAS-Token und URL generieren**.

1. Kopieren Sie den **Blob SAS-URL**-Eintrag in die Zwischenablage.

1. Öffnen Sie ein weiteres InPrivate-Browserfenster, und navigieren Sie zur Blob SAS-URL, die Sie im vorherigen Schritt kopiert haben.

    >**Hinweis:** Sie sollten den Inhalt der Datei anzeigen können. 

## Aufgabe 3: Erstellen und Konfigurieren von Azure-Dateispeicher

In dieser Aufgabe werden Sie Azure-Dateifreigaben erstellen und konfigurieren. Sie werden den Speicherbrowser verwenden, um die Dateifreigabe zu verwalten. 

### Erstellen der Dateifreigabe und Hochladen einer Datei

1. Navigieren Sie im Azure-Portal zurück zu Ihrem Speicherkonto und klicken Sie im Blatt **Datenspeicher** auf **Dateifreigaben**.

1. Klicken Sie auf **+ Dateifreigabe** und geben Sie der Dateifreigabe auf der Registerkarte **Grundlagen** den Namen `share1`. 

1. Beachten Sie die Optionen für die **Zugriffsebene**. Belassen Sie den Standard **Transaktion optimiert**.
   
1. Wechseln Sie zur Registerkarte **Sicherung**, und stellen Sie sicher, dass **Sicherung aktivieren** **nicht** aktiviert ist. Wir deaktivieren die Sicherung, um die Labkonfiguration zu vereinfachen.

1. Klicken Sie auf **Überprüfen + erstellen** und dann auf **Erstellen**. Warten Sie, bis die Dateifreigabe bereitgestellt wurde.

    ![Screenshot der Seite „Dateifreigabe erstellen“.](../media/az104-lab07-create-share.png)

### Erkunden des Speicherbrowsers und Hochladen einer Datei

1. Kehren Sie zu Ihrem Speicherkonto zurück, und wählen Sie **Speicherbrowser** aus. Der Azure-Speicherbrowser ist ein Portaltool, mit dem Sie schnell alle Speicherdienste unter Ihrem Konto anzeigen können.

1. Wählen Sie **Dateifreigaben** aus, und überprüfen Sie, ob Ihr **share1**-Verzeichnis vorhanden ist.

1. Wählen Sie Ihr **share1**-Verzeichnis aus, und beachten Sie, dass Sie das **+ Verzeichnis hinzufügen** wählen können. Auf diese Weise können Sie eine Ordnerstruktur erstellen.

1. Wählen Sie die Option **Hochladen**. Navigieren Sie zu einer Datei Ihrer Wahl, und klicken Sie dann auf **Hochladen**.

    >**Hinweis:** Sie können Dateifreigaben anzeigen und diese Freigaben im Speicherbrowser verwalten. Es gibt derzeit keine Einschränkungen.

### Einschränken des Netzwerkzugriffs auf das Speicherkonto

1. Suchen Sie im Portal nach der Option **Virtuelle Netzwerke** und wählen Sie sie aus.

1. Wählen Sie **+ Erstellen** aus. Wählen Sie Ihre Ressourcengruppe aus. und geben Sie dem virtuellen Netzwerk einen **Namen**, `vnet1`.

1. Übernehmen Sie die Standardwerte für andere Parameter, und wählen Sie **Überprüfen + erstellen** und dann **Erstellen** aus.

1. Warten Sie, bis das virtuelle Netzwerk bereitgestellt ist, und wählen Sie dann **Zur Ressource wechseln** aus.

1. Wählen Sie im **Abschnitt "Einstellungen** " das **Blatt "Dienstendpunkte" aus**.
    + Wählen Sie **Hinzufügen**. 
    + Wählen Sie in der **Dropdownliste "Dienste** " "Microsoft.Storage **"** aus.
    + Überprüfen Sie in der **Dropdownliste "Subnetze** " das **Standardsubnetz**.
    + Klicken Sie auf **"Hinzufügen"**, um Ihre Änderungen zu speichern.  

1. Kehren Sie zu Ihrem Speicherkonto zurück.

1. Wählen Sie auf dem Blatt **Sicherheit und Netzwerk** die Option **Netzwerk** aus.

1. Wählen Sie **Vorhandenes virtuelles Netzwerk hinzufügen** und dann **vnet1** und das Subnetz **Standard** aus, und wählen Sie **Hinzufügen**.

1. **Löschen Sie** im Abschnitt **Firewall ** Ihre Computer-IP-Adresse. Zulässiger Datenverkehr sollte nur aus dem virtuellen Netzwerk stammen. 

1. Klicken Sie auf **Speichern**, um die Änderungen zu speichern.

    >**Hinweis:** Auf das Speicherkonto sollte jetzt nur über das von Ihnen soeben erstellte virtuelle Netzwerk zugegriffen werden. 

1. Wählen Sie den **Speicherbrowser** aus, und **aktualisieren** Sie die Seite. Navigieren Sie zu Ihrer Dateifreigabe oder zum Blobinhalt.  

    >**Hinweis:** Sie sollten eine Nachricht *nicht autorisiert zum Durchführen dieses Vorgangs* erhalten. Sie verbinden sich nicht über das virtuelle Netzwerk. Es kann einige Minuten dauern, bis dies wirksam wird.


![Screenshot des nicht autorisierten Zugriffs.](../media/az104-lab07-notauthorized.png)

## Bereinigen Ihrer Ressourcen

Wenn Sie mit **Ihrem eigenen Abonnement** arbeiten, nehmen Sie sich eine Minute Zeit, um die Labressourcen zu löschen. Dadurch wird sichergestellt, dass Ressourcen freigegeben und Kosten minimiert werden. Die einfachste Möglichkeit zum Löschen der Labressourcen besteht darin, die Ressourcengruppe des Labs zu löschen. 

+ Wählen Sie im Azure-Portal die Ressourcengruppe und dann **Ressourcengruppe löschen** aus, **geben Sie den Ressourcengruppennamen ein**, und klicken Sie dann auf **Löschen**.
+ Bei Verwendung von Azure PowerShell: `Remove-AzResourceGroup -Name resourceGroupName`.
+ Bei Verwendung der Befehlszeilenschnittstelle: `az group delete --name resourceGroupName`.

## Erweitern Ihrer Lernerfahrung mit Copilot
Copilot kann Sie beim Erlernen der Verwendung von Azure-Skripttools unterstützen. Copilot kann Sie auch in Bereichen unterstützen, die nicht im Lab behandelt werden oder in denen Sie weitere Informationen benötigen. Öffnen Sie einen Edge-Browser, und wählen Sie „Copilot“ (rechts oben) aus, oder navigieren Sie zu *copilot.microsoft.com*. Nehmen Sie sich einige Minuten Zeit, um diese Prompts auszuprobieren.

+ Erstelle ein Azure PowerShell-Skript für die Erstellung eines Speicherkontos mit einem Blobcontainer. 
+ Erstelle eine Prüfliste, mit der ich sicherstellen kann, dass mein Azure Storage-Konto sicher ist.
+ Erstelle eine Tabelle zum Vergleichen von Azure Storage-Redundanzmodellen.

## Weiterlernen im eigenen Tempo

+ [Optimieren Ihrer Kosten mit Azure Blob Storage](https://learn.microsoft.com/training/modules/optimize-your-cost-azure-blob-storage/). Erfahren Sie, wie Sie Ihre Kosten mit Azure Blob Storage optimieren.
+ [Steuern des Zugriffs auf Azure Storage mit freigegebenen Zugriffssignaturen](https://learn.microsoft.com/training/modules/control-access-to-azure-storage-with-sas/). Gewähren Sie unter Verwendung von Shared Access Signatures (SAS) sicheren Zugriff auf die in Ihren Azure Storage-Konten gespeicherten Daten.

## Wichtige Erkenntnisse

Herzlichen Glückwunsch zum erfolgreichen Abschluss des Labs. Hier sind die wichtigsten Erkenntnisse für dieses Lab. 

+ Ein Azure-Speicherkonto enthält alle Ihre Azure Storage-Datenobjekte: Blobs, Dateien, Warteschlangen und Tabellen. Das Speicherkonto stellt einen eindeutigen Namespace für Ihre Azure Storage-Daten bereit, auf den von jedem Ort der Welt aus über HTTP oder HTTPS zugegriffen werden kann.
+ Azure-Speicher bietet mehrere Redundanzmodelle, einschließlich lokal redundantem Speicher (LRS), zonenredundantem Speicher (ZRS) und georedundantem Speicher (GRS). 
+ Mit Azure-Blobspeicher können Sie große Mengen unstrukturierter Daten auf der Datenspeicherplattform von Microsoft speichern. Der Begriff „Blob“ steht für „Binary Large Object“, wozu Objekte wie Bilder und Multimediadateien zählen.
+ Azure-Dateispeicher bietet gemeinsamen Speicher für strukturierte Daten. Die Daten können in Ordnern organisiert werden.
+ Die Funktion für unveränderlichen Speicher bietet die Möglichkeit, Daten im WORM-Zustand (Write Once, Read Many) zu speichern. Richtlinien für unveränderlichen Speicher können auf Zeit oder gesetzlicher Aufbewahrungspflicht basiert sein.
