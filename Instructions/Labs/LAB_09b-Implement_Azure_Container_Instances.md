---
lab:
  title: 09b – Implementieren von Azure Container Instances
  module: Module 09 - Serverless Computing
ms.openlocfilehash: 2783bb606e0536ec7ef53d4966d07064392c6421
ms.sourcegitcommit: 8a0ced6338608682366fb357c69321ba1aee4ab8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2021
ms.locfileid: "132625488"
---
# <a name="lab-09b---implement-azure-container-instances"></a>Lab 09b – Implementieren von Azure Container Instances
# <a name="student-lab-manual"></a>Lab-Handbuch für Kursteilnehmer

## <a name="lab-scenario"></a>Labszenario

Contoso möchte eine neue Plattform für virtualisierte Workloads finden. Sie haben eine Reihe von Containerimages identifiziert, die genutzt werden können, um dieses Ziel zu erreichen. Da Sie die Containerverwaltung auf ein Mindestmaß begrenzen möchten, möchten Sie die Verwendung von Azure Container Instances zur Bereitstellung von Docker-Images auswerten.

## <a name="objectives"></a>Ziele

Dieses Lab deckt Folgendes ab:

- Aufgabe 1: Bereitstellen eines Docker-Images mithilfe von Azure Container Instances
- Aufgabe 2: Überprüfen der Funktionalität der Azure Container Instances-Instanz

## <a name="estimated-timing-20-minutes"></a>Geschätzte Zeit: 20 Minuten

## <a name="architecture-diagram"></a>Architekturdiagramm

![image](../media/lab09b.png)

## <a name="instructions"></a>Anweisungen

### <a name="exercise-1"></a>Übung 1

#### <a name="task-1-deploy-a-docker-image-by-using-the-azure-container-instance"></a>Aufgabe 1: Bereitstellen eines Docker-Images mithilfe von Azure Container Instances

In dieser Aufgabe erstellen Sie eine neue Containerinstanz für die Webanwendung.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Suchen Sie im Azure-Portal nach **Containerinstanzen**, und klicken Sie dann auf dem Blatt **Containerinstanzen** auf **+ Erstellen**.

1. Geben Sie auf der Registerkarte **Grundeinstellungen** des Blatts **Containerinstanz erstellen** die folgenden Einstellungen an (und übernehmen Sie die Standardwerte für die übrigen Einstellungen):

    | Einstellung | Wert |
    | ---- | ---- |
    | Subscription | Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden |
    | Resource group | Der Name einer neuen Ressourcengruppe **az104-09b-rg1** |
    | Containername | **az104-9b-c1** |
    | Region | Der Name einer Region, in der Sie Azure-Containerinstanzen bereitstellen können |
    | Imagequelle | **Schnellstartimages** |
    | Image | **mcr.microsoft.com/azuredocs/aci-helloworld:latest (Linux)** |

1. Klicken Sie auf **Weiter: Netzwerk >** , und geben Sie auf dem Blatt **Containerinstanz erstellen** auf der Registerkarte **Netzwerk** die folgenden Einstellungen an (und übernehmen Sie die Standardwerte für die übrigen Einstellungen):

    | Einstellung | Wert |
    | --- | --- |
    | DNS-Namensbezeichnung | Beliebiger gültiger, global eindeutiger DNS-Hostname |

    >**Hinweis**: Ihr Container ist unter „dns-name-label.region.azurecontainer.io“ öffentlich erreichbar. Falls die Fehlermeldung **DNS-Namensbezeichnung ist nicht verfügbar** angezeigt wird, geben Sie einen anderen Wert an.

1. Klicken Sie auf **Weiter: Erweitert >** , und prüfen Sie auf dem Blatt **Containerinstanz erstellen** die Einstellungen auf der Registerkarte **Erweitert**, ohne Änderungen vorzunehmen. Klicken Sie auf **Überprüfen + erstellen**, stellen Sie sicher, dass die Überprüfung erfolgreich ist, und klicken Sie dann auf **Erstellen**.

    >**Hinweis**: Warten Sie, bis die Bereitstellung abgeschlossen ist. Dieser Vorgang dauert etwa drei Minuten.

    >**Hinweis**: Während Sie warten, möchten Sie sich vielleicht den [Code hinter der Beispielanwendung](https://github.com/Azure-Samples/aci-helloworld) ansehen. Durchsuchen Sie dazu den Ordner „\\app“.

#### <a name="task-2-review-the-functionality-of-the-azure-container-instance"></a>Aufgabe 2: Überprüfen der Funktionalität der Azure Container Instances-Instanz

In dieser Aufgabe überprüfen Sie die Bereitstellung der Containerinstanz.

1. Klicken Sie auf dem Blatt „Bereitstellung“ auf **Zu Ressource wechseln**.

1. Stellen Sie auf dem Blatt **Übersicht** der Containerinstanz sicher, dass der **Status** als **Wird ausgeführt** gemeldet wird.

1. Kopieren Sie den Wert **FQDN** der Containerinstanz, öffnen Sie eine neue Browserregisterkarte, und wechseln Sie zu der entsprechenden URL.

1. Stellen Sie sicher, dass die Seite **Willkommen bei Azure Container Instances** angezeigt wird.

1. Schließen Sie die neue Browserregisterkarte. Klicken Sie im Azure-Portal auf dem Blatt der Containerinstanz im Abschnitt **Einstellungen** auf **Container**, und klicken Sie dann auf **Protokolle**.

1. Stellen Sie sicher, dass die Protokolleinträge für die generierte HTTP GET-Anforderung angezeigt werden, indem Sie die Anwendung im Browser anzeigen.

#### <a name="clean-up-resources"></a>Bereinigen von Ressourcen

   >**Hinweis**: Denken Sie daran, alle neu erstellten Azure-Ressourcen zu entfernen, die Sie nicht mehr verwenden. Durch das Entfernen nicht verwendeter Ressourcen wird sichergestellt, dass keine unerwarteten Kosten anfallen.

1. Öffnen Sie im Azure-Portal im Bereich **Cloud Shell** die **PowerShell**-Sitzung.

1. Listen Sie alle Ressourcengruppen auf, die während der Labs in diesem Modul erstellt wurden, indem Sie den folgenden Befehl ausführen:

   ```powershell
   Get-AzResourceGroup -Name 'az104-09b*'
   ```

1. Löschen Sie alle Ressourcengruppen, die Sie während der praktischen Übungen in diesem Modul erstellt haben, indem Sie den folgenden Befehl ausführen:

   ```powershell
   Get-AzResourceGroup -Name 'az104-09b*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Hinweis**: Der Befehl wird (wie über den Parameter „-AsJob“ festgelegt) asynchron ausgeführt. Dies bedeutet, dass Sie zwar direkt im Anschluss einen weiteren PowerShell-Befehl in derselben PowerShell-Sitzung ausführen können, es jedoch einige Minuten dauert, bis die Ressourcengruppen tatsächlich entfernt werden.

#### <a name="review"></a>Überprüfung

In diesem Lab haben Sie die folgenden Aufgaben ausgeführt:

- Bereitstellen eines Docker-Images mithilfe von Azure Container Instances
- Überprüfen der Funktionalität der Azure Container Instances-Instanz
