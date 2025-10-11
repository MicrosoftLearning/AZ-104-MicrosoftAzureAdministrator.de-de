---
lab:
  title: 'Lab 09b: Implementieren von Azure Container Instances'
  module: Administer PaaS Compute Options
---

# Lab 09b – Implementieren von Azure Container Instances

## Einführung

In diesem Lab erfahren Sie, wie Sie Azure Container Instances implementieren und bereitstellen.

Für dieses Lab wird ein Azure-Abonnement benötigt. Ihr Abonnementtyp kann sich auf die Verfügbarkeit von Features in diesem Lab auswirken. Die Region kann geändert werden. In den Schritten wird allerdings die Region **USA, Osten** verwendet.

## Geschätzte Zeit: 15 Minuten

## Labszenario

Ihre Organisation verfügt über eine Webanwendung, die auf einem virtuellen Computer in Ihrem lokalen Rechenzentrum ausgeführt wird. Die Organisation möchte alle Anwendungen in die Cloud verlagern, aber keine große Anzahl von Servern verwalten. Sie entscheiden sich dafür, Azure Container Instances und Docker zu testen. 

## Architekturdiagramm

![Diagramm der Aufgaben](../media/az104-lab09b-aci-architecture.png)

## Stellenqualifikationen

- Aufgabe 1: Bereitstellen einer Azure-Containerinstanz unter Verwendung eines Docker-Images
- Aufgabe 2: Testen und Überprüfen der Bereitstellung einer Azure-Containerinstanz

## Aufgabe 1: Bereitstellen einer Azure Container Instances-Instanz mithilfe eines Docker-Images

In dieser Aufgabe wird eine einfache Webanwendung unter Verwendung eines Docker-Images erstellt. Docker ist eine Plattform, mit der Sie Anwendungen in isolierten Umgebungen (sogenannten Containern) verpacken und ausführen können. Azure Container Instances stellt die Compute-Umgebung für das Containerimage bereit.

1. Melden Sie sich beim **Azure-Portal** - `https://portal.azure.com` an.

1. Suchen Sie im Azure-Portal nach `Container instances`, wählen Sie die entsprechende Option aus, und klicken Sie anschließend auf dem Blatt **Containerinstanzen** auf **+ Erstellen**.

1. Geben Sie auf der Registerkarte **Grundeinstellungen** des Blatts **Containerinstanz erstellen** die folgenden Einstellungen an (und übernehmen Sie die Standardwerte für die übrigen Einstellungen):

    | Einstellung | Wert |
    | ---- | ---- |
    | Subscription | Auswählen des Azure-Abonnements |
    | Resource group | `az104-rg9` (Wählen Sie bei Bedarf **Neu erstellen** aus.) |
    | Containername | `az104-c1` |
    | Region | **USA, Osten** (oder eine in Ihrer Nähe verfügbare Region)|
    | Imagequelle | **Schnellstartimages** |
    | Image | **mcr.microsoft.com/azuredocs/aci-helloworld:latest (Linux)** |

1. Klicken Sie auf **Next: Netzwerk >**, geben Sie die folgenden Einstellungen an, und übernehmen Sie ansonsten die Standardwerte:

    | Einstellung | Wert |
    | --- | --- |
    | DNS-Namensbezeichnung | Beliebiger gültiger, global eindeutiger DNS-Hostname |

    >**Hinweis**: Ihr Container ist unter „dns-name-label.region.azurecontainer.io“ öffentlich erreichbar. Falls die Fehlermeldung **DNS-Namensbezeichnung ist nicht verfügbar** angezeigt wird, geben Sie einen anderen Wert an.

1. Klicken Sie auf **Weiter: Überwachung >** und entfernen Sie die Markierung bei **Containerinstanzprotokolle aktivieren**. 

1. Klicken Sie auf **Next: Erweitert >**, und überprüfen Sie die Einstellungen, ohne Änderungen vorzunehmen.

1. Klicken Sie auf **Überprüfen + erstellen**, vergewissern Sie sich, dass die Überprüfung erfolgreich war, und wählen Sie anschließend **Erstellen** aus.

    >**Hinweis**: Warten Sie, bis die Bereitstellung abgeschlossen ist. Dieser Vorgang dauert zwei bis drei Minuten.

    >**Hinweis**: Während Sie warten, möchten Sie sich vielleicht den [Code hinter der Beispielanwendung](https://github.com/Azure-Samples/aci-helloworld) ansehen. Durchsuchen Sie zum Anzeigen des Codes den Ordner „\\app“.

## Aufgabe 2: Testen und Überprüfen der Bereitstellung einer Azure-Container-Instanz 

In dieser Aufgabe wird die Bereitstellung der Containerinstanz überprüft. Auf die Azure-Containerinstanz kann standardmäßig über den Port 80 zugegriffen werden. Nachdem die Instanz bereitgestellt wurde, können Sie unter Verwendung des DNS-Namens, den Sie in der vorherigen Aufgabe angegeben haben, zu dem Container navigieren.

1. Wenn die Bereitstellung abgeschlossen ist, wählen Sie den Link **Zur Ressource gehen**.

1. Stellen Sie auf dem Blatt **Übersicht** der Containerinstanz sicher, dass der **Status** als **Wird ausgeführt** gemeldet wird.

1. Kopieren Sie den Wert **FQDN** der Containerinstanz, öffnen Sie eine neue Browserregisterkarte, und wechseln Sie zu der entsprechenden URL.

     ![Screenshot: ACI-Übersichtsseite im Portal](../media/az104-lab09b-aci-overview.png)

1. Stellen Sie sicher, dass die Seite **Willkommen bei Azure Container Instances** angezeigt wird. Aktualisieren Sie die Seite mehrmals, um ein paar Protokolleinträge zu erstellen, und schließen Sie dann den Browsertab.  

1. Klicken Sie im Azure-Portal auf dem Blatt der Containerinstanz im Abschnitt **Einstellungen** auf **Container** und anschließend auf **Protokolle**.

1. Stellen Sie sicher, dass die Protokolleinträge für die generierte HTTP GET-Anforderung angezeigt werden, indem Sie die Anwendung im Browser anzeigen.
   
## Bereinigen Ihrer Ressourcen

Wenn Sie mit **Ihrem eigenen Abonnement** arbeiten, nehmen Sie sich eine Minute Zeit, um die Labressourcen zu löschen. Dadurch wird sichergestellt, dass Ressourcen freigegeben und Kosten minimiert werden. Die einfachste Möglichkeit zum Löschen der Labressourcen besteht darin, die Ressourcengruppe des Labs zu löschen. 

+ Wählen Sie im Azure-Portal die Ressourcengruppe und dann **Ressourcengruppe löschen** aus, **geben Sie den Ressourcengruppennamen ein**, und klicken Sie dann auf **Löschen**.
+ Bei Verwendung von Azure PowerShell: `Remove-AzResourceGroup -Name resourceGroupName`.
+ Bei Verwendung der Befehlszeilenschnittstelle: `az group delete --name resourceGroupName`.

## Erweitern Ihrer Lernerfahrung mit Copilot
Copilot kann Sie beim Erlernen der Verwendung von Azure-Skripttools unterstützen. Copilot kann Sie auch in Bereichen unterstützen, die nicht im Lab behandelt werden oder in denen Sie weitere Informationen benötigen. Öffnen Sie einen Edge-Browser, und wählen Sie „Copilot“ (rechts oben) aus, oder navigieren Sie zu *copilot.microsoft.com*. Nehmen Sie sich einige Minuten Zeit, um diese Prompts auszuprobieren.

+ Fasse die Schritte zum Erstellen und Konfigurieren einer Azure-Containerinstanz zusammen.
+ Wie kann ich einen serverlosen Container in Azure ausführen?

## Weiterlernen im eigenen Tempo

+ [Ausführen von Containerimages in Azure Container Instances](https://learn.microsoft.com/training/modules/create-run-container-images-azure-container-instances/) Erfahren Sie, wie Azure Container Instances Sie beim schnellen Bereitstellen von Containern unterstützen kann, wie Sie Umgebungsvariablen festlegen und wie Sie Neustartrichtlinien für Container angeben.

## Wichtige Erkenntnisse

Herzlichen Glückwunsch zum erfolgreichen Abschluss des Labs. Hier sind die wichtigsten Erkenntnisse für dieses Lab. 

+ Azure Container Instances (ACI) ist ein Dienst, mit dem Sie Container in der öffentlichen Microsoft Azure-Cloud bereitstellen können.
+ Für ACI muss keine zugrunde liegende Infrastruktur bereitgestellt oder verwaltet werden.
+ ACI unterstützt sowohl Linux- als auch Windows-Container.
+ Workloads in ACI werden üblicherweise durch eine Art von Prozess oder Trigger gestartet und beendet und sind in der Regel kurzlebig. 

    
