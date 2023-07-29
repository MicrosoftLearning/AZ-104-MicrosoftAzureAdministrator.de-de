---
lab:
  title: 'Lab 09c: Implementieren von Azure Container Apps'
  module: Administer PaaS Compute Options
---

# Lab 09c: Implementieren von Azure Container Apps
# Lab-Handbuch für Kursteilnehmer

## Labszenario
Mit Azure Container Apps können Sie Microservices und Containeranwendungen auf einer serverlosen Plattform ausführen. Mit Container Apps genießen Sie die Vorteile von Containern und müssen sich nicht mehr um die manuelle Konfiguration von Cloudinfrastrukturen und komplexen Containerorchestratoren kümmern.

## Ziele

In diesem Lab werden folgende Aufgaben ausgeführt:
- Aufgabe 1: Erstellen einer Container-App und -Umgebung
- Aufgabe 2: Bereitstellen der Container-App
- Aufgabe 3: Testen und Überprüfen der Bereitstellung der Container-App

Melden Sie sich zunächst beim [Azure-Portal](https://portal.azure.com) an.

## Geschätzte Zeit: 20 Minuten

## Aufgabe 1: Erstellen einer Container-App und -Umgebung

Beginnen Sie auf der Startseite des Azure-Portals, um Ihre Container-App zu erstellen.

1. Suchen Sie in der oberen Suchleiste nach `Container Apps`.
1. Wählen Sie in den Suchergebnissen **Container Apps** aus.
1. Wählen Sie die Schaltfläche **Erstellen**.

### Registerkarte „Grundlagen“

Gehen Sie auf der Registerkarte *Grundeinstellungen* wie folgt vor:

1. Geben Sie im Abschnitt *Projektdetails* die folgenden Werte ein.

    | Einstellung | Aktion |
    |---|---|
    | Subscription | Wählen Sie Ihr Azure-Abonnement. |
    | Resource group | Wählen Sie **Neu erstellen** aus, und geben Sie `az104-09c-rg1` ein. |
    | Name der Container-App |  Geben Sie `my-container-app` ein. |

#### Erstellen einer Umgebung

Erstellen Sie als Nächstes eine Umgebung für Ihre Container-App.

1. Wählen Sie die geeignete Region aus.

    | Einstellung | Wert |
    |--|--|
    | Region | **Beliebig**. |

1. Wählen Sie im Feld *Container Apps-Umgebung erstellen* den Link **Neu erstellen** aus.
1. Geben Sie auf der Seite *Container Apps-Umgebung erstellen* auf der Registerkarte *Grundlagen* die folgenden Werte ein:

    | Einstellung | Wert |
    |--|--|
    | Umgebungsname | Geben Sie `my-environment` ein. |
    | Zonenredundanz | Wählen Sie **Deaktiviert** aus. |

1. Wählen Sie die Registerkarte **Überwachung** aus, um einen Log Analytics-Arbeitsbereich zu erstellen.
1. Wählen Sie im Feld *Log Analytics-Arbeitsbereich* den Link **Neu erstellen** aus, und geben Sie die folgenden Werte ein:

    | Einstellung | value |
    |--|--|
    | Name | Geben Sie `my-container-apps-logs` ein. |
  
    Im Feld *Standort* ist bereits eine Region für Sie eingegeben.

1. Klicken Sie auf **OK**.


## Aufgabe 2: Bereitstellen der Container-App

1. Wählen Sie unten auf der Seite die Schaltfläche **Überprüfen und erstellen** aus.  

    Als Nächstes werden die Einstellungen in der Container-App überprüft. Wenn keine Fehler gefunden werden, wird die Schaltfläche *Erstellen* aktiviert.  

    Werden Fehler gefunden, wird jede Registerkarte, die Fehler enthält, mit einem roten Punkt markiert.  Navigieren Sie zur entsprechenden Registerkarte. Felder mit einem Fehler sind rot hervorgehoben.  Wenn Sie alle Fehler behoben haben, wählen Sie erneut **Überprüfen und erstellen** aus.

1. Klicken Sie auf **Erstellen**.

    Eine Seite mit der Meldung *Bereitstellung wird durchgeführt* wird angezeigt.  Nach dem erfolgreichen Abschluss der Bereitstellung wird die Meldung *Ihre Bereitstellung wurde abgeschlossen* angezeigt.
   
## Aufgabe 3: Testen und Überprüfen der Bereitstellung der Container-App

Wählen Sie **Zu Ressource wechseln**, um Ihre neue Container-App anzuzeigen.  Wählen Sie den Link neben *Anwendungs-URL* aus, um Ihre Anwendung anzuzeigen. Vergewissern Sie sich, dass die Meldung *Willkommen bei Azure Container Apps* angezeigt wird.

## Bereinigen von Ressourcen

Wenn Sie diese Anwendung nicht weiter verwenden möchten, können Sie die Azure Container Apps-Instanz und alle zugehörigen Dienste löschen, indem Sie die Ressourcengruppe entfernen.

1. Wählen Sie im Abschnitt *Übersicht* die Ressourcengruppe **my-container-apps** aus.
1. Wählen Sie oben in der *Übersicht* der Ressourcengruppe die Option **Ressourcengruppe löschen** aus.
1. Geben Sie den Namen der Ressourcengruppe ein, und bestätigen Sie, dass Sie die App löschen möchten. 
1. Klicken Sie auf **Löschen**.
1. Der Vorgang zum Löschen der Ressourcengruppe kann einige Minuten dauern.
