---
lab:
  title: 'Lab 01: Verwalten von Microsoft Entra ID-Identitäten'
  module: Administer Identity
---

# Lab 01 – Verwalten von Microsoft Entra ID-Identitäten

## Einführung in das Lab

Dies ist das erste in einer Reihe von Labs für Azure-Administratoren. In dieser Übung erfahren Sie mehr über Benutzer und Gruppen. Benutzer und Gruppen sind die grundlegenden Bausteine für eine Identitätslösung. 

Für dieses Lab wird ein Azure-Abonnement benötigt. Ihr Abonnementtyp kann sich auf die Verfügbarkeit von Features in diesem Lab auswirken. Sie können die Region ändern, aber in den Schritten wird die Region **USA, Osten** verwendet. 


## Geschätzte Zeit: 30 Minuten

## Labszenario

Ihre Organisation erstellt eine neue Labumgebung für Vorproduktionstests von Apps und Diensten.  Einige Techniker werden eingestellt, um die Labumgebung zu verwalten, einschließlich den VMs. Damit sich die Techniker mithilfe von Microsoft Entra ID authentifizieren können, wurden Sie mit der Bereitstellung von Benutzern und Gruppen beauftragt. Um den Verwaltungsaufwand zu minimieren, sollte die Mitgliedschaft in den Gruppen basierend auf den Positionen automatisch aktualisiert werden. 

## Interaktive Labsimulation

Dieses Lab verwendet eine interaktive Laborsimulation. In der Simulation können Sie sich in Ihrem eigenen Tempo durch ein ähnliches Szenario klicken. Es gibt zwar Unterschiede zwischen der interaktiven Simulation und diesem Lab, aber viele der Kernkonzepte sind identisch. Ein Azure-Abonnement ist nicht erforderlich.

>**Hinweis:** Diese Simulation wird aktualisiert. Microsoft Entra ID ist der neue Name für Azure Active Directory (Azure AD). 

+ [Verwalten von Entra ID-Identitäten](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%201). Erstellen und konfigurieren Sie Benutzer und weisen diese zu Gruppen hinzu. Erstellen Sie einen Azure-Mandanten, und verwalten Sie Gastkonten. 

## Architekturdiagramm
![Diagramm der Lab 01-Architektur.](../media/az104-lab01-architecture.png)

## Stellenqualifikationen

+ Aufgabe 1: Erstellen und Konfigurieren von Benutzerkonten.
+ Aufgabe 2: Erstellen von Gruppen hinzufügen von Mitgliedern.

## Aufgabe 1: Erstellen und Konfigurieren von Benutzerkonten

In dieser Aufgabe werden Sie Benutzerkonten erstellen und konfigurieren. Benutzerkonten werden Benutzerdaten wie Name, Abteilung, Standort und Kontaktinformationen speichern.

1. Melden Sie sich beim **Azure-Portal** - `https://portal.azure.com` an.

    >**Hinweis:** Das Azure-Portal wird in allen Labs verwendet. Wenn Sie mit Azure noch nicht vertraut sind, suchen Sie nach `Quickstart Center` und wählen dies aus. Nehmen Sie sich ein paar Minuten Zeit, um sich das Video **Erste Schritte im Azure-Portal** anzusehen. Selbst wenn Sie das Portal zuvor verwendet haben, werden Sie einige Tipps und Tricks zum Navigieren und Anpassen der Schnittstelle finden.
    
1. Suchen Sie nach `Microsoft Entra ID`, und wählen Sie diese Option aus. Microsoft Entra ID ist die cloudbasierte Verwaltungslösung von Azure für Identität und Zugriff. Nehmen Sie sich einige Minuten Zeit, um sich mit einigen der im linken Bereich aufgeführten Features vertraut zu machen. 

1. Wählen Sie das Blatt **Übersicht** und dann die Registerkarte **Mandanten verwalten** aus. 

    >**Schon gewusst?** Ein Mandant ist eine bestimmte Instanz von Microsoft Entra ID, die Konten und Gruppen enthält. Je nach Ihrer Situation können Sie weitere Mandanten erstellen und zwischen ihnen **wechseln**. 

1. Kehren Sie zur Seite **Entra ID** zurück, und wählen Sie **Lizenzen** aus. Von hier aus können Sie eine Lizenz erwerben, die von Ihnen erworbenen Lizenzen verwalten und Benutzern und Gruppen Lizenzen zuweisen. Wählen Sie **Lizenzierte Features** aus, um zu sehen, was verfügbar ist.
   
### Erstellen eines neuen Benutzers

1. Wählen Sie **Benutzer** und dann in der Dropdownliste **Neuer Benutzer** die Option **Neuen Benutzer erstellen** aus. 

1. Erstellen Sie einen neuen Benutzer mit den folgenden Einstellungen (belassen Sie andere auf ihren Standardwerten). Beachten Sie auf der Registerkarte **Eigenschaften** alle verschiedenen Arten von Informationen, die im Benutzerkonto enthalten sein können. 

    | Einstellung | Wert |
    | --- | --- |
    | Benutzerprinzipalname | `az104-user1` |
    | Anzeigename | `az104-user1` |
    | Kennwort automatisch generieren | **checked** |
    | Konto aktiviert? | **checked** |
    | Position (Registerkarte „Eigenschaften“) | `IT Lab Administrator` |
    | Abteilung (Registerkarte „Eigenschaften“) | `IT` |
    | Verwendungsort (Registerkarte „Eigenschaften“) | **USA** |

1. Nachdem Sie die Überprüfung abgeschlossen haben, wählen Sie **Überprüfen + Erstellen** und dann **Erstellen** aus.

1. Aktualisieren Sie die Seite, und bestätigen Sie, dass Ihr neuer Benutzer erstellt wurde. 

### Einladen eines externen Benutzers

1. Wählen Sie in der Dropdownliste **Neuer Benutzer** die Option **Externen Benutzer einladen** aus. 

    | Einstellung | Wert |
    | --- | --- |
    | E‑Mail | Ihre E-Mail-Adresse |
    | Anzeigename | Ihr Name |
    | Senden einer Einladungsnachricht | **Kontrollkästchen aktivieren** |
    | `Message` | `Welcome to Azure and our group project` |

1. Wechseln Sie zur Registerkarte **Eigenschaften**. Füllen Sie die grundlegenden Informationen einschließlich dieser Felder aus. 

    | Einstellung | Wert |
    | --- | --- |
    | Position  | `IT Lab Administrator` |
    | Department  | `IT` |
    | Verwendungsort (Registerkarte „Eigenschaften“) | **USA** |

1. Wählen Sie **Überprüfen+ Einladen** und dann **Einladen** aus.

1. **Aktualisieren** Sie die Seite, und bestätigen Sie, dass der eingeladene Benutzer erstellt wurde. Sie sollten die Einladungs-E-Mail in Kürze erhalten. 

    >**Hinweis:** Es ist unwahrscheinlich, dass Sie Benutzerkonten einzeln erstellen werden. Wissen Sie, wie Ihre Organisation Benutzerkonten erstellen und verwalten möchte?
    
## Aufgabe 2: Erstellen von Gruppen und Hinzufügen von Mitgliedern

In dieser Aufgabe erstellen Sie ein Gruppenkonto. Gruppenkonten können Benutzerkonten oder Geräte enthalten. Dies sind zwei grundlegende Methoden, wie Mitgliedern Gruppen zugewiesen werden: Statisch und Dynamisch. Statische Gruppen erfordern, dass Administratoren Mitglieder manuell hinzufügen und entfernen.  Dynamische Gruppen werden automatisch basierend auf den Eigenschaften eines Benutzerkontos oder Geräts aktualisiert. Beispiel: Position. 

1. Suchen Sie im Azure-Portal nach `Groups` und wählen Sie es aus.

1. Nehmen Sie sich eine Minute Zeit, um sich mit den Gruppeneinstellungen im linken Bereich vertraut zu machen.

   + Mit dem **Ablauf** können Sie eine Gruppenlebensdauer in Tagen konfigurieren. Danach muss die Gruppe vom Besitzer erneuert werden.
   + Mithilfe der **Benennungsrichtlinie** können Sie blockierte Wörter konfigurieren und Gruppennamen ein Präfix oder Suffix hinzufügen.

1. Wählen Sie im Blatt **Alle Gruppen** die Option **+ Neue Gruppe** aus, und erstellen Sie eine neue Gruppe.     

    | Einstellung | Wert |
    | --- | --- |
    | Gruppentyp | **Sicherheit** |
    | Gruppenname | `IT Lab Administrators` |
    | Gruppenbeschreibung | `Administrators that manage the IT lab` |
    | Mitgliedschaftstyp | **Zugewiesen** |

    >**Hinweis:** Für die dynamische Mitgliedschaft ist eine Entra ID Premium P1- oder P2-Lizenz erforderlich. Wenn andere **Mitgliedschaftstypen** verfügbar sind, werden die Optionen in der Dropdownliste angezeigt. 
    
    ![Screenshot „zugewiesene Gruppe erstellen“.](../media/az104-lab01-create-assigned-group.png)

1. Wählen Sie **Keine Besitzer ausgewählt** aus.

1. Suchen Sie auf der Seite **Besitzer hinzufügen** nach sich selbst, und **wählen** sich als Besitzer aus. Beachten Sie, dass Sie mehrere Besitzer haben können. 

1. Wählen Sie die Option **Keine Mitglieder ausgewählt** aus.

1. Suchen Sie im Bereich **Mitglieder hinzufügen** nach **az104-user1** und dem von Ihnen eingeladenen **Gastbenutzer** und **wählen** diese aus. Fügen Sie beide Benutzer zur Gruppe hinzu. 

1. Wählen Sie **Erstellen** aus, um die Gruppe bereitzustellen.

1. **Aktualisieren** Sie die Seite, und stellen Sie sicher, dass Ihre Gruppe erstellt wurde.

1. Wählen Sie die neue Gruppe aus, und überprüfen Sie die Informationen zu **Mitgliedern** und **Besitzern**.

>**Hinweis:** Möglicherweise verwalten Sie eine große Anzahl von Gruppen. Verfügt Ihre Organisation über einen Plan zum Erstellen von Gruppen und Hinzufügen von Mitgliedern?
   
## Bereinigen Ihrer Ressourcen

Wenn Sie mit **Ihrem eigenen Abonnement** arbeiten, nehmen Sie sich eine Minute Zeit, um die Labressourcen zu löschen. Dadurch wird sichergestellt, dass Ressourcen freigegeben und Kosten minimiert werden. Die einfachste Möglichkeit zum Löschen der Labressourcen besteht darin, die Ressourcengruppe des Labs zu löschen. 

+ Wählen Sie im Azure-Portal die Ressourcengruppe und dann **Ressourcengruppe löschen** aus, **geben Sie den Ressourcengruppennamen ein**, und klicken Sie dann auf **Löschen**.
+ Bei Verwendung von Azure PowerShell: `Remove-AzResourceGroup -Name resourceGroupName`.
+ Bei Verwendung der Befehlszeilenschnittstelle: `az group delete --name resourceGroupName`.
  

## Erweitern Ihrer Lernerfahrung mit Copilot

Copilot kann Sie beim Erlernen der Verwendung von Azure-Skripttools unterstützen. Copilot kann Sie auch in Bereichen unterstützen, die nicht im Lab behandelt werden oder in denen Sie weitere Informationen benötigen. Öffnen Sie einen Edge-Browser, und wählen Sie „Copilot“ (rechts oben) aus, oder navigieren Sie zu *copilot.microsoft.com*. Nehmen Sie sich einige Minuten Zeit, um diese Prompts auszuprobieren.
+ Was sind die Azure PowerShell- und CLI-Befehle zum Erstellen einer Sicherheitsgruppe namens „IT-Administratoren“? Geben Sie die offizielle Referenzseite des Befehls an.  
+ Stellen Sie eine schrittweise Strategie für die Verwaltung von Benutzer*innen und Gruppen in Microsoft Entra ID bereit.
+ Welche Schritte sind im Azure-Portal erforderlich, um Benutzer*innen und Gruppen in Massen zu erstellen?
+ Stellen Sie eine Vergleichstabelle mit internen und externen Microsoft Entra ID-Benutzerkonten bereit. 


## Weiterlernen im eigenen Tempo

+ [Grundlegendes zu Microsoft Entra ID](https://learn.microsoft.com/training/modules/understand-azure-active-directory/). Vergleichen Sie Microsoft Entra ID mit Active Directory DS, lernen Sie Microsoft Entra ID P1 und P2 kennen, und erkunden Sie Microsoft Entra Domain Services für die Verwaltung von in Domänen eingebundene Geräte und Apps in der Cloud.
+ [Erstellen von Azure-Benutzern und -Gruppen in Microsoft Entra ID](https://learn.microsoft.com//training/modules/create-users-and-groups-in-azure-active-directory/). Erstellen Sie Benutzer in Microsoft Entra ID. Grundlegende Informationen zu den verschiedenartigen Gruppen. Erstellen einer Gruppe und Hinzufügen von Mitgliedern. Verwalten von Business-to-Business-Gastkonten.
+ [Zulassen, dass Benutzer*innen ihr Kennwort mit der Self-Service-Kennwortzurücksetzung von Microsoft Entra zurücksetzen können](https://learn.microsoft.com/training/modules/allow-users-reset-their-password/). Evaluieren der Self-Service-Kennwortzurücksetzung, damit Benutzer in Ihrer Organisation ihre Kennwörter zurücksetzen oder ihre Konten entsperren können. Einrichten, Konfigurieren und Testen der Self-Service-Kennwortzurücksetzung.


## Wichtige Erkenntnisse

Herzlichen Glückwunsch zum erfolgreichen Abschluss des Labs. Hier sind einige wichtige Erkenntnisse für dieses Lab:

+ Ein Mandant stellt Ihre Organisation dar und unterstützt Sie dabei, eine bestimmte Microsoft Cloud Services-Instanz für Ihre internen und externen Benutzer zu verwalten.
+ Microsoft Entra ID verfügt über Benutzer- und Gastkonten. Jedes Konto verfügt über eine Zugriffsebene, die für den Umfang der zu erwartenden Arbeit spezifisch ist.
+ Gruppen fassen verwandte Benutzer oder Geräte zusammen. Es gibt zwei Arten von Gruppen, einschließlich Sicherheit und Microsoft 365.
+ Die Gruppenmitgliedschaft kann statisch oder dynamisch zugewiesen werden.
