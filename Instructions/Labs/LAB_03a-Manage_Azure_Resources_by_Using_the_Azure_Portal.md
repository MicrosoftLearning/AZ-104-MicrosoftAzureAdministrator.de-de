---
lab:
  title: 03a – Verwalten von Azure-Ressourcen über das Azure-Portal
  module: Module 03 - Azure Administration
ms.openlocfilehash: 38ca9fa2ec16f786824e7ba5b27bb194f31f7d7b
ms.sourcegitcommit: 8282cbcee5f7cd46bdc73d781c460d6a078049bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2022
ms.locfileid: "143611539"
---
# <a name="lab-03a---manage-azure-resources-by-using-the-azure-portal"></a>Übung 03a – Verwalten von Azure-Ressourcen über das Azure-Portal
# <a name="student-lab-manual"></a>Lab-Handbuch für Kursteilnehmer

## <a name="lab-scenario"></a>Labszenario

Sie möchten die grundlegenden Azure-Verwaltungsfunktionen erkunden, die mit der Bereitstellung von Ressourcen und der Strukturierung von Ressourcen in Ressourcengruppen verbunden sind, einschließlich des Verschiebens von Ressourcen zwischen Ressourcengruppen. Außerdem möchten Sie Optionen zum Schutz von Datenträgerressourcen vor versehentlichem Löschen untersuchen, aber gleichzeitig die Änderung der zugehörigen Leistungsmerkmale und Größen zulassen.

## <a name="objectives"></a>Ziele

In diesem Lab werden folgende Aufgaben ausgeführt:

+ Aufgabe 1: Erstellen von Ressourcengruppen und Bereitstellen von Ressourcen in Ressourcengruppen
+ Aufgabe 2: Verschieben von Ressourcen zwischen Ressourcengruppen
+ Aufgabe 3: Implementieren und Testen von Ressourcensperren

## <a name="estimated-timing-20-minutes"></a>Geschätzte Zeit: 20 Minuten

## <a name="architecture-diagram"></a>Architekturdiagramm

![image](../media/lab03a.png)

## <a name="instructions"></a>Anweisungen

### <a name="exercise-1"></a>Übung 1

#### <a name="task-1-create-resource-groups-and-deploy-resources-to-resource-groups"></a>Aufgabe 1: Erstellen von Ressourcengruppen und Bereitstellen von Ressourcen in Ressourcengruppen

In dieser Aufgabe verwenden Sie das Azure-Portal, um Ressourcengruppen und einen Datenträger in der Ressourcengruppe zu erstellen.

1. Melden Sie sich am [**Azure-Portal**](http://portal.azure.com) an.

1. Suchen Sie im Azure-Portal nach **Datenträger**, wählen Sie diese Option aus. Klicken Sie auf **+ Erstellen**, und geben Sie die folgenden Einstellungen an:

    |Einstellung|Wert|
    |---|---|
    |Subscription| Der Name des Azure-Abonnements, in dem Sie die Ressourcengruppe erstellt haben |
    |Ressourcengruppe| Der Name einer neuen Ressourcengruppe **az104-03a-rg1** |
    |Name des Datenträgers| **az104-03a-disk1** |
    |Region| Der Name der Azure-Region, in der Sie die Ressourcengruppe erstellt haben |
    |Verfügbarkeitszone| **None** |
    |Quellentyp| **None** |

    >**Hinweis**: Beim Erstellen einer Ressource haben Sie die Möglichkeit, eine neue Ressourcengruppe zu erstellen oder eine vorhandene zu verwenden.

1. Ändern Sie den Datenträgertyp und die Größe in **HDD Standard** bzw. **32 GiB**.

1. Klicken Sie auf **Überprüfen + erstellen** und dann auf **Erstellen**.

    >**Hinweis**: Warten Sie, bis der Datenträger erstellt wurde. Das sollte weniger als eine Minute dauern.

#### <a name="task-2-move-resources-between-resource-groups"></a>Aufgabe 2: Verschieben von Ressourcen zwischen Ressourcengruppen 

In dieser Aufgabe verschieben wir die Datenträgerressource, die Sie in der vorherigen Aufgabe erstellt haben, in eine neue Ressourcengruppe. 

1. Suchen Sie nach **Ressourcengruppen**, und wählen Sie diese Option aus. 

1. Klicken Sie auf dem Blatt **Ressourcengruppen** auf den Eintrag der Ressourcengruppe **az104-03a-rg1**, die Sie in der vorherigen Aufgabe erstellt haben.

1. Wählen Sie auf dem Blatt **Übersicht** der Ressourcengruppe in der Liste der Ressourcengruppenressourcen den Eintrag aus, der den neu erstellten Datenträger darstellt, klicken Sie auf der Symbolleiste auf **Verschieben**, und wählen Sie in der Dropdownliste die Option **In eine andere Ressourcengruppe verschieben** aus.

    >**Hinweis**: Mit dieser Methode können Sie mehrere Ressourcen gleichzeitig verschieben. 

1. Klicken Sie unterhalb des Textfelds **Ressourcengruppe** auf **Neu erstellen**, und geben Sie dann **az104-03a-rg2** in das Textfeld ein. Aktivieren Sie zur Bestätigung das Kontrollkästchen **Ich nehme zur Kenntnis, dass die verschobenen Ressourcen zugeordneten Tools und Skripts erst wieder funktionieren, wenn sie mit neuen Ressourcen-IDs aktualisiert wurden**, und klicken Sie dann auf **OK**.

    >**Hinweis**: Warten Sie nicht, bis der Verschiebungsvorgang abgeschlossen ist, sondern fahren Sie stattdessen mit der nächsten Aufgabe fort. Die Verschiebung kann etwa zehn Minuten dauern. Sie können ermitteln, ob der Vorgang abgeschlossen wurde, indem Sie Aktivitätsprotokolleinträge der Quell- oder Zielressourcengruppe überwachen. Kehren Sie zu diesem Schritt zurück, sobald Sie die nächste Aufgabe abgeschlossen haben.

#### <a name="task-3-implement-resource-locks"></a>Aufgabe 3: Implementieren von Ressourcensperren

In dieser Aufgabe wenden Sie eine Ressourcensperre auf eine Azure-Ressourcengruppe an, die eine Datenträgerressource enthält.

1. Suchen Sie im Azure-Portal nach **Datenträger**, wählen Sie diese Option aus. Klicken Sie auf **+ Erstellen**, und geben Sie die folgenden Einstellungen an:

    |Einstellung|Wert|
    |---|---|
    |Subscription| Name des Abonnements, das Sie für dieses Lab verwenden |
    |Ressourcengruppe| Klicken Sie auf **Neue Ressourcengruppe erstellen**, und nennen Sie sie **az104-03a-rg3**. |
    |Name des Datenträgers| **az104-03a-disk2** |
    |Region| Der Name der Azure-Region, in der Sie die anderen Ressourcengruppen in diesem Lab erstellt haben |
    |Verfügbarkeitszone| **None** |
    |Quellentyp| **None** |

1. Legen Sie den Datenträgertyp und die Größe auf **HDD Standard** bzw. **32 GiB** fest.

1. Klicken Sie auf **Überprüfen + erstellen** und dann auf **Erstellen**.

1. Klicken Sie auf **Zu Ressource wechseln**.

1. Klicken Sie auf der Seite „Übersicht“ des Datenträgers auf den Namen der Ressourcengruppe, **az104-03a-rg3**.

1. Klicken Sie auf dem Blatt der Ressourcengruppe **az104-03a-rg3** auf **Sperren** und dann auf **+ Hinzufügen**, und geben Sie die folgenden Einstellungen an:

    |Einstellung|Wert|
    |---|---|
    |Sperrenname| **az104-03a-delete-lock** |
    |Sperrtyp| **Löschen** |
    
1. Klicken Sie auf **OK**    

1. Klicken Sie auf dem Blatt der Ressourcengruppe **az104-03a-rg3** auf **Übersicht**, wählen Sie in der Liste der Ressourcengruppenressourcen den Eintrag aus, der den zuvor in dieser Aufgabe erstellten Datenträger darstellt, und klicken Sie auf der Symbolleiste auf **Löschen.** 

1. Wenn die Meldung **Möchten Sie alle ausgewählten Ressourcen löschen?** angezeigt wird, geben Sie im Feld **Löschen bestätigen** das Wort **ja** ein, und klicken Sie auf **Löschen**.

1. Es sollte eine Fehlermeldung angezeigt werden, in der Sie über den fehlerhaften Löschvorgang benachrichtigt werden. 

    >**Hinweis**: Wie die Fehlermeldung besagt, handelt es sich um einen erwarteten Fehler aufgrund der Löschsperre auf Ressourcengruppenebene.

1. Kehren Sie zur Liste der Ressourcen in der Ressourcengruppe **az104-03a-rg3** zurück, und klicken Sie auf den Eintrag der Ressource **az104-03a-disk2**. 

1. Klicken Sie auf dem Blatt **az104-03a-disk2** im Abschnitt **Einstellungen** auf **Größe und Leistung**, legen Sie den Datenträgertyp und die Größe auf **SSD Premium** bzw. **64 GiB** fest, und klicken Sie auf **Größe ändern**, um die Änderung anzuwenden. Stellen Sie sicher, dass die Änderung erfolgreich vorgenommen wurde.

    >**Hinweis**: Dies entspricht den Erwartungen, weil die Sperre auf Ressourcengruppenebene nur für Löschvorgänge gilt. 

#### <a name="clean-up-resources"></a>Bereinigen von Ressourcen

   >**Hinweis**: Löschen Sie keine Ressourcen, die Sie in diesem Lab bereitgestellt haben. Sie werden sie im nächsten Lab dieses Moduls verwenden. Entfernen Sie nur die Ressourcensperre, die Sie in diesem Lab erstellt haben.

1. Navigieren Sie zum Blatt der Ressourcengruppe **az104-03a-rg3**, zeigen Sie das Blatt **Sperren** an, und entfernen Sie die Sperre **az104-03a-delete-lock**, indem Sie auf der rechten Seite des Sperreintrags **Löschen** auf den Link **Löschen** klicken.

#### <a name="review"></a>Überprüfung

In diesem Lab haben Sie die folgenden Aufgaben ausgeführt:

- Erstellen von Ressourcengruppen und Bereitstellen von Ressourcen in Ressourcengruppen
- Verschieben von Ressourcen zwischen Ressourcengruppen
- Implementieren und Testen von Ressourcensperren
