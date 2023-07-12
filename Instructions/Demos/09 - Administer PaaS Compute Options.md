---
demo:
  title: "Demo\_09: Verwalten von PaaS-Computeoptionen"
  module: Administer PaaS Compute Options
---

# 09: Verwalten von PaaS-Computeoptionen

## Konfigurieren von Azure App Service-Plänen

In dieser Demo erstellen und verwenden wir Azure App Service-Pläne.

**Referenz:** [Verwalten von App Service-Plänen – Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-plan-manage)

**Referenz:** [Hochskalieren einer App in Azure App Service](https://learn.microsoft.com/azure/app-service/manage-scale-up)

**Referenz:** [Automatische Skalierung in Azure App Service](https://learn.microsoft.com/azure/app-service/manage-automatic-scaling?tabs=azure-portal)

1. Verwenden Sie dafür das Azure-Portal. 

1. Suchen Sie nach  **App Service-Plänen**, und wählen Sie diese Option aus.

1. Erstellen Sie einen einfachen App Service-Plan. Besprechen Sie die Notwendigkeit, Windows oder Linux auszuwählen. Besprechen Sie die Preispläne jetzt oder in den nächsten Schritten. 

1. Stellen Sie Ihren neuen App Service-Plan bereit. 

1. Sehen Sie sich das Blatt **Hochskalieren (App Service Plan)** an. Diskutieren Sie den Unterschied zwischen **Dev/Test**-Plänen und **Produktionsplänen**. Sehen Sie sich die Featureliste an. 

1. Sehen Sie sich das Blatt **Aufskalieren (App Service Plan)** an. Überprüfen Sie den Unterschied zwischen **Manual** (manuell) und **Rule=based** (regelbasiert). 

## Konfigurieren von Azure App Services

In dieser Demo erstellen wir eine neue Web-App, in der ein Docker-Container ausgeführt wird.  Der Container zeigt eine Begrüßungsnachricht an.

**Referenz:** [Erstellen einer Web-App](https://learn.microsoft.com/training/modules/host-a-web-app-with-azure-app-service/3-exercise-create-a-web-app-in-the-azure-portal?pivots=csharp)

In dieser Aufgabe erstellen Sie eine Azure App Service-Web-App.

1. Verwenden Sie dafür das Azure-Portal. 

1. Suchen Sie nach  **App Services**, und wählen Sie die Option aus.

1. **Erstellen** Sie eine **Web-App**.

    - Veröffentlichen: **Code**. Sehen Sie sich die anderen Optionen an.
    - Runtimestapel. **.NET**. Sehen Sie sich die anderen Optionen an.
    - Betriebssystem: **Linux**

1. Wählen Sie den Dienstplan **Free F1** aus.

1. **Überprüfen und erstellen** Sie die Web-App. Warten Sie, bis die Ressource bereitgestellt wurde.

1. Stellen Sie auf der Seite **Übersicht** sicher, dass der **Status** **Wird ausgeführt** lautet.

1. Wählen Sie die **URL** aus, und stellen Sie sicher, dass die Seite für Standardplatzhalter geladen wird.

1. Wenn Sie Zeit haben, erkunden Sie die Optionen für **Bereitstellungsslots**.
   
## Konfigurieren von Azure Container Instances

In dieser Demo werden wir einen Container mithilfe von Azure Container Instances (ACI) über das Azure Portal erstellen, konfigurieren und bereitstellen. Die ACI-Anwendung zeigt eine statische HTML-Seite mit dem öffentlichen Microsoft-Image „Hallo Welt“ an. 

**Referenz:** [Schnellstart: Bereitstellen von Docker-Containern in einer Containerinstanz](https://learn.microsoft.com/en-us/azure/container-instances/container-instances-quickstart-portal)

1. Verwenden Sie dafür das Azure-Portal.

1. Suchen Sie nach  **Containerinstanzen**, und wählen Sie die Option aus.

1. **Erstellen** Sie eine neue Containerinstanz. 

1. Geben Sie die **Ressourcengruppe** und den **Containernamen** ein. 

1. Diskutieren Sie die Optionen für die **Imagequelle**. Verwenden Sie **Schnellstartimages**.

1. Verwenden Sie für **Containerimage** **mcr.microsoft.com/azuredocs/aci-helloworld:latest (Linux)** . Dieses Linux-Beispielimage verpackt eine kleine in Node.js geschriebene Web-App, die eine statische HTML-Seite bedient.

1. Geben Sie auf der Seite **Netzwerk** eine **DNS-Namensbezeichnung** für Ihren Container an. 

1. Behalten Sie für alle anderen Einstellungen die Standardwerte bei, und wählen Sie dann **Überprüfen + erstellen** aus.

1. Warten Sie, bis die Ressource bereitgestellt wurde.

1. Stellen Sie auf der Seite **Übersicht** für die Ressource sicher, dass der **Status** **Wird ausgeführt** lautet.

1. Navigieren Sie zum **FQDN** für die Containerinstanz, und stellen Sie sicher, dass die Willkommensseite angezeigt wird. 

**Hinweis:** Löschen Sie die Ressource, um zusätzliche Kosten zu vermeiden. 

## Konfigurieren von Azure Container Apps

In dieser Demo erstellen und verwenden wir Azure Container Apps-Instanzen. 

**Referenz:** [Schnellstart: Bereitstellen Ihrer ersten Container-App über das Azure-Portal](https://learn.microsoft.com/azure/container-apps/quickstart-portal)

1. Suchen Sie nach **Container-Apps**, und wählen Sie diese Option aus.

1. Füllen Sie die **Projektdetails** aus, und erstellen Sie die Container-Apps-**Umgebung**.

1. **Überprüfen und erstellen Sie** die Container-App.

1. Verwenden Sie den **Anwendungs-URL**-Link, um Ihre Anwendung anzuzeigen.

1. Überprüfen Sie, ob im Browser die Meldung **Willkommen bei Azure Container Apps** angezeigt wird. 






