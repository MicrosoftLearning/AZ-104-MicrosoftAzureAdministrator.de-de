---
lab:
  title: "Lab\_09a: Implementieren von Web-Apps"
  module: Administer PaaS Compute Options
---

# Lab 09a – Implementieren von Web-Apps


## Einführung

In diesem Lab werden Informationen zu Azure-Web-Apps vermittelt. Sie lernen, eine Web-App so zu konfigurieren, dass eine „Hallo Welt“-Anwendung in einem externen GitHub Repository angezeigt wird. Sie lernen, wie Sie einen Stagingslot erstellen und mit dem Produktionsslot tauschen. Und Sie erhalten Informationen zur automatischen Skalierung, um auf Bedarfsänderungen zu reagieren.

Für dieses Lab wird ein Azure-Abonnement benötigt. Ihr Abonnementtyp kann sich auf die Verfügbarkeit von Features in diesem Lab auswirken. Die Region kann geändert werden. In den Schritten wird allerdings die Region „USA, Osten“ verwendet.

## Geschätzte Zeit: 20 Minuten

## Labszenario

Ihre Organisation möchte Azure-Web-Apps zum Hosten Ihrer Unternehmenswebsites verwenden. Die Websites werden derzeit in einem lokalen Rechenzentrum gehostet. Die Websites werden unter Verwendung des PHP-Runtimestapels auf Windows-Servern ausgeführt. Die Hardware nähert sich dem Ende der Lebensdauer und muss bald ersetzt werden. Ihre Organisation möchte Azure zum Hosten der Websites verwenden, um Kosten für neue Hardware zu vermeiden. 

## Interaktive Labsimulation

Für dieses Thema stehen hilfreiche interaktive Labsimulationen zur Verfügung. In der Simulation können Sie sich in Ihrem eigenen Tempo durch ein ähnliches Szenario klicken. Es gibt zwar Unterschiede zwischen der interaktiven Simulation und diesem Lab, aber viele der Kernkonzepte sind identisch. Ein Azure-Abonnement ist nicht erforderlich.

+ [Erstellen einer Web-App](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%202): Hier erfahren Sie, wie Sie eine Web-App erstellen, die einen Docker-Container ausführt.
    
+ [Implementieren von Azure-Web-Apps](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2013): Hier erfahren Sie, wie Sie eine Azure-Web-App erstellen, die Bereitstellung verwalten und die App skalieren. 

## Architekturdiagramm

![Diagramm der Aufgaben](../media/az104-lab09a-architecture.png)

## Stellenqualifikationen

+ Aufgabe 1: Erstellen und Konfigurieren einer Azure-Web-App
+ Aufgabe 2: Erstellen und Konfigurieren eines Bereitstellungsslots
+ Aufgabe 3: Konfigurieren von Web-App-Bereitstellungseinstellungen
+ Aufgabe 4: Austauschen von Bereitstellungsslots
+ Aufgabe 5: Konfigurieren und Testen der automatischen Skalierung der Azure-Web-App

## Aufgabe 1: Erstellen und Konfigurieren einer Azure-Web-App

In dieser Aufgabe wird eine Azure-Web-App erstellt. Azure App Services ist eine PaaS-Lösung (Platform-as-a-Service) für Webanwendungen, mobile Anwendungen und andere webbasierte Anwendungen. Azure-Web-Apps sind Teil von Azure App Services, von dem die meisten Runtimeumgebungen wie PHP, Java und .NET gehostet werden. Der von Ihnen ausgewählte App Service-Plan hat Auswirkungen auf die Compute- und Speicherressourcen sowie auf die Features der Web-App. 

1. Melden Sie sich beim **Azure-Portal** - `https://portal.azure.com` an.

1. Suchen Sie nach `App services`, und wählen Sie diese Option aus.

1. Wählen Sie **+ Erstellen** und anschließend im Dropdownmenü die Option **Web-App** aus. Beachten Sie die anderen Optionen. 

1. Geben Sie auf der Registerkarte **Grundlagen** des Blatts **Web-App erstellen** die folgenden Einstellungen an (übernehmen Sie für andere Einstellungen die Standardwerte):

    | Einstellung | Wert |
    | --- | ---|
    | Subscription | Ihr Azure-Abonnement |
    | Resource group | `az104-rg9` (Wählen Sie bei Bedarf **Neu erstellen** aus.) |
    | Web-App-Name | Ein global eindeutiger Name |
    | Veröffentlichen | **Code** |
    | Laufzeitstapel | **PHP 8.2** |
    | Betriebssystem | **Linux** |
    | Region | **USA, Osten** |
    | Tarife | **Premium V3 P1V3** |
    | Zonenredundanz | Übernehmen Sie die Standardeinstellungen. |

 1. Klicken Sie auf **Überprüfen + erstellen** und dann auf **Erstellen**.

    >**Hinweis:** Warten Sie, bis die Web-App erstellt wurde, bevor Sie mit der nächsten Aufgabe fortfahren. Dieser Vorgang dauert etwa eine Minute.
    
    >**Hinweis**: Wenn die Bereitstellung fehlschlägt, wechseln Sie zu einer anderen Region, und versuchen Sie es erneut. Wechseln Sie beispielsweise zu **USA, Osten 2**. 

1. Wählen Sie nach der erfolgreichen Bereitstellung **Zu Ressource wechseln** aus.

## Aufgabe 2: Erstellen und Konfigurieren eines Bereitstellungsslots

In dieser Aufgabe erstellen Sie einen Stagingbereitstellungsslot. Bereitstellungsslots ermöglichen die Durchführung von Tests, bevor Sie Ihre App öffentlich (oder für Ihre Endbenutzer) verfügbar machen. Nach dem Testen können Sie den Slot aus der Entwicklungs- oder Stagingphase in die Produktion überführen. Viele Organisationen verwenden Slots für Tests vor der Produktion. Zudem nutzen viele Organisationen mehrere Slots für jede Anwendung (z. B. für Entwicklung, Qualitätssicherung, Tests und Produktion).

1. Klicken Sie auf dem Blatt der neu bereitgestellten Web-App auf den Link **Standarddomäne**, um die Standardwebseite auf einem neuen Browsertab anzuzeigen.

1. Schließen Sie den neuen Browsertab. Kehren Sie zum Azure-Portal zurück, und klicken Sie im Abschnitt **Bereitstellung** des Blatts der Web-App auf **Bereitstellungsslots**.

    >**Hinweis:** Die Web-App hat zu diesem Zeitpunkt einen einzelnen Bereitstellungsslot mit der Bezeichnung **PRODUKTION**.

1. Klicken Sie auf **Slot hinzufügen**, und fügen Sie einen neuen Slot mit den folgenden Einstellungen hinzu:

    | Einstellung | Wert |
    | --- | ---|
    | Name | `staging` |
    | Einstellungen klonen von: | **Einstellungen nicht klonen**|

1. Wählen Sie **Hinzufügen** aus.

1. Klicken Sie auf dem Blatt **Bereitstellungsslots** der Web-App auf den Eintrag, der den neu erstellten Stagingslot darstellt.

    >**Hinweis**: Auf diese Weise wird das Blatt geöffnet, das die Eigenschaften des Stagingslots zeigt.

1. Überprüfen Sie das Blatt des Stagingslots, und beachten Sie, dass sich die zugehörige URL von der URL unterscheidet, die dem Produktionsslot zugewiesen ist.

## Aufgabe 3: Konfigurieren von Web-App-Bereitstellungseinstellungen

In dieser Aufgabe werden Einstellungen für die Web-App-Bereitstellung konfiguriert. Bereitstellungseinstellungen ermöglichen Continuous Deployment. Dadurch wird sichergestellt, dass der App-Dienst über die neueste Version der Anwendung verfügt.

1. Wählen Sie im Stagingslot die Option **Bereitstellungscenter** und anschließend **Einstellungen** aus.

    >**Hinweis:** Vergewissern Sie sich, dass Sie sich auf dem Blatt für den Stagingslot (und nicht auf dem Blatt für den Produktionsslot) befinden.
    
1. Wählen Sie in der Dropdownliste **Quelle** den Eintrag **Git extern** aus. Beachten Sie die anderen Optionen. 

1. Geben Sie im Repositoryfeld die Zeichenfolge `https://github.com/Azure-Samples/php-docs-hello-world` ein.

1. Geben Sie im Branchfeld die Zeichenfolge `master` ein.

1. Wählen Sie **Speichern**.

1. Wählen Sie im Stagingslot die Option **Übersicht** aus.

1. Wählen Sie den Link **Standarddomäne** aus, und öffnen Sie die URL auf einem neuen Tab. 

1. Vergewissern Sie sich, dass der Stagingslot **Hello World** (Hallo Welt) anzeigt.

>**Hinweis:** Die Bereitstellung kann etwas dauern. Wählen Sie **Aktualisieren** aus, um die Anwendungsseite zu aktualisieren.

## Aufgabe 4: Austauschen von Bereitstellungsslots

In dieser Aufgabe wird der Stagingslot mit dem Produktionsslot getauscht. Durch Austauschen eines Slots können Sie den Code, den Sie im Stagingslot getestet haben, in die Produktion überführen. Im Azure-Portal wird außerdem eine Aufforderung angezeigt, wenn weitere Anwendungseinstellungen überführt werden müssen, die Sie für den Slot angepasst haben. Das Austauschen von Slots ist eine gängige Aufgabe für Anwendungsteams und Anwendungssupportteams – insbesondere bei der routinemäßigen Bereitstellung von App-Updates und Fehlerbehebungen.

1. Kehren Sie zum Blatt **Bereitstellungsslots** zurück, und wählen Sie **Austausch** aus.

1. Überprüfen Sie die Standardeinstellungen, und klicken Sie auf **Austausch starten**.

1. Wählen Sie auf dem Blatt **Übersicht** der Web-App den Link **Standarddomäne** aus, um die Homepage der Website anzuzeigen.

1. Vergewissern Sie sich, dass auf der Produktionswebseite **Hello World!** (Hallo Welt!) angezeigt wird. .

    >**Hinweis:** Kopieren Sie unter **URL** die URL der Standarddomäne. Sie wird für den Auslastungstest in der nächsten Aufgabe benötigt. 

## Aufgabe 5: Konfigurieren und Testen der automatischen Skalierung der Azure-Web-App

In dieser Aufgabe wird die automatische Skalierung der Azure-Web-App konfiguriert. Die automatische Skalierung sorgt dafür, dass Ihre Web-App auch bei zunehmendem Datenverkehr für die Web-App optimal funktioniert. Um zu steuern, wann die App skaliert werden soll, können Sie Metriken wie CPU-Auslastung, Arbeitsspeicher oder Bandbreite überwachen.

1. Wählen Sie im Abschnitt **Einstellungen** die Option **Aufskalieren (App Service-Plan)** aus.

    >**Hinweis:** Achten Sie darauf, dass Sie den Produktionsslot (und nicht den Stagingslot) verwenden.  

1. Wählen Sie im Abschnitt **Skalierung** die Option **Automatisch** aus. Beachten Sie die Option **Regelbasiert**. Die regelbasierte Skalierung kann für verschiedene App-Metriken konfiguriert werden. 

1. Wählen Sie im Feld **Maximaler Burst** den Wert **2** aus.

    ![Screenshot: Seite „Autoskalierung“](../media/az104-lab09a-autoscale.png)

1. Wählen Sie **Speichern**.

1. Wählen Sie **Probleme diagnostizieren und beheben** (linker Bereich) aus.

1. Wählen Sie im Feld **Ausführen von Auslastungstests für eine App** die Option **Auslastungstest erstellen** aus.

    + Wählen Sie **+ Erstellen** aus, und geben Sie unter **Name** einen Namen für den Auslastungstest an.  Der Name muss eindeutig sein.
    + Wählen Sie **Überprüfen + erstellen** und danach **Erstellen** aus.

1. Warten Sie, bis der Auslastungstest erstellt wurde, und wählen Sie anschließend **Zu Ressource wechseln** aus.

1. Wählen Sie unter **Übersicht** | **HTTP-Anforderungen hinzufügen** die Option **Erstellen** aus.

1. Wählen Sie auf der Registerkarte **Testplan** die Option **Anforderung hinzufügen** aus. Fügen Sie im Feld **URL** die URL Ihrer **Standarddomäne** ein. Achten Sie darauf, dass sie ordnungsgemäß formatiert ist und mit **https://** beginnt.

1. Wählen Sie **Überprüfen + erstellen** und dann **Erstellen** aus.

    >**Hinweis:** Die Erstellung des Tests kann einige Minuten dauern. 

1. Überprüfen Sie die Testergebnisse, einschließlich **Virtuelle Benutzer**, **Antwortzeit** und **Anforderungen/s**.

1. Wählen Sie **Beenden** aus, um die Testausführung abzuschließen.

## Bereinigen Ihrer Ressourcen

Wenn Sie mit **Ihrem eigenen Abonnement** arbeiten, nehmen Sie sich eine Minute Zeit, um die Labressourcen zu löschen. Dadurch wird sichergestellt, dass Ressourcen freigegeben und Kosten minimiert werden. Die einfachste Möglichkeit zum Löschen der Labressourcen besteht darin, die Ressourcengruppe des Labs zu löschen. 

+ Wählen Sie im Azure-Portal die Ressourcengruppe und dann **Ressourcengruppe löschen** aus, **geben Sie den Ressourcengruppennamen ein**, und klicken Sie dann auf **Löschen**.
+ Bei Verwendung von Azure PowerShell: `Remove-AzResourceGroup -Name resourceGroupName`.
+ Bei Verwendung der Befehlszeilenschnittstelle: `az group delete --name resourceGroupName`.

## Erweitern Ihrer Lernerfahrung mit Copilot
Copilot kann Sie beim Erlernen der Verwendung von Azure-Skripttools unterstützen. Copilot kann Sie auch in Bereichen unterstützen, die nicht im Lab behandelt werden oder in denen Sie weitere Informationen benötigen. Öffnen Sie einen Edge-Browser, und wählen Sie „Copilot“ (rechts oben) aus, oder navigieren Sie zu *copilot.microsoft.com*. Nehmen Sie sich einige Minuten Zeit, um diese Prompts auszuprobieren.

+ Fasse die Schritte zum Erstellen und Konfigurieren einer Azure-Web-App zusammen.
+ Wie kann ich eine Azure-Web-App skalieren?

## Weiterlernen im eigenen Tempo

+ [Staging einer Web-App-Bereitstellung für Test- und Rollbackzwecke mithilfe von App Service-Bereitstellungsslots](https://learn.microsoft.com/training/modules/stage-deploy-app-service-deployment-slots/). Verwenden Sie Bereitstellungsslots, um Bereitstellung und Rollback einer Web-App in Azure App Service zu optimieren.
+ [Skalieren einer App Service-Web-App mit App Service-Hochskalierung und -Aufskalierung zur effizienten Erfüllung der Anforderungen](https://learn.microsoft.com/training/modules/app-service-scale-up-scale-out/): Reagieren Sie auf Phasen erhöhter Aktivität, indem Sie die verfügbaren Ressourcen schrittweise erhöhen und diese dann bei abnehmender Aktivität verringern, um Kosten zu senken.

## Wichtige Erkenntnisse

Herzlichen Glückwunsch zum erfolgreichen Abschluss des Labs. Hier sind die wichtigsten Erkenntnisse für dieses Lab. 

+ Mit Azure App Services können Sie Web-Apps schnell erstellen, bereitstellen und skalieren.
+ App Service unterstützt zahlreiche Entwicklerumgebungen wie ASP.NET, Java, PHP und Python.
+ Mithilfe von Bereitstellungsslots können separate Umgebungen zum Bereitstellen und Testen Ihrer Web-App erstellt werden.
+ Sie können eine Web-App manuell oder automatisch skalieren, um auf zunehmenden Bedarf zu reagieren.
+ Es steht eine breite Palette von Diagnosen und Testtools zur Verfügung. 
