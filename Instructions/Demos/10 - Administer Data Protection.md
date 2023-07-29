---
demo:
  title: "Demo\_10: Verwalten des Schutzes von Daten"
  module: Administer Data Protection
---

# 10: Verwalten des Schutzes von Daten

## Sichern von Azure-Dateifreigaben

In dieser Demo sichern wir eine Dateifreigabe im Azure-Portal.

> **Hinweis:** Für diese Demonstration ist ein Speicherkonto mit einer Dateifreigabe erforderlich. 

**Referenz:** [Sichern von Azure-Dateifreigaben im Azure-Portal](https://docs.microsoft.com/azure/backup/backup-afs)

**Erstellen eines Recovery Services-Tresors**

1. Verwenden Sie dafür das Azure-Portal.

1. Suchen Sie nach **Recovery Services-Tresore**, und wählen Sie die Option aus.

1. Erstellen Sie einen **Recovery Services-Tresor**. Betonen Sie noch einmal, dass sich der Tresor in derselben Region wie die Dateifreigabe befinden muss. 

1. Warten Sie, bis der Tresor erstellt wurde. 

**Konfigurieren der Azure-Dateisicherung**

1. Wechseln Sie zu **Backup Center**, und erstellen Sie eine neue **Sicherungsinstanz**.

1. Wiederholen und besprechen Sie die Optionen in der Dropdownliste **Datenquellentyp**. Wählen Sie **Azure Files (Azure Storage)** aus. 

1. Wählen Sie Ihren **Tresor** aus.

1. Fahren Sie mit der **Konfiguration der Sicherung** fort. Wählen Sie das Speicherkonto und die Dateifreigabe aus, die Sie sichern möchten.  

1. Klicken Sie in den **Richtliniendetails** auf **Diese Richtlinie bearbeiten**. Besprechen Sie den Zweck der Sicherungsrichtlinien. Gehen Sie auf den **Sicherungszeitplan** und die **Beibehaltungsdauer** ein.  

1. **Aktivieren Sie die Sicherung**, um Ihre Änderungen zu speichern. 

1. Wenn Sie Zeit haben, besprechen Sie, wie eine **Sicherungsinstanz** **wiederhergestellt** wird. Erklären Sie außerdem, wie **Sicherungsaufträge** überwacht werden. 

## Sichern von virtuellen Azure-Computern

In dieser Demo planen wir eine tägliche Sicherung eines virtuellen Computers in einem Recovery Services-Tresor.

> **Hinweis:**  Für diese Demo sind ein virtueller Computer und ein Recovery Services-Tresor erforderlich.

**Referenz:** [Tutorial: Sichern von mehreren virtuellen Azure-Computern](https://docs.microsoft.com/azure/backup/tutorial-backup-vm-at-scale)

1. Verwenden Sie dafür das Azure-Portal.

1. Wechseln Sie zu **Backup Center**, und erstellen Sie eine neue **Sicherungsinstanz**.

1. Wählen Sie **Virtuelle Azure-Computer** als **Datenquellentyp** und dann den Tresor aus.

1. Gehen Sie auf **DefaultPolicy** ein. Die Standardrichtlinie sichert den virtuellen Computer einmal täglich. Die täglichen Sicherungen werden 30 Tage lang aufbewahrt. Momentaufnahmen für die sofortige Wiederherstellung werden zwei Tage lang aufbewahrt.

1. Verwenden Sie **Sicherung aktivieren**, um Ihre Konfiguration zu speichern.

1. Wenn Sie Zeit haben, besprechen Sie, wie die Option **Jetzt sichern** verwendet wird. Erklären Sie außerdem, wie **Sicherungsaufträge** überprüft werden.  

