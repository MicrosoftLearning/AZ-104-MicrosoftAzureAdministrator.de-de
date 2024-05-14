---
lab:
  title: 'Lab 03: Verwalten von Azure-Ressourcen mithilfe von Azure Resource Manager-Vorlagen'
  module: Administer Azure Resources
---

# Lab 03 – Verwalten von Azure-Ressourcen mithilfe von Azure Resource Manager-Vorlagen

## Einführung

In diesem Lab erfahren Sie, wie Sie Ressourcenbereitstellungen automatisieren. Sie erfahren mehr über Azure Resource Manager-Vorlagen und Bicep-Vorlagen. Sie erfahren mehr über die verschiedenen Möglichkeiten der Bereitstellung der Vorlagen. 

Für dieses Lab wird ein Azure-Abonnement benötigt. Ihr Abonnementtyp kann sich auf die Verfügbarkeit von Features in diesem Lab auswirken. Die Region kann geändert werden. In den Schritten wird allerdings die Region **USA, Osten** verwendet. 

## Geschätzte Zeit: 50 Minuten

## Interaktive Labsimulation

Für dieses Thema stehen hilfreiche interaktive Labsimulationen zur Verfügung. In der Simulation können Sie sich in Ihrem eigenen Tempo durch ein ähnliches Szenario klicken. Es gibt zwar Unterschiede zwischen der interaktiven Simulation und diesem Lab, aber viele der Kernkonzepte sind identisch. Ein Azure-Abonnement ist nicht erforderlich. 

+ [Verwalten von Azure-Ressourcen mithilfe von Azure Resource Manager-Vorlagen](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%205). Überprüfen und erstellen Sie verwaltete Datenträgern mit einer Vorlage und stellen diese bereit.
  
+ [Erstellen einer VM mit einer Vorlage](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%209). Stellen Sie eine VM mit einer Schnellstartvorlage bereit.
  
## Labszenario

Ihr Team möchte Möglichkeiten zum Automatisieren und Vereinfachen von Ressourcenbereitstellungen untersuchen. Ihre Organisation sucht nach Möglichkeiten, den Verwaltungsaufwand zu reduzieren, menschliche Fehler zu reduzieren und die Konsistenz zu erhöhen.  

## Architekturdiagramm

![Diagramm der Aufgaben](../media/az104-lab03-architecture.png)

## Stellenqualifikationen

+ Aufgabe 1: Erstellen einer Azure Resource Manager-Vorlage
+ Aufgabe 2: Bearbeiten einer Azure Resource Manager-Vorlage und erneutes Bereitstellen der Vorlage
+ Aufgabe 3: Konfigurieren der Cloud Shell und Bereitstellen einer Vorlage mit Azure PowerShell
+ Aufgabe 4: Bereitstellen einer Vorlage mit der CLI 
+ Aufgabe 5: Stellen Sie eine Ressource mithilfe von Azure Bicep bereit.

## Aufgabe 1: Erstellen einer Azure Resource Manager-Vorlage

In dieser Aufgabe werden wir einen verwalteten Datenträger im Azure-Portal erstellen. Verwaltete Datenträger sind Speicher, der für die Verwendung mit VMs konzipiert ist. Sobald der Datenträger bereitgestellt wurde, werden Sie eine Vorlage exportieren, die Sie in anderen Bereitstellungen verwenden können.

1. Melden Sie sich beim **Azure-Portal** - `https://portal.azure.com` an.

1. Suchen Sie nach `Disks`, und wählen Sie diese Option aus.

1. Wählen Sie auf der Seite „Datenträger“ die Option **Erstellen** aus.

1. Konfigurieren Sie auf der Seite **Verwaltete Datenträger erstellen** den Datenträger, und wählen Sie dann **OK** aus. 
    
    | Einstellung | Wert |
    | --- | --- |
    | Abonnement | *Ihr Abonnement* | 
    | Ressourcengruppe | `az104-rg3` (Wählen Sie bei Bedarf **Neu erstellen** aus.)
    | Name des Datenträgers | `az104-disk1` | 
    | Region | **USA, Osten** |
    | Verfügbarkeitszone | **Keine Infrastrukturredundanz erforderlich** | 
    | Quellentyp | **Keine** |
    | Leistung | **HDD Standard** (Größe ändern) |
    | Größe | **32 GiB** | 

    >**Hinweis:** Wir erstellen einen einfachen verwalteten Datenträger, damit Sie mit Vorlagen üben können. Verwaltete Azure-Datenträger sind Speichervolumes auf Blockebene, die von Azure verwaltet werden.

1. Wählen Sie **Bewerten + Erstellen** und dann **Erstellen** aus.

1. Überwachen Sie die Benachrichtigungen (oben rechts), und wählen Sie nach der Bereitstellung **Zur Ressource wechseln** aus. 

1. Wählen Sie im Blatt **Automatisierung** die Option **Vorlage exportieren** aus. 

1. Nehmen Sie sich eine Minute Zeit, um die Dateien für **Vorlagen** und **Parameter** zu überprüfen.

1. Klicken Sie auf **Herunterladen**, und speichern Sie die Vorlagen auf dem lokalen Laufwerk. Dadurch wird eine komprimierte ZIP-Datei erstellt. 

1. Verwenden Sie den Datei-Explorer, um den Inhalt der heruntergeladenen Datei in den Ordner **Downloads** auf Ihrem Computer zu extrahieren. Beachten Sie, dass es zwei JSON-Dateien (Vorlage und Parameter) gibt. 

   >**Schon gewusst?**  Sie können eine gesamte Ressourcengruppe oder nur bestimmte Ressourcen innerhalb dieser Ressourcengruppe exportieren.

## Aufgabe 2: Bearbeiten einer Azure Resource Manager-Vorlage und erneutes Bereitstellen der Vorlage

In dieser Aufgabe verwenden Sie die heruntergeladene Vorlage, um einen neuen verwalteten Datenträger bereitzustellen. Diese Aufgabe beschreibt, wie Sie Bereitstellungen schnell und einfach wiederholen können. 

1. Suchen Sie im Azure-Portal nach `Deploy a custom template` und wählen Sie es aus.

1. Beachten Sie auf dem Blatt **Benutzerdefinierte Bereitstellung**, dass es die Möglichkeit gibt, eine **Schnellstartvorlage** zu verwenden. Es gibt viele integrierte Vorlagen, wie im Dropdownmenü gezeigt. 

1. Anstatt einen Schnellstart zu verwenden, wählen Sie **Eigene Vorlage im Editor erstellen** aus.

1. Klicken Sie auf dem Blatt **Vorlage bearbeiten** auf **Datei laden**, und laden Sie die Datei **template.json** hoch, die Sie auf ihren lokalen Datenträger heruntergeladen haben.

1. Nehmen Sie im Editorbereich diese Änderungen vor.

    + Ändern Sie **disks_az104_disk1_name** in `disk_name` (zwei zu ändernde Stellen)
    + Ändern Sie **az104-disk1** in `az104-disk2` (eine zu ändernde Stelle).

1. Beachten Sie, dass es sich um einen **Standard**-Datenträger handelt. Der Speicherort ist **eastus**. Die Datenträgergröße beträgt **32 GB**.

1. **Speichern** Sie die Änderungen.

1. Vergessen Sie nicht die Parameterdatei. Wählen Sie **Parameter bearbeiten** aus, klicken Sie auf **Datei laden**, und laden Sie **parameters.json** hoch. 

1. Nehmen Sie diese Änderung vor, damit sie mit der Vorlagendatei übereinstimmt.

    Ändern Sie **disks_az104_disk1_name** in **disk_name** (eine zu ändernde Stelle)

1. **Speichern** Sie die Änderungen. 

1. Vervollständigen Sie die Einstellungen für die benutzerdefinierte Bereitstellung:

    | Einstellung | Wert |
    | --- |--- |
    | Abonnement | *Ihr Abonnement* |
    | Ressourcengruppe | `az104-rg3` |
    | Region | **(USA) USA, Osten** |
    | Disk_name | `az104-disk2` |

1. Wählen Sie **Überprüfen + erstellen** und anschließend **Erstellen** aus.

1. Wählen Sie **Zu Ressource wechseln** aus. Überprüfen Sie, ob **az104-disk2** erstellt wurde.

1. Wählen Sie auf dem Blatt **Übersicht** die Ressourcengruppe **az104-rg3** aus. Sie sollten nun über zwei Datenträger verfügen.
   
1. Klicken Sie im Abschnitt **Einstellungen** auf **Bereitstellungen**.

    >**Hinweis:** Alle Bereitstellungsdetails sind in der Ressourcengruppe dokumentiert. Es empfiehlt sich, die ersten vorlagenbasierten Bereitstellungen zu überprüfen, um den Erfolg vor der Verwendung der Vorlagen für Vorgänge im großen Stil sicherzustellen.

1. Wählen Sie eine Bereitstellung aus, und überprüfen Sie den Inhalt der Blätter **Eingabe** und **Vorlage**.

## Aufgabe 3: Konfigurieren der Cloud Shell und Bereitstellen einer Vorlage mit Azure PowerShell

In dieser Aufgabe arbeiten Sie mit Azure Cloud Shell und Azure PowerShell. Azure Cloud Shell ist ein interaktives, authentifiziertes, über den Browser zugängliches Terminal für die Verwaltung von Azure-Ressourcen. Sie bietet Ihnen die Flexibilität, die Shell-Umgebung auszuwählen, die sich am besten für Ihre Arbeitsweise eignet: Bash oder PowerShell. In dieser Aufgabe verwenden Sie PowerShell zum Bereitstellen einer Vorlage. 

1. Wählen Sie oben rechts im Azure-Portal das Symbol **Cloud Shell** aus. Alternativ können Sie direkt zu `https://shell.azure.com` navigieren.

   ![Screenshot des Cloud Shell-Symbols.](../media/az104-lab03-cloudshell-icon.png)

1. Wenn Sie aufgefordert werden, entweder **Bash** oder **PowerShell** auszuwählen, wählen Sie **PowerShell** aus. 

    >**Schon gewusst?**  Wenn Sie hauptsächlich mit Linux-Systemen arbeiten, fühlt sich Bash (CLI) vertrauter an. Wenn Sie hauptsächlich mit Windows-Systemen arbeiten, fühlt sich Azure PowerShell vertrauter an. 

1. Wählen Sie auf dem Bildschirm **Sie haben keinen Speicher eingebunden** die Option **Erweiterte Einstellungen anzeigen** aus, und stellen Sie die erforderlichen Informationen bereit. 

    >**Hinweis:** Da Sie mit der Cloud Shell arbeiten, sind ein Speicherkonto und eine Dateifreigabe erforderlich. 

    | Einstellungen | Werte |
    |  -- | -- |
    | Ressourcengruppe | **az104-rg3** |
    | Speicherkonto (Neues erstellen) | * muss global eindeutig sein, zwischen 3 und 24 Zeichen lang und darf nur Zahlen und Kleinbuchstaben enthalten* |
    | Dateifreigabe (Neue erstellen) | `fs-cloudshell` |

1. Wählen Sie nach Abschluss die Option **Speicher erstellen** aus. Sie müssen dies nur tun, wenn Sie die Cloud Shell zum ersten Mal verwenden. Es wird ein paar Minuten dauern, um den Speicher bereitzustellen.

1. Verwenden Sie das Symbol **Dateien hochladen/herunterladen**, um die Vorlage und die Parameterdatei aus dem Downloadverzeichnis hochzuladen. Sie müssen jede Datei separat hochladen.

1. Überprüfen Sie, ob Ihre Dateien im Cloud Shell-Speicher verfügbar sind. 

    ```powershell
    dir
    ```
    >**Hinweis:** Bei Bedarf können Sie **cls** verwenden, um das Befehlsfenster zu löschen. Sie können die Pfeiltasten verwenden, um den Befehlsverlauf zu verschieben.
   
1. Wählen Sie das Symbol **Editor** (geschweifte Klammern) aus, und navigieren Sie zur JSON-Vorlagendatei.

1. Führen Sie eine Änderung durch. Ändern Sie beispielsweise den Datenträgernamen in **az104-disk3**. Verwenden Sie **STRG+S**, um Ihre Änderungen zu speichern. 

    >**Hinweis:** Sie können als Ziel für Ihre Vorlagenbereitstellung eine Ressourcengruppe, ein Abonnement, eine Verwaltungsgruppe oder einen Mandanten verwenden. Abhängig vom Umfang der Bereitstellung verwenden Sie unterschiedliche Befehle.

1. Verwenden Sie **New-AzResourceGroupDeployment**, um eine Ressourcengruppe bereitzustellen.

    ```powershell
    New-AzResourceGroupDeployment -ResourceGroupName az104-rg3 -TemplateFile template.json -TemplateParameterFile parameters.json
    ```
1. Stellen Sie sicher, dass der Befehl abgeschlossen ist und der ProvisioningState **erfolgreich** ist.

1. Bestätigen Sie, dass der Datenträger erstellt wurde.

   ```powershell
   Get-AzDisk
   ```
   
## Aufgabe 4: Bereitstellen einer Vorlage mit der CLI 

1. Fahren Sie in der **Cloud Shell** fort und wählen Sie **Bash** aus. **Bestätigen** Sie Ihre Auswahl.

1. Überprüfen Sie, ob Ihre Dateien im Cloud Shell-Speicher verfügbar sind. Wenn Sie die vorherige Aufgabe abgeschlossen haben, sollten Ihre Vorlagendateien verfügbar sein. 

    ```sh
    ls
    ```

1. Wählen Sie das Symbol **Editor** (geschweifte Klammern) aus, und navigieren Sie zur JSON-Vorlagendatei.

1. Führen Sie eine Änderung durch. Ändern Sie beispielsweise den Datenträgernamen in **az104-disk4**. Verwenden Sie **STRG+S**, um Ihre Änderungen zu speichern. 

    >**Hinweis:** Sie können als Ziel für Ihre Vorlagenbereitstellung eine Ressourcengruppe, ein Abonnement, eine Verwaltungsgruppe oder einen Mandanten verwenden. Abhängig vom Umfang der Bereitstellung verwenden Sie unterschiedliche Befehle.

1. Für die Bereitstellung in einer Ressourcengruppe verwenden Sie **az deployment group create**.

    ```sh
    az deployment group create --resource-group az104-rg3 --template-file template.json --parameters parameters.json
    ```
    
1. Stellen Sie sicher, dass der Befehl abgeschlossen ist und der ProvisioningState **erfolgreich** ist.

1. Bestätigen Sie, dass der Datenträger erstellt wurde.

     ```sh
     az disk list --output table
     ```
   
## Aufgabe 5: Bereitstellen einer Ressource mithilfe von Azure Bicep

In dieser Aufgabe verwenden Sie eine Bicep-Datei, um einen verwalteten Datenträger bereitzustellen. Bicep ist ein deklaratives Automatisierungstool, das auf ARM-Vorlagen aufbaut.

1. Arbeiten Sie in der **Cloud Shell** in einer **Bash**-Sitzung weiter.

1. Suchen Sie die Datei **\Allfiles\Lab03\azuredeploydisk.bicep** und laden diese herunter.

1. **Laden Sie** die Bicep-Datei in die Cloud Shell hoch. 

1. Wählen Sie das Symbol **Editor** (geschweifte Klammern) aus, und navigieren Sie zu der Datei.

1. Nehmen Sie sich eine Minute Zeit, um die Bicep-Vorlagendatei durchzulesen. Beachten Sie, wie die Datenträgerressource definiert ist. 
   
1. Nehmen Sie die folgenden Änderungen vor:

    + Ändern Sie den Wert **managedDiskName** in `Disk4`.
    + Ändern Sie den Wert **sku name** in `StandardSSD_LRS`.
    + Ändern Sie den Wert **diskSizeinGiB** in `32`.

1. Verwenden Sie **STRG+S**, um Ihre Änderungen zu speichern.

1. Stellen Sie jetzt die Vorlage bereit.

    ```
    az deployment group create --resource-group az104-rg3 --template-file azuredeploydisk.bicep
    ```

1. Bestätigen Sie, dass der Datenträger erstellt wurde.

    ```sh
    az disk list --output table
    ```

    >**Hinweis:** Sie haben erfolgreich fünf verwaltete Datenträger bereitgestellt, jeden auf eine andere Weise. Gute Arbeit!

## Bereinigen Ihrer Ressourcen

Wenn Sie mit **Ihrem eigenen Abonnement** arbeiten, nehmen Sie sich eine Minute Zeit, um die Labressourcen zu löschen. Dadurch wird sichergestellt, dass Ressourcen freigegeben und Kosten minimiert werden. Die einfachste Möglichkeit zum Löschen der Labressourcen besteht darin, die Ressourcengruppe des Labs zu löschen. 

+ Wählen Sie im Azure-Portal die Ressourcengruppe und dann **Ressourcengruppe löschen** aus, **geben Sie den Ressourcengruppennamen ein**, und klicken Sie dann auf **Löschen**.
+ Bei Verwendung von Azure PowerShell: `Remove-AzResourceGroup -Name resourceGroupName`.
+ Bei Verwendung der Befehlszeilenschnittstelle: `az group delete --name resourceGroupName`.

## Erweitern Ihrer Lernerfahrung mit Copilot

Copilot kann Sie beim Erlernen der Verwendung von Azure-Skripttools unterstützen. Copilot kann Sie auch in Bereichen unterstützen, die nicht im Lab behandelt werden oder in denen Sie weitere Informationen benötigen. Öffnen Sie einen Edge-Browser, und wählen Sie „Copilot“ (rechts oben) aus, oder navigieren Sie zu *copilot.microsoft.com*. Nehmen Sie sich einige Minuten Zeit, um diese Prompts auszuprobieren.

+ Welches Format weist die Azure Resource Manager-Vorlagendatei auf? Erläutere jede Komponente anhand von Beispielen. 
+ Wie bearbeite ich eine vorhandene Azure Resource Manager-Vorlage?
+ Zeige mir die Gemeinsamkeiten und Unterschiede zwischen Azure Resource Manager-Vorlagen und Azure Bicep-Vorlagen. 


## Weiterlernen im eigenen Tempo

+ [Bereitstellen der Azure-Infrastruktur mithilfe von JSON ARM-Vorlagen](https://learn.microsoft.com/training/modules/create-azure-resource-manager-template-vs-code/). Schreiben Sie JSON Azure Resource Manager-Vorlagen (ARM-Vorlagen) mit Visual Studio Code, um Ihre Infrastruktur konsistent und zuverlässig in Azure bereitzustellen.
+ [Review der Features und Tools für Azure Cloud Shell](https://learn.microsoft.com/training/modules/review-features-tools-for-azure-cloud-shell/). Cloud Shell-Features und -Tools. 
+ [Verwalten von Azure-Ressourcen mit Windows PowerShell](https://learn.microsoft.com/training/modules/manage-azure-resources-windows-powershell/). In diesem Modul wird erläutert, wie Sie die erforderlichen Module für die Verwaltung von Clouddiensten installieren und PowerShell-Befehle verwenden, um einfache Verwaltungsaufgaben für Cloudressourcen wie Azure-VMs, Azure-Abonnements und Azure-Speicherkonten durchzuführen.
+ [Einführung in Bash](https://learn.microsoft.com/training/modules/bash-introduction/). Verwenden Sie Bash, um IT-Infrastruktur zu verwalten.
+ [Erstellen Ihrer ersten Bicep-Vorlage](https://learn.microsoft.com/training/modules/build-first-bicep-template/). Definieren Sie Azure-Ressourcen in einer Bicep-Vorlage. Verbessern Sie die Konsistenz und Zuverlässigkeit Ihrer Bereitstellungen, reduzieren Sie den erforderlichen manuellen Aufwand, und skalieren Sie Ihre Bereitstellungen für verschiedene Umgebungen. Durch die Verwendung von Parametern, Variablen, Ausdrücken und Modulen ist Ihre Vorlage flexibel und wiederverwendbar.

## Wichtige Erkenntnisse

Herzlichen Glückwunsch zum erfolgreichen Abschluss des Labs. Hier sind die wichtigsten Erkenntnisse für dieses Lab. 

+ Mit Azure Resource Manager-Vorlagen können Sie alle Ressourcen als Gruppe bereitstellen, verwalten und überwachen, statt diese Ressourcen einzeln zu verwalten.
+ Eine Azure Resource Manager-Vorlage ist eine JSON (JavaScript Object Notation)-Datei, mit der Sie Ihre Infrastruktur deklarativ statt mit Skripts verwalten können.
+ Anstatt Parameter als Inlinewerte in Ihrer Vorlage zu übergeben, können Sie eine separate JSON-Datei verwenden, welche die Parameterwerte enthält.
+ Azure Resource Manager-Vorlagen können auf unterschiedliche Weise bereitgestellt werden, einschließlich im Azure-Portal, über Azure PowerShell und die CLI.
+ Bicep ist eine Alternative zu Azure Resource Manager-Vorlagen. Bicep verwendet eine deklarative Syntax zum Bereitstellen von Azure-Ressourcen.
+ Bicep bietet eine präzise Syntax, zuverlässige Typsicherheit und Unterstützung für die Wiederverwendung von Code. Bicep bietet die beste Form der Dokumenterstellung für Ihre Infrastructure-as-Code-Lösungen in Azure.


