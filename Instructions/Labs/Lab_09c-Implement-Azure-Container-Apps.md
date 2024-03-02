---
lab:
  title: 'Lab 09c: Implementieren von Azure Container Apps'
  module: Administer PaaS Compute Options
---

# Lab 09c: Implementieren von Azure Container Apps

## Einführung

In diesem Lab erfahren Sie, wie Sie Azure Container Apps implementieren und bereitstellen.

Für dieses Lab wird ein Azure-Abonnement benötigt. Ihr Abonnementtyp kann sich auf die Verfügbarkeit von Features in diesem Lab auswirken. Die Region kann geändert werden. In den Schritten wird allerdings die Region **USA, Osten** verwendet.

## Geschätzte Zeit: 15 Minuten

## Labszenario

Ihre Organisation verfügt über eine Webanwendung, die auf einem virtuellen Computer in Ihrem lokalen Rechenzentrum ausgeführt wird. Die Organisation möchte alle Anwendungen in die Cloud verlagern, aber keine große Anzahl von Servern verwalten. Sie entscheiden sich dafür, Azure Container Apps zu testen.

## Interaktive Labsimulation

Für dieses Thema sind keine interaktiven Labsimulationen verfügbar. 

## Stellenqualifikationen

- Aufgabe 1: Erstellen und Konfigurieren einer Azure-Container-App und einer entsprechenden Umgebung
- Aufgabe 2: Testen und Überprüfen der Bereitstellung der Azure-Container-App

## Architekturdiagramm

![Diagramm der Aufgaben](../media/az104-lab09b-aca-architecture.png)

## Aufgabe 1: Erstellen und Konfigurieren einer Azure-Container-App und einer entsprechenden Umgebung

Azure Container Apps erweitert das Konzept eines verwalteten Kubernetes-Clusters und verwaltet nicht nur die Clusterumgebung, sondern stellt zusätzlich zum Cluster weitere verwaltete Dienste zur Verfügung. Im Vergleich zu einem Azure Kubernetes-Cluster, bei dem Sie weiterhin den Cluster verwalten müssen, beseitigt eine Azure Container Apps-Instanz einige der komplexen Aspekte der Einrichtung eines Kubernetes-Clusters.

1. Suchen Sie im Azure-Portal nach `Container Apps`, und wählen Sie die entsprechende Option aus.

1. Wählen Sie in **Container Apps** die Option **Erstellen** aus.

1. Füllen Sie die Registerkarte **Grundeinstellungen** mit den folgenden Informationen aus.*

    | Einstellung | Aktion |
    |---|---|
    | Abonnement | Wählen Sie Ihr Azure-Abonnement aus. |
    | Resource group | `az104-rg9` |
    | Name der Container-App |  `my-app` |
    | Region    | **USA, Osten** (oder eine in Ihrer Nähe verfügbare Region) |
    | Container Apps-Umgebung | Standardeinstellung beibehalten |

1. Stellen Sie auf der Registerkarte **Container** sicher, dass die Option **Verwenden des Schnellstartimages** aktiviert und das Schnellstartimage auf **Einfacher Hallo Welt-Container** festgelegt ist.

1. Wählen Sie **Überprüfen und erstellen** und dann **Erstellen** aus.

    >**Hinweis:** Warten Sie, bis die Container-App bereitgestellt wurde. Dies dauert einige Minuten. 
 
## Aufgabe 2: Testen und Überprüfen der Bereitstellung der Azure-Container-App

Die von Ihnen erstellte Azure-Container-App akzeptiert standardmäßig Datenverkehr am Port 80 unter Verwendung der Hallo Welt-Beispielanwendung. Azure Container Apps stellt einen DNS-Namen für die Anwendung bereit. Kopieren Sie diese URL, und rufen Sie sie auf, um sich zu vergewissern, dass die Anwendung ausgeführt wird.

1. Wählen Sie **Zu Ressource wechseln**, um Ihre neue Container-App anzuzeigen.

1. Wählen Sie den Link neben *Anwendungs-URL* aus, um Ihre Anwendung anzuzeigen.

    ![Screenshot: ACA-Übersichtsseite im Portal](../media/az104-lab09b-aca-overview.png)

1. Vergewissern Sie sich, dass Sie die Nachricht **Ihre Azure Container Apps-App ist live** erhalten haben.
   
## Bereinigen Ihrer Ressourcen

Falls Sie **Ihr eigenes Abonnement** verwenden, sollten Sie die Labressourcen wieder löschen. Dadurch werden Ressourcen freigegeben und die Kosten minimiert. Die einfachste Möglichkeit zum Löschen der Labressourcen besteht darin, die Ressourcengruppe des Labs zu löschen. 

+ Wählen Sie im Azure-Portal die Ressourcengruppe aus, wählen Sie **Ressourcengruppe** **löschen** aus, geben Sie den Namen der Ressourcengruppe ein, und klicken Sie anschließend auf **Löschen**.
+ Der Azure PowerShell-Befehl dafür lautet: `Remove-AzResourceGroup -Name resourceGroupName`.
+ Der CLI-Befehl lautet: `az group delete --name resourceGroupName`.



## Wichtige Erkenntnisse

Herzlichen Glückwunsch, Sie haben das Lab erfolgreich abgeschlossen. Hier sind die wichtigsten Erkenntnisse für dieses Lab. 

+ Azure Container Apps (ACA) ist eine serverlose Plattform, mit der Sie beim Ausführen containerisierter Anwendungen weniger Infrastruktur verwalten müssen und Kosten sparen können.
+ Container Apps bietet Serverkonfiguration, Containerorchestrierung und Bereitstellungsdetails. 
+ Workloads in ACA sind in der Regel Prozesse mit langer Ausführungsdauer (beispielsweise Web-Apps).

## Weiterlernen im eigenen Tempo

+ [Konfigurieren einer Container-App in Azure Container Apps](https://learn.microsoft.com/training/modules/configure-container-app-azure-container-apps/): Dieses Modul geht auf die Features und Funktionen von Azure Container Apps ein und konzentriert sich dann auf das Erstellen, Konfigurieren, Skalieren und Verwalten von Container-Apps mithilfe von Azure Container Apps.
     
