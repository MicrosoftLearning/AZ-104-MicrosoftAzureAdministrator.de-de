---
demo:
  title: 'Demo 07: Verwalten von Azure Storage'
  module: Administer Azure Storage
---


# 07: Verwalten von Azure Storage

## Konfigurieren von Speicherkonten

In dieser Demo erstellen wir ein Speicherkonto.

**Referenz:** [Erstellen eines Speicherkontos](https://docs.microsoft.com/azure/storage/common/storage-account-create?tabs=azure-portal)

1. Verwenden Sie dafür das Azure-Portal.

1. Überprüfen Sie den Zweck von Speicherkonten. 
   
1. Suchen Sie nach der Option **Speicherkonten**, und wählen Sie sie aus. 
 
1. Erstellen Sie ein einfaches Speicherkonto. 

    - Besprechen Sie die Anforderungen für die Benennung eines Speicherkontos und die Notwendigkeit, dass der Name in Azure eindeutig sein muss. 

    - Sehen Sie sich die verschiedenen Speichertypen an. Ein Beispiel ist der Typ „Universell v2“. 

    - Überprüfen Sie die verschiedenen Optionen für die Zugriffsebenenauswahl. Beispielsweise die kalte und die heiße Ebene. 

    - Andere Registerkarten und Einstellungen werden in anderen Demos behandelt. 

1. Erstellen Sie das Speicherkonto, und warten Sie, bis die Ressource bereitgestellt wird. 


## Konfigurieren von Blob Storage

In dieser Demo sehen wir uns Blob Storage genauer an.

**Hinweis:**  Für diese Schritte ist ein Speicherkonto erforderlich.

**Referenz:** [Schnellstart: Hochladen, Herunterladen und Auflisten von Blobs mit .NET](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)

1. Navigieren Sie im Azure-Portal zu einem Speicherkonto.

1. Überprüfen Sie den Zweck des Blobspeichers. 

1. Erstellen Sie einen Blobcontainer. Überprüfen Sie die Zugriffsebene für den Container. Wählen Sie „Privat (kein anonymer Zugriff)“ aus. 

1. Laden Sie nun ein Blob in den Container. Wenn Sie Zeit haben, überprüfen Sie die erweiterten Einstellungen. Beispiel: Blobtyp und Blobgröße. 

## Konfigurieren der Speichersicherheit

In dieser Demo erstellen wir eine Shared Access Signature.

**Hinweis:**  Diese Demo erfordert ein Speicherkonto mit einem Blobcontainer und eine hochgeladene Datei.

**Referenz:** [Erstellen von SAS-Tokens für Speichercontainer](https://learn.microsoft.com/azure/applied-ai-services/form-recognizer/create-sas-tokens?source=recommendations&view=form-recog-3.0.0)

1. Wählen Sie ein Blob oder eine Datei aus, die Sie sichern möchten. 

1. Sie generieren eine Shared Access Signature (SAS). Überprüfen Sie die Berechtigungen, Start- und Ablaufzeiten sowie zulässige Protokolle.

1. Verwenden Sie die SAS-URL, um sicherzustellen, dass die Ressource angezeigt wird. 


## Konfigurieren von Azure Files 

In dieser Demo verwenden wir Dateifreigaben und Momentaufnahmen.

**Hinweis:**  Für diese Schritte ist ein Speicherkonto erforderlich.

**Referenz:** [Schnellstart: Verwalten von Azure-Dateifreigaben](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-portal?tabs=azure-portal)

1. Überprüfen Sie den Zweck von Dateifreigaben. 

1. Greifen Sie auf ein Speicherkonto zu, und klicken Sie auf **Dateien**.

1. Erstellen einer Dateifreigabe Überprüfen Sie Kontingente, laden Sie Dateien hoch, und fügen Sie Verzeichnisse hinzu, um die Informationen zu organisieren. 

1. Erstellen Sie einen Momentaufnahme einer Dateifreigabe. Überprüfen Sie, wann Momentaufnahmen verwendet werden sollten und wie sie sich von Sicherungen unterscheiden. Laden Sie eine Datei hoch, erstellen Sie eine Momentaufnahme, löschen Sie die Datei, und stellen Sie die Momentaufnahme wieder her.


## Speichertools (optional)

In dieser Demo sehen wir uns verschiedene gängige Azure Storage-Tools an. 

**Referenz:** [Erste Schritte mit dem Storage-Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows)

1. Installieren Sie den Storage-Explorer, oder verwenden Sie den Speicherbrowser.

1. Überprüfen Sie, wie Sie Speicherressourcen durchsuchen und erstellen. Fügen Sie beispielsweise einen Blobcontainer hinzu. 

**Referenz:** [Kopieren oder Verschieben von Daten in Azure Storage mithilfe von AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=/azure/storage/files/toc.json)

1. Besprechen Sie, wann AzCopy verwendet werden sollte. Sehen Sie sich die Hilfe **azcopy /?** an.

1. Scrollen Sie im Abschnitt **Beispiele** nach unten. Wenn Sie Zeit haben, probieren Sie eines der Beispiele aus. 
    



