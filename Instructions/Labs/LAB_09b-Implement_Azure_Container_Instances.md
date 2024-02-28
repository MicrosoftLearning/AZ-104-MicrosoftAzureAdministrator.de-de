---
lab:
  title: 'Lab 09b: Implementieren von Azure Container Instances'
  module: Administer PaaS Compute Options
---

# Lab 09b – Implementieren von Azure Container Instances

## Einführung

In diesem Lab erfahren Sie, wie Sie Azure Container Instances implementieren und bereitstellen.

Für dieses Lab wird ein Azure-Abonnement benötigt. Ihr Abonnementtyp kann sich auf die Verfügbarkeit von Features in diesem Lab auswirken. Die Region kann geändert werden. In den Schritten wird allerdings die Region **USA, Osten** verwendet.

## Geschätzte Zeit: 15 Minuten

## Labszenario

Ihre Organisation verfügt über eine Webanwendung, die auf einem virtuellen Computer in Ihrem lokalen Rechenzentrum ausgeführt wird. Die Organisation möchte alle Anwendungen in die Cloud verlagern, aber keine große Anzahl von Servern verwalten. Sie entscheiden sich dafür, Azure Container Instances und Docker zu testen. 
## Interaktive Labsimulation

Für dieses Thema stehen hilfreiche interaktive Labsimulationen zur Verfügung. In der Simulation können Sie sich in Ihrem eigenen Tempo durch ein ähnliches Szenario klicken. Es gibt zwar Unterschiede zwischen der interaktiven Simulation und diesem Lab, viele der zentralen Konzepte sind jedoch identisch. Ein Azure-Abonnement ist nicht erforderlich.

+ [Bereitstellen von Azure Container Instances](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%203): Hier finden Sie Informationen zum Erstellen, Konfigurieren und Bereitstellen eines Docker-Containers mit Azure Container Instances.
  
+ [Implementieren von Azure Container Instances](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2014):  Hier erfahren Sie, wie Sie ein Docker-Image unter Verwendung von Azure Container Instances bereitstellen. 

## Stellenqualifikationen

- Aufgabe 1: Bereitstellen einer Azure-Containerinstanz unter Verwendung eines Docker-Images
- Aufgabe 2: Testen und Überprüfen der Bereitstellung einer Azure-Container-Instanz


## Architekturdiagramm

![Diagramm der Aufgaben](../media/az104-lab09b-aci-architecture.png)

## Aufgabe 1: Bereitstellen einer Azure Container Instances-Instanz mithilfe eines Docker-Images

In dieser Aufgabe wird eine einfache Webanwendung unter Verwendung eines Docker-Images erstellt. Docker ist eine Plattform, mit der Sie Anwendungen in isolierten Umgebungen (sogenannten Containern) verpacken und ausführen können. Azure Container Instances stellt die Compute-Umgebung für das Containerimage bereit.

1. Melden Sie sich beim **Azure-Portal** - `https://portal.azure.com` an.

1. Suchen Sie im Azure-Portal nach `Container instances`, wählen Sie die entsprechende Option aus, und klicken Sie anschließend auf dem Blatt **Containerinstanzen** auf **+ Erstellen**.

1. Geben Sie auf der Registerkarte **Grundeinstellungen** des Blatts **Containerinstanz erstellen** die folgenden Einstellungen an (und übernehmen Sie die Standardwerte für die übrigen Einstellungen):

    | Einstellung | Wert |
    | ---- | ---- |
    | Subscription | Wählen Sie Ihr Azure-Abonnement aus. |
    | Resource group | `az104-rg9` (Wählen Sie bei Bedarf **Neu erstellen** aus.) |
    | Containername | `az104-c1` |
    | Region | **USA, Osten** (oder eine in Ihrer Nähe verfügbare Region)|
    | Imagequelle | **Schnellstartimages** |
    | Image | **mcr.microsoft.com/azuredocs/aci-helloworld:latest (Linux)** |

1. Klicken Sie auf **Next: Netzwerk >**, geben Sie die folgenden Einstellungen an, und übernehmen Sie ansonsten die Standardwerte:

    | Einstellung | Wert |
    | --- | --- |
    | DNS-Namensbezeichnung | Beliebiger gültiger, global eindeutiger DNS-Hostname |

    >**Hinweis**: Ihr Container ist unter „dns-name-label.region.azurecontainer.io“ öffentlich erreichbar. Falls die Fehlermeldung **DNS-Namensbezeichnung ist nicht verfügbar** angezeigt wird, geben Sie einen anderen Wert an.

1. Klicken Sie auf **Next: Erweitert >**, und überprüfen Sie die Einstellungen, ohne Änderungen vorzunehmen.

 1. Klicken Sie auf **Überprüfen + erstellen**, vergewissern Sie sich, dass die Überprüfung erfolgreich war, und wählen Sie anschließend **Erstellen** aus.

    >**Hinweis**: Warten Sie, bis die Bereitstellung abgeschlossen ist. Dieser Vorgang dauert zwei bis drei Minuten.

    >**Hinweis**: Während Sie warten, möchten Sie sich vielleicht den [Code hinter der Beispielanwendung](https://github.com/Azure-Samples/aci-helloworld) ansehen. Durchsuchen Sie zum Anzeigen des Codes den Ordner „\\app“.

## Aufgabe 2: Testen und Überprüfen der Bereitstellung einer Azure-Container-Instanz 

In dieser Aufgabe wird die Bereitstellung der Containerinstanz überprüft. Auf die Azure-Containerinstanz kann standardmäßig über den Port 80 zugegriffen werden. Nachdem die Instanz bereitgestellt wurde, können Sie unter Verwendung des DNS-Namens, den Sie in der vorherigen Aufgabe angegeben haben, zu dem Container navigieren.

1. Klicken Sie auf dem Blatt „Bereitstellung“ auf **Zu Ressource wechseln**.

1. Stellen Sie auf dem Blatt **Übersicht** der Containerinstanz sicher, dass der **Status** als **Wird ausgeführt** gemeldet wird.

1. Kopieren Sie den Wert **FQDN** der Containerinstanz, öffnen Sie eine neue Browserregisterkarte, und wechseln Sie zu der entsprechenden URL.

     ![Screenshot: ACI-Übersichtsseite im Portal](../media/az104-lab09b-aci-overview.png)

1. Stellen Sie sicher, dass die Seite **Willkommen bei Azure Container Instances** angezeigt wird. Aktualisieren Sie die Seite mehrmals, um ein paar Protokolleinträge zu erstellen, und schließen Sie dann den Browsertab.  

1. Klicken Sie im Azure-Portal auf dem Blatt der Containerinstanz im Abschnitt **Einstellungen** auf **Container** und anschließend auf **Protokolle**.

1. Stellen Sie sicher, dass die Protokolleinträge für die generierte HTTP GET-Anforderung angezeigt werden, indem Sie die Anwendung im Browser anzeigen.
   
## Bereinigen Ihrer Ressourcen

Falls Sie **Ihr eigenes Abonnement** verwenden, sollten Sie die Labressourcen wieder löschen. Dadurch werden Ressourcen freigegeben und die Kosten minimiert. Die einfachste Möglichkeit zum Löschen der Labressourcen besteht darin, die Ressourcengruppe des Labs zu löschen. 

+ Wählen Sie im Azure-Portal die Ressourcengruppe aus, wählen Sie **Ressourcengruppe** **löschen** aus, geben Sie den Namen der Ressourcengruppe ein, und klicken Sie anschließend auf **Löschen**.
+ Der Azure PowerShell-Befehl dafür lautet: `Remove-AzResourceGroup -Name resourceGroupName`.
+ Der CLI-Befehl lautet: `az group delete --name resourceGroupName`.


## Wichtige Erkenntnisse

Herzlichen Glückwunsch, Sie haben das Lab erfolgreich abgeschlossen. Hier sind die wichtigsten Erkenntnisse für dieses Lab. 

+ Azure Container Instances (ACI) ist ein Dienst, mit dem Sie Container in der öffentlichen Microsoft Azure-Cloud bereitstellen können.
+ Für ACI muss keine zugrunde liegende Infrastruktur bereitgestellt oder verwaltet werden.
+ ACI unterstützt sowohl Linux- als auch Windows-Container.
+ Workloads in ACI werden üblicherweise durch eine Art von Prozess oder Trigger gestartet und beendet und sind in der Regel kurzlebig. 

## Weiterlernen im eigenen Tempo

+ [Ausführen von Containerimages in Azure Container Instances](https://learn.microsoft.com/training/modules/create-run-container-images-azure-container-instances/) Erfahren Sie, wie Azure Container Instances Sie beim schnellen Bereitstellen von Containern unterstützen kann, wie Sie Umgebungsvariablen festlegen und wie Sie Neustartrichtlinien für Container angeben.

    
