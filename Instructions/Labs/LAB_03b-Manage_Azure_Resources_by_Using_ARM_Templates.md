---
lab:
  title: 03b – Verwalten von Azure-Ressourcen mithilfe von ARM-Vorlagen
  module: Module 03 - Azure Administration
ms.openlocfilehash: 4d50e205d76db7bfeffd89a970ffcb42bd9c2f42
ms.sourcegitcommit: 8a0ced6338608682366fb357c69321ba1aee4ab8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2021
ms.locfileid: "132625495"
---
# <a name="lab-03b---manage-azure-resources-by-using-arm-templates"></a>Lab 03b – Verwalten von Azure-Ressourcen mithilfe von ARM-Vorlagen
# <a name="student-lab-manual"></a>Lab-Handbuch für Kursteilnehmer

## <a name="lab-scenario"></a>Labszenario
Nachdem Sie die grundlegenden Azure-Verwaltungsfunktionen im Zusammenhang mit der Bereitstellung von Ressourcen und deren Strukturierung basierend auf Ressourcengruppen mithilfe des Azure-Portals kennengelernt haben, führen Sie die entsprechenden Aufgaben jetzt mithilfe von Azure Resource Manager-Vorlagen aus.

## <a name="objectives"></a>Ziele

Dieses Lab deckt Folgendes ab:

+ Aufgabe 1: Überprüfen einer ARM-Vorlage für die Bereitstellung eines verwalteten Azure-Datenträgers
+ Aufgabe 2: Erstellen eines verwalteten Azure-Datenträgers mithilfe einer ARM-Vorlage
+ Aufgabe 3: Überprüfen der auf einer ARM-Vorlage basierenden Bereitstellung eines verwalteten Azure-Datenträgers

## <a name="estimated-timing-20-minutes"></a>Geschätzte Zeit: 20 Minuten

## <a name="architecture-diagram"></a>Architekturdiagramm

![image](../media/lab03b.png)

## <a name="instructions"></a>Anweisungen

### <a name="exercise-1"></a>Übung 1

#### <a name="task-1-review-an-arm-template-for-deployment-of-an-azure-managed-disk"></a>Aufgabe 1: Überprüfen einer ARM-Vorlage für die Bereitstellung eines verwalteten Azure-Datenträgers

In dieser Aufgabe erstellen Sie eine Azure-Datenträgerressource mithilfe einer Azure Resource Manager-Vorlage (ARM).

1. Melden Sie sich am [**Azure-Portal**](https://portal.azure.com) an.

1. Suchen Sie im Azure-Portal nach **Ressourcengruppen**, und wählen Sie die entsprechende Option aus. 

1. Klicken Sie in der Liste der Ressourcengruppen auf **az104-03a-rg1**.

1. Klicken Sie auf dem Blatt der Ressourcengruppe **az104-03a-rg1** im Abschnitt **Einstellungen** auf **Bereitstellungen**.

1. Klicken Sie auf dem Blatt **az104-03a-rg1 – Bereitstellungen** auf den ersten Eintrag in der Liste der Bereitstellungen.

1. Klicken Sie auf dem Blatt **Microsoft.ManagedDisk-* XXXXXXXXX* \| Übersicht** auf **Vorlagen**.

    >**Hinweis**: Überprüfen Sie den Inhalt der Vorlage, und beachten Sie das Vorhandensein der Optionen **Herunterladen** zum Download auf den lokalen Computer, **Zu Bibliothek hinzuzufügen** oder **Bereitstellen** für eine erneute Bereitstellung.

1. Klicken Sie auf **Herunterladen**, und speichern Sie die komprimierte Datei mit den Vorlagen- und Parameterdateien im Ordner **Downloads** auf Ihrem Lab-Computer.

1. Klicken Sie auf dem Blatt **Microsoft.ManagedDisk-* XXXXXXXXX* \| Vorlage** auf **Eingaben**.

1. Beachten Sie den Wert des Parameters **location**. Sie werden dies in der nächsten Aufgabe benötigen.

1. Extrahieren Sie den Inhalt der heruntergeladenen Datei in den Ordner **Downloads** auf Ihrem Lab-Computer.

    >**Hinweis**: Diese Dateien stehen auch als **\\Allfiles\\Labs\\03\\az104-03b-md-template.json** und **\\Allfiles\\Labs\\03\\az104-03b-md-parameters.json** zur Verfügung.
    
1. Schließen Sie alle **Datei-Explorer**-Fenster.

#### <a name="task-2-create-an-azure-managed-disk-by-using-an-arm-template"></a>Aufgabe 2: Erstellen eines verwalteten Azure-Datenträgers mithilfe einer ARM-Vorlage

1. Suchen Sie im Azure-Portal nach **Benutzerdefinierte Vorlage bereitstellen**, und wählen Sie sie aus.

1. Klicken Sie unterhalb der Gruppe **Marketplace** auf **Vorlagenbereitstellung (mit benutzerdefinierten Vorlagen bereitstellen)** .

1. Klicken Sie auf dem Blatt **Benutzerdefinierte Bereitstellung** auf **Eigene Vorlage im Editor erstellen**.

1. Klicken Sie auf dem Blatt **Vorlage bearbeiten** auf **Datei laden**, und laden Sie die Datei **template.json** hoch, die Sie in der vorherigen Aufgabe heruntergeladen haben.

1. Entfernen Sie im Editorbereich die folgenden Zeilen:

   ```json
   "sourceResourceId": {
       "type": "String"
   },
   "sourceUri": {
       "type": "String"
   },
   "osType": {
       "type": "String"
   },
   ```

   ```json
   "hyperVGeneration": {
       "defaultValue": "V1",
       "type": "String"
   },      
   ```

   ```json
   "osType": "[parameters('osType')]",
   ```

    >**Hinweis**: Diese Parameter werden entfernt, da sie auf die aktuelle Bereitstellung nicht anwendbar sind. Insbesondere die Parameter „sourceResourceId“, „sourceUri“, „osType“ und „hyperVGeneration“ gelten für die Erstellung eines Azure-Datenträgers aus einer vorhandenen VHD-Datei.

1. **Speichern** Sie die Änderungen.

1. Klicken Sie zurück auf dem Blatt **Benutzerdefinierte Bereitstellung** auf **Parameter bearbeiten**. 

1. Klicken Sie auf dem Blatt **Parameter bearbeiten** auf **Datei laden**, und laden Sie die Datei **parameters.json** hoch, die Sie in der vorherigen Aufgabe heruntergeladen haben. **Speichern** Sie anschließend die Änderungen.

1. Geben Sie auf dem Blatt **Benutzerdefinierte Bereitstellung** die folgenden Einstellungen an:

    | Einstellung | Wert |
    | --- |--- |
    | Subscription | *Der Name des in diesem Lab verwendeten Azure-Abonnements* |
    | Ressourcengruppe | Der Name einer **neuen** Ressourcengruppe **az104-03b-rg1** |
    | Region | Der Name einer beliebigen Azure-Region, die in dem für dieses Lab verwendeten Abonnement verfügbar ist |
    | Datenträgername | **az104-03b-disk1** |
    | Location | Der Wert des Parameters „location“, den Sie sich in der vorherigen Aufgabe notiert haben |
    | Sku | **Standard_LRS** |
    | Datenträgergröße (GB) | **32** |
    | Erstellungsoption | **empty** |
    | Typ des Datenträgerverschlüsselungssatzes | **EncryptionAtRestWithPlatformKey** |
    | Netzwerkzugriffsrichtlinie | **AllowAll** |

1. Wählen Sie **Überprüfen + erstellen** und anschließend **Erstellen** aus.

1. Überprüfen Sie, ob die Bereitstellung erfolgreich abgeschlossen wurde.

#### <a name="task-3-review-the-arm-template-based-deployment-of-the-managed-disk"></a>Aufgabe 3: Überprüfen der auf einer ARM-Vorlage basierenden Bereitstellung eines verwalteten Azure-Datenträgers

1. Suchen Sie im Azure-Portal nach **Ressourcengruppen**, und wählen Sie die entsprechende Option aus. 

1. Klicken Sie in der Liste der Ressourcengruppen auf **az104-03b-rg1**.

1. Klicken Sie auf dem Blatt der Ressourcengruppe **az104-03b-rg1** im Abschnitt **Einstellungen** auf **Bereitstellungen**.

1. Klicken Sie auf dem Blatt **az104-03b-rg1 – Bereitstellungen** auf den ersten Eintrag in der Liste der Bereitstellungen, und überprüfen Sie den Inhalt der Blätter **Eingabe** und **Vorlage**.

#### <a name="clean-up-resources"></a>Bereinigen von Ressourcen

   >**Hinweis**: Löschen Sie keine Ressourcen, die Sie in diesem Lab bereitgestellt haben. Sie werden im nächsten Lab dieses Moduls darauf verweisen.

#### <a name="review"></a>Überprüfung

In diesem Lab haben Sie die folgenden Aufgaben ausgeführt:

- Überprüfen einer ARM-Vorlage für die Bereitstellung eines verwalteten Azure-Datenträgers
- Erstellen eines verwalteten Azure-Datenträgers mithilfe einer ARM-Vorlage
- Überprüfen der auf einer ARM-Vorlage basierenden Bereitstellung eines verwalteten Azure-Datenträgers
