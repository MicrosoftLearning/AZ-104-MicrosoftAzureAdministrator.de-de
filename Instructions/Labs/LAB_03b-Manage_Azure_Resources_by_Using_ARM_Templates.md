---
lab:
  title: 03b – Verwalten von Azure-Ressourcen mithilfe von ARM-Vorlagen
  module: Module 03 - Azure Administration
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

1. Melden Sie sich am [**Azure-Portal**](http://portal.azure.com) an.

1. Suchen Sie im Azure-Portal nach **Ressourcengruppen**, und wählen Sie die entsprechende Option aus. 

1. Klicken Sie in der Liste der Ressourcengruppen auf **az104-03a-rg1**.

1. Klicken Sie auf dem Blatt der Ressourcengruppe **az104-03a-rg1** im Abschnitt **Einstellungen** auf **Bereitstellungen**.

1. Klicken Sie auf dem Blatt **az104-03a-rg1 – Bereitstellungen** auf den ersten Eintrag in der Liste der Bereitstellungen.

1. Klicken Sie auf dem Blatt **Microsoft.ManagedDisk-* XXXXXXXXX* \| Übersicht** auf **Vorlagen**.

    >**Hinweis**: Überprüfen Sie den Inhalt der Vorlage, und beachten Sie das Vorhandensein der Optionen **Herunterladen** zum Download auf den lokalen Computer, **Zu Bibliothek hinzuzufügen** oder **Bereitstellen** für eine erneute Bereitstellung.

1. Klicken Sie auf **Herunterladen**, und speichern Sie die komprimierte Datei mit den Vorlagen- und Parameterdateien im Ordner **Downloads** auf Ihrem Lab-Computer.

1. Klicken Sie auf dem Blatt **Microsoft.ManagedDisk-* XXXXXXXXX* \| Vorlage** auf **Eingaben**.

1. Note the value of the <bpt id="p1">**</bpt>location<ept id="p1">**</ept> parameter. You will need it in the next task.

1. Extrahieren Sie den Inhalt der heruntergeladenen Datei in den Ordner **Downloads** auf Ihrem Lab-Computer.

    >**Hinweis**: Diese Dateien stehen auch als **\\Allfiles\\Labs\\03\\az104-03b-md-template.json** und **\\Allfiles\\Labs\\03\\az104-03b-md-parameters.json** zur Verfügung.
    
1. Schließen Sie alle **Datei-Explorer**-Fenster.

#### <a name="task-2-create-an-azure-managed-disk-by-using-an-arm-template"></a>Aufgabe 2: Erstellen eines verwalteten Azure-Datenträgers mithilfe einer ARM-Vorlage

1. Suchen Sie im Azure-Portal nach **Benutzerdefinierte Vorlage bereitstellen**, und wählen Sie sie aus.

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

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: These parameters are removed since they are not applicable to the current deployment. In particular, sourceResourceId, sourceUri, osType, and hyperVGeneration parameters are applicable to creating an Azure disk from an existing VHD file.

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

   ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Do not delete resources you deployed in this lab. You will reference them in the next lab of this module.

#### <a name="review"></a>Überprüfung

In diesem Lab haben Sie die folgenden Aufgaben ausgeführt:

- Überprüfen einer ARM-Vorlage für die Bereitstellung eines verwalteten Azure-Datenträgers
- Erstellen eines verwalteten Azure-Datenträgers mithilfe einer ARM-Vorlage
- Überprüfen der auf einer ARM-Vorlage basierenden Bereitstellung eines verwalteten Azure-Datenträgers
