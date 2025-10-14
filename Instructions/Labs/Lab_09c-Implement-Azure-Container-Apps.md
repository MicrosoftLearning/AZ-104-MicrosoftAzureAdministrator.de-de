---
lab:
  title: 'Lab 09c: Implementieren von Azure Container Apps'
  module: Administer PaaS Compute Options
---

# Lab 09c: Implementieren von Azure Container Apps

## Einführung

In diesem Lab erfahren Sie, wie Sie Azure Container Apps implementieren und bereitstellen.

Für dieses Lab wird ein Azure-Abonnement benötigt. Ihr Abonnementtyp kann sich auf die Verfügbarkeit von Features in diesem Lab auswirken. Die Region kann geändert werden. In den Schritten wird allerdings die Region **USA, Osten** verwendet.

## Geschätzte Zeit: 15 Minuten

## Labszenario

Ihre Organisation verfügt über eine Webanwendung, die auf einem virtuellen Computer in Ihrem lokalen Rechenzentrum ausgeführt wird. Die Organisation möchte alle Anwendungen in die Cloud verlagern, aber keine große Anzahl von Servern verwalten. Sie entscheiden sich dafür, Azure Container Apps zu testen.

## Architekturdiagramm

![Diagramm der Aufgaben](../media/az104-lab09b-aca-architecture.png)

## Stellenqualifikationen

- Aufgabe 1: Erstellen und Konfigurieren einer Azure-Container-App und einer entsprechenden Umgebung
- Aufgabe 2: Testen und Überprüfen der Bereitstellung der Azure-Container-App

## Aufgabe 1: Erstellen und Konfigurieren einer Azure-Container-App und einer entsprechenden Umgebung

Azure Container Apps erweitert das Konzept eines verwalteten Kubernetes-Clusters und verwaltet nicht nur die Clusterumgebung, sondern stellt zusätzlich zum Cluster weitere verwaltete Dienste zur Verfügung. Im Vergleich zu einem Azure Kubernetes-Cluster, bei dem Sie weiterhin den Cluster verwalten müssen, beseitigt eine Azure Container Apps-Instanz einige der komplexen Aspekte der Einrichtung eines Kubernetes-Clusters.

1. Suchen Sie im Azure-Portal nach `Container Apps`, und wählen Sie die entsprechende Option aus.

1. Wählen Sie **+ Erstellen**, aus dem Dropdown-Menü, **Container App**. Beachten Sie die anderen Optionen. 

1. Füllen Sie die Registerkarte **Grundeinstellungen** mit den folgenden Informationen aus.

    | Einstellung | Aktion |
    |---|---|
    | Abonnement | Auswählen des Azure-Abonnements |
    | Resource group | `az104-rg9` |
    | Name der Container-App |  `my-app` |
    | Region    | **USA, Osten** |
    | Container-Apps-Umgebung | Wählen Sie **Neu erstellen** > Umgebungsname auf `my-environment` > **Erstellen** festlegen |

1. Klicken Sie auf **Weiter: Container** Registerkarte und stellen Sie sicher, dass **Schnellstartimage verwenden** aktiviert ist. Möglicherweise müssen Sie nach oben scrollen, um diese Einstellung anzuzeigen. 

1. Stellen Sie sicher, dass die **Schnellstartimage** auf **Einfacher Hallo Welt Container** festgelegt ist. Beachten Sie die anderen Optionen. 

1. Wählen Sie **Überprüfen und erstellen** und dann **Erstellen** aus.

    >**Hinweis:** Warten Sie, bis die Container-App bereitgestellt wurde. Dies dauert einige Minuten. 
 
## Aufgabe 2: Testen und Überprüfen der Bereitstellung der Azure-Container-App

Die von Ihnen erstellte Azure-Container-App akzeptiert standardmäßig Datenverkehr am Port 80 unter Verwendung der Hallo Welt-Beispielanwendung. Azure Container Apps stellt einen DNS-Namen für die Anwendung bereit. Kopieren Sie diese URL, und rufen Sie sie auf, um sich zu vergewissern, dass die Anwendung ausgeführt wird.

1. Wählen Sie **Zu Ressource wechseln**, um Ihre neue Container-App anzuzeigen.

1. Wählen Sie den Link neben *Anwendungs-URL* aus, um Ihre Anwendung anzuzeigen.

    ![Screenshot: ACA-Übersichtsseite im Portal](../media/az104-lab09b-aca-overview.png)

1. Vergewissern Sie sich, dass Sie die Nachricht **Ihre Azure Container Apps-App ist live** erhalten haben.
   
## Bereinigen Ihrer Ressourcen

Wenn Sie mit **Ihrem eigenen Abonnement** arbeiten, nehmen Sie sich eine Minute Zeit, um die Labressourcen zu löschen. Dadurch wird sichergestellt, dass Ressourcen freigegeben und Kosten minimiert werden. Die einfachste Möglichkeit zum Löschen der Labressourcen besteht darin, die Ressourcengruppe des Labs zu löschen. 

+ Wählen Sie im Azure-Portal die Ressourcengruppe und dann **Ressourcengruppe löschen** aus, **geben Sie den Ressourcengruppennamen ein**, und klicken Sie dann auf **Löschen**.
+ Bei Verwendung von Azure PowerShell: `Remove-AzResourceGroup -Name resourceGroupName`.
+ Bei Verwendung der Befehlszeilenschnittstelle: `az group delete --name resourceGroupName`.

## Erweitern Ihrer Lernerfahrung mit Copilot
Copilot kann Sie beim Erlernen der Verwendung von Azure-Skripttools unterstützen. Copilot kann Sie auch in Bereichen unterstützen, die nicht im Lab behandelt werden oder in denen Sie weitere Informationen benötigen. Öffnen Sie einen Edge-Browser, und wählen Sie „Copilot“ (rechts oben) aus, oder navigieren Sie zu *copilot.microsoft.com*. Nehmen Sie sich einige Minuten Zeit, um diese Prompts auszuprobieren.

+ Fasse die Schritte zum Erstellen und Konfigurieren einer Azure-Container-App zusammen.
+ Zeige Gemeinsamkeiten und Unterschiede zwischen Azure Container Apps und Azure Kubernetes Service.

## Weiterlernen im eigenen Tempo

+ [Konfigurieren einer Container-App in Azure Container Apps](https://learn.microsoft.com/training/modules/configure-container-app-azure-container-apps/): Dieses Modul geht auf die Features und Funktionen von Azure Container Apps ein und konzentriert sich dann auf das Erstellen, Konfigurieren, Skalieren und Verwalten von Container-Apps mithilfe von Azure Container Apps.


## Wichtige Erkenntnisse

Herzlichen Glückwunsch zum erfolgreichen Abschluss des Labs. Hier sind die wichtigsten Erkenntnisse für dieses Lab. 

+ Azure Container Apps (ACA) ist eine serverlose Plattform, mit der Sie beim Ausführen containerisierter Anwendungen weniger Infrastruktur verwalten müssen und Kosten sparen können.
+ Container Apps bietet Serverkonfiguration, Containerorchestrierung und Bereitstellungsdetails. 
+ Workloads in ACA sind in der Regel Prozesse mit langer Ausführungsdauer (beispielsweise Web-Apps).

     
