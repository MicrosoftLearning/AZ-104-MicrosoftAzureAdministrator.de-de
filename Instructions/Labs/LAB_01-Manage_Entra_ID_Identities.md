---
lab:
  title: "Lab\_01: Verwalten von Microsoft Entra ID-Identitäten"
  module: Administer Identity
---

# Lab 01 – Verwalten von Microsoft Entra ID-Identitäten

# Lab-Handbuch für Kursteilnehmer

## Labszenario

Um Contoso-Benutzer*innen die Authentifizierung mit Microsoft Entra ID zu ermöglichen, wurden Sie mit dem Bereitstellen von Benutzer- und Gruppenkonten beauftragt. Die Mitgliedschaft in den Gruppen soll automatisch auf Grundlage der Tätigkeitsbezeichnungen der Benutzer aktualisiert werden. Sie müssen außerdem einen Testmandanten mit einem Testbenutzerkonto erstellen und diesem Konto eingeschränkte Berechtigungen für Ressourcen im Contoso-Azure-Abonnement gewähren.

**Hinweis:** Eine **[interaktive Labsimulation](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%201)** ist verfügbar, mit der Sie dieses Lab in Ihrem eigenen Tempo durcharbeiten können. Möglicherweise liegen geringfügige Unterschiede zwischen der interaktiven Simulation und dem gehosteten Lab vor, aber die dargestellten Kernkonzepte und Ideen sind identisch.

## Ziele

Dieses Lab deckt Folgendes ab:

+ Aufgabe 1: Erstellen und Konfigurieren von Benutzern
+ Aufgabe 2: Erstellen von Gruppen mit zugewiesener und dynamischer Mitgliedschaft
+ Aufgabe 3: Erstellen eines Mandanten (Optional: Problem mit der Laborumgebung)
+ Aufgabe 4: Verwalten von Gastbenutzer*innen (Optional: Problem mit der Laborumgebung)

## Geschätzte Zeit: 30 Minuten

## Architekturdiagramm
![image](../media/lab01entra.png)

### Anweisungen

## Übung 1

## Aufgabe 1: Erstellen und Konfigurieren von Benutzern

In dieser Aufgabe erstellen und konfigurieren Sie Benutzer.

>**Hinweis**: Wenn Sie zuvor die Testlizenz für Microsoft Entra ID auf diesem Mandanten verwendet haben, benötigen Sie einen neuen Mandanten und führen Aufgabe 2 nach Aufgabe 3 im neuen Mandanten aus.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Suchen Sie im Azure-Portal nach **Microsoft Entra ID**, und wählen Sie sie aus.

1. Scrollen Sie auf dem Blatt „Microsoft Entra ID“ nach unten zum Abschnitt **Verwalten**, klicken Sie auf **Benutzereinstellungen**, und überprüfen Sie die verfügbaren Konfigurationsoptionen.

1. Klicken Sie auf dem Blatt „Microsoft Entra ID“ im Abschnitt **Verwalten** auf **Benutzer** und dann auf Ihr Benutzerkonto, um die Einstellungen für das zugehörige **Profil** anzuzeigen. 

1. Klicken Sie auf **Eigenschaften bearbeiten**, und legen Sie auf der Registerkarte **Einstellungen** den **Nutzungsstandort** auf **USA** fest. Klicken Sie dann auf **Speichern**, um die Änderung zu übernehmen.

    >**Hinweis**: Diese Änderung ist erforderlich, um Ihrem Benutzerkonto später in dieser Übung eine Microsoft Entra ID P2-Lizenz zuzuweisen.

1. Navigieren Sie zurück zum Blatt **Benutzer – Alle Benutzer**, und klicken Sie dann auf **+ Neuer Benutzer**.

1. Erstellen Sie einen neuen Benutzer mit den folgenden Einstellungen (übernehmen Sie für andere Einstellungen die Standardwerte):

    | Einstellung | Wert |
    | --- | --- |
    | Benutzerprinzipalname | **az104-01a-aaduser1** |
    | Anzeigename | **az104-01a-aaduser1** |
    | Kennwort automatisch generieren | Auswahl aufheben |
    | Erstes Kennwort | **Bereitstellen eines sicheren Kennworts** |
    | Position (Registerkarte „Eigenschaften“) | **Cloudadministrator** |
    | Abteilung (Registerkarte „Eigenschaften“) | **IT** |
    | Verwendungsort (Registerkarte „Eigenschaften“) | **USA** |

    >**Hinweis**: Kopieren Sie den vollständigen **Benutzerprinzipalnamen** (Benutzername plus Domäne) über **In Zwischenablage kopieren** in die Zwischenablage. Sie benötigen ihn später in dieser Aufgabe.

1. Klicken Sie in der Liste der Benutzer auf das neu erstellte Benutzerkonto, um das zugehörige Blatt anzuzeigen.

1. Überprüfen Sie die im Abschnitt **Verwalten** verfügbaren Optionen. Beachten Sie, dass Sie die dem Benutzerkonto zugewiesenen Rollen sowie die Berechtigungen des Benutzerkontos für Azure-Ressourcen identifizieren können.

1. Klicken Sie im Abschnitt **Verwalten** auf **Zugewiesene Rollen** und dann auf die Schaltfläche **+ Zuweisung hinzufügen**. Weisen Sie **az104-01a-aaduser1** die Rolle **Benutzeradministrator** zu.

    >**Hinweis**: Sie können Rollen auch bei der Bereitstellung eines neuen Benutzers zuweisen.

1. Öffnen Sie ein **InPrivate**-Browserfenster, und melden Sie sich beim [Azure-Portal](https://portal.azure.com) mit dem neu erstellten Benutzerkonto an. Wenn Sie aufgefordert werden, das Kennwort zu aktualisieren, ändern Sie es in ein sicheres Kennwort Ihrer Wahl. 

    >**Hinweis**: Anstatt den Benutzernamen (einschließlich des Domänennamens) einzugeben, können Sie den Inhalt der Zwischenablage einfügen.

1. Klicken Sie im Azure-Portal im **InPrivate**-Browserfenster auf **Microsoft Entra ID**.

    >**Hinweis**: Dieses Benutzerkonto kann zwar auf den Mandanten zugreifen, hat aber keinen Zugriff auf Azure-Ressourcen. Dies ist das erwartete Verhalten, da ein solcher Zugriff mithilfe der rollenbasierten Azure-Zugriffssteuerung (Role-Based Access Control, RBAC) explizit gewährt werden muss. 

1. Scrollen Sie im **InPrivate**-Browserfenster auf dem Microsoft Entra ID-Blatt nach unten zum Abschnitt **Verwalten**, und klicken Sie auf **Benutzereinstellungen**. Beachten Sie, dass Sie über keinerlei Berechtigungen zum Ändern von Konfigurationsoptionen verfügen.

1. Scrollen Sie im **InPrivate**-Browserfenster auf dem Microsoft Entra ID-Blatt nach unten zum Abschnitt **Verwalten**, klicken Sie auf **Benutzer** und dann auf **+Neuer Benutzer**.

1. Erstellen Sie einen neuen Benutzer mit den folgenden Einstellungen (übernehmen Sie für andere Einstellungen die Standardwerte):

    | Einstellung | Wert |
    | --- | --- |
    | Benutzerprinzipalname | **az104-01a-aaduser2** |
    | Anzeigename | **az104-01a-aaduser2** |
    | Kennwort automatisch generieren | Auswahl aufheben  |
    | Erstes Kennwort | **Bereitstellen eines sicheren Kennworts** |
    | Berufsbezeichnung | **Systemadministrator** |
    | Department | **IT** |
    | Verwendungsstandort | **USA** |
    
1. Melden Sie sich als Benutzer „az104-01a-aaduser1“ über das Azure-Portal an, und schließen Sie das InPrivate-Browserfenster.

## Aufgabe 2: Erstellen von Gruppen mit zugewiesener und dynamischer Mitgliedschaft

In dieser Aufgabe erstellen Sie Gruppen mit zugewiesener und dynamischer Mitgliedschaft.

1. Navigieren Sie im Azure-Portal, in dem Sie mit Ihrem **Benutzerkonto** angemeldet sind, zurück zum Blatt **Übersicht** für den Mandanten. Klicken Sie dann im Abschnitt **Verwalten** auf **Lizenzen**.

    >**Hinweis**: Für die Implementierung dynamischer Gruppen werden Microsoft Entra ID Premium P1- oder P2-Lizenzen benötigt.

1. Klicken Sie im Bereich **Verwalten** auf **Alle Produkte**.

1. Klicken Sie auf **+ Testen/Kaufen**, und aktivieren Sie die kostenlose Testversion von Microsoft Entra ID Premium P2.

1. Aktualisieren Sie das Browserfenster, um zu überprüfen, ob die Aktivierung erfolgreich war. 

    >**Hinweis:** Es kann bis zu 10 Minuten dauern, bis die Lizenzen aktiviert werden. Aktualisieren Sie die Seite weiter, bis sie angezeigt wird. Fahren Sie erst fort, nachdem die Lizenzen aktiviert wurden.

1. Wählen Sie im Bereich **Lizenzen - Alle Produkte** den Eintrag **Microsoft Entra ID P2** und weisen Sie Ihrem Benutzerkonto und den beiden neu erstellten Benutzerkonten alle Lizenzoptionen zu.

1. Navigieren Sie im Azure-Portal zurück zum Blatt des Microsoft Entra ID-Mandanten, und klicken Sie auf **Gruppen**.

1. Verwenden Sie die Schaltfläche **+ Neue Gruppe**, um eine neue Gruppe mit den folgenden Einstellungen zu erstellen:

    | Einstellung | Wert |
    | --- | --- |
    | Gruppentyp | **Sicherheit** |
    | Gruppenname | **IT-Cloudadministratoren** |
    | Gruppenbeschreibung | **IT-Cloudadministratoren von Contoso** |
    | Mitgliedschaftstyp | **Dynamischer Benutzer** |

    >**Hinweis**: Wenn die Dropdownliste **Mitgliedschaftstyp** abgeblendet dargestellt wird, warten Sie einige Minuten, und aktualisieren Sie die Browserseite.

1. Klicken Sie auf **Dynamische Abfrage hinzufügen**.

1. Erstellen Sie auf der Registerkarte **Regeln konfigurieren** des Blatts **Regeln für dynamische Mitgliedschaft** eine neue Regel mit den folgenden Einstellungen:

    | Einstellung | Wert |
    | --- | --- |
    | Eigenschaft | **jobTitle** |
    | Betreiber | **Ist gleich** |
    | Wert | **Cloudadministrator** |

1. Speichern Sie die Regel, indem Sie auf **+ Ausdruck hinzufügen** und anschließend auf **Speichern** klicken. Klicken Sie zurück auf dem Blatt **Neue Gruppe** auf **Erstellen**. 

1. Klicken Sie zurück auf dem Blatt **Gruppen – Alle Gruppen** des Mandanten auf die Schaltfläche **+ Neue Gruppe**, und erstellen Sie eine neue Gruppe mit den folgenden Einstellungen:

    | Einstellung | Wert |
    | --- | --- |
    | Gruppentyp | **Sicherheit** |
    | Gruppenname | **IT-Systemadministratoren** |
    | Gruppenbeschreibung | **IT-Systemadministratoren von Contoso** |
    | Mitgliedschaftstyp | **Dynamischer Benutzer** |

1. Klicken Sie auf **Dynamische Abfrage hinzufügen**.

1. Erstellen Sie auf der Registerkarte **Regeln konfigurieren** des Blatts **Regeln für dynamische Mitgliedschaft** eine neue Regel mit den folgenden Einstellungen:

    | Einstellung | Wert |
    | --- | --- |
    | Eigenschaft | **jobTitle** |
    | Betreiber | **Ist gleich** |
    | Wert | **Systemadministrator** |

1. Speichern Sie die Regel, indem Sie auf **+ Ausdruck hinzufügen** und anschließend auf **Speichern** klicken. Klicken Sie zurück auf dem Blatt **Neue Gruppe** auf **Erstellen**. 

1. Klicken Sie zurück auf dem Blatt **Gruppen – Alle Gruppen** des Mandanten auf die Schaltfläche **+ Neue Gruppe**, und erstellen Sie eine neue Gruppe mit den folgenden Einstellungen:

    | Einstellung | Wert |
    | --- | --- |
    | Gruppentyp | **Sicherheit** |
    | Gruppenname | **IT-Lab-Administratoren** |
    | Gruppenbeschreibung | **IT-Lab-Administratoren von Contoso** |
    | Mitgliedschaftstyp | **Zugewiesen** |
    
1. Klicken Sie auf **Keine Mitglieder ausgewählt**.

1. Suchen Sie auf dem Blatt **Mitglieder hinzufügen** die Gruppen **IT-Cloudadministratoren** und **IT-Systemadministratoren**, und klicken Sie dann auf dem Blatt **Neue Gruppe** auf **Erstellen**.

1. Klicken Sie zurück auf dem Blatt **Gruppen – Alle Gruppen** auf den Eintrag für die Gruppe **IT-Cloudadministratoren**, und zeigen Sie dann das zugehörige Blatt **Mitglieder** an. Überprüfen Sie, ob der Benutzer **az104-01a-aaduser1** in der Liste der Mitglieder angezeigt wird.

    >**Hinweis**: Es kann zu Verzögerungen bei der Aktualisierung der dynamischen Gruppenmitgliedschaften kommen. Um die Aktualisierung zu beschleunigen, navigieren Sie zum Blatt für die Gruppe und zeigen das Blatt **Dynamische Mitgliedschaftsregeln** an. **Bearbeiten** Sie dann die im Textfeld **Regelsyntax** aufgeführte Regel, indem Sie ein Leerzeichen am Ende hinzufügen, und **Speichern** Sie die Änderung.

1. Navigieren Sie zurück zum Blatt **Gruppen – Alle Gruppen**, klicken Sie auf den Eintrag für die Gruppe **IT-Systemadministratoren**, und zeigen Sie dann das zugehörige Blatt **Mitglieder** an. Überprüfen Sie, ob der Benutzer **az104-01a-aaduser2** in der Liste der Mitglieder angezeigt wird.

## Aufgabe 3: Erstellen eines Mandanten (Optional: Problem mit der Laborumgebung)

In dieser Aufgabe erstellen Sie einen neuen Mandanten.
    
1. Suchen Sie im Azure-Portal nach **Microsoft Entra ID**, und wählen Sie sie aus.

    >**Hinweis:** Es liegt ein bekanntes Problem mit der Captcha-Überprüfung in der Laborumgebung vor. Wenn der Fehler **Erstellung aufgrund zu vieler Anforderungen fehlgeschlagen. Versuchen Sie es später erneut.** angezeigt wird, gehen Sie wie folgt vor:<br>
    - Versuchen Sie mehrmals, die Erstellung durchzuführen.<br>
    - Überprüfen Sie den Abschnitt **Mandanten verwalten**, um sicherzustellen, dass der Mandant nicht im Hintergrund erstellt wurde. <br>
    - Öffnen Sie ein neues **InPrivate**-Fenster, und verwenden Sie das Azure-Portal, um den Mandanten von dort aus zu erstellen.<br>
     Lösen Sie das Problem mit dem Trainer, und verwenden Sie dann die **[interaktive Labsimulation](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%201)** , um die Schritte anzuzeigen. <br>
    - Sie können diese Aufgabe später durchführen, aber das Erstellen eines Mandanten ist in anderen Labs nicht erforderlich. 

1. Klicken Sie auf **Mandanten verwalten**, dann im nächsten Bildschirm auf **+ Erstellen**, und geben Sie die folgende Einstellung an:

    | Einstellung | Wert |
    | --- | --- |
    | Verzeichnistyp | **Microsoft Entra ID** |
    
1. Klicken Sie auf **Weiter: Konfiguration**.

    | Einstellung | Wert |
    | --- | --- |
    | Name der Organisation | **Contoso-Lab** |
    | Name der Anfangsdomäne | Ein beliebiger gültiger DNS-Name, der aus Kleinbuchstaben und Ziffern besteht und mit einem Buchstaben beginnt. | 
    | Land/Region | **USA** |

   > **Hinweis**: Der **Name der Anfangsdomäne** darf kein offizieller Name sein, der möglicherweise von Ihrer oder einer anderen Organisation verwendet wird. Das grüne Häkchen im Textfeld **Name der Anfangsdomäne** gibt an, dass der eingegebene Domänenname gültig und eindeutig ist.

1. Klicken Sie auf **Überprüfen + erstellen** und dann auf **Erstellen**.

1. Zeigen Sie das Blatt des neu erstellten Mandanten an, indem Sie auf den Link **Klicken Sie hier, um zu Ihrem neuen Mandanten zu navigieren: Contoso-Lab** oder auf die Schaltfläche **Verzeichnis + Abonnement** (direkt rechts neben der Schaltfläche „Cloud Shell“) in der Symbolleiste des Azure-Portals klicken.

## Aufgabe 4: Erstellen und Verwalten von Gastbenutzern.

In dieser Aufgabe erstellen Sie Gastbenutzer und gewähren ihnen Zugriff auf Ressourcen in einem Azure-Abonnement.

1. Klicken Sie im Azure-Portal, das den Mandanten „Contoso-Lab“ anzeigt, im Abschnitt **Verwalten** auf **Benutzer** und dann auf **+ Neuer Benutzer**.

1. Erstellen Sie einen neuen Benutzer mit den folgenden Einstellungen (übernehmen Sie für andere Einstellungen die Standardwerte):

    | Einstellung | Wert |
    | --- | --- |
    | Benutzerprinzipalname | **az104-01b-aaduser1** |
    | Anzeigename | **az104-01b-aaduser1** |
    | Kennwort automatisch generieren | Auswahl aufheben  |
    | Erstes Kennwort | **Bereitstellen eines sicheren Kennworts** |
    | Berufsbezeichnung | **Systemadministrator** |
    | Department | **IT** |

1. Klicken Sie auf die das neu erstellte Profil.

    >**Hinweis**: Kopieren Sie den vollständigen **Benutzerprinzipalnamen** (Benutzername plus Domäne) über **In Zwischenablage kopieren** in die Zwischenablage. Sie benötigen ihn später in dieser Aufgabe.

1. Kehren Sie zum ersten Mandanten zurück, den Sie zuvor erstellt haben. Verwenden Sie dazu die Schaltfläche **Verzeichnis + Abonnement** (direkt rechts neben der Schaltfläche „Cloud Shell“) in der Symbolleiste des Azure-Portals.

1. Navigieren Sie zurück zum Blatt **Benutzer – Alle Benutzer**, und klicken Sie dann auf **+ Externen Benutzer einladen**.

1. Laden Sie einen neuen Gastbenutzer mit den folgenden Einstellungen ein (übernehmen Sie für andere Einstellungen die Standardwerte):

    | Einstellung | Wert |
    | --- | --- |
    | E-Mail | Der Benutzerprinzipalname, den Sie zu einem früheren Zeitpunkt in dieser Aufgabe kopiert haben |
    | Anzeigename (Registerkarte „Eigenschaften“)  | **az104-01b-aaduser1** |
    | Position (Registerkarte „Eigenschaften“) | **Lab-Administrator** |
    | Abteilung (Registerkarte „Eigenschaften“) | **IT** |
    | Verwendungsort (Registerkarte „Eigenschaften“) | **USA** |

1. Klicken Sie auf **Einladen**. 

1. Klicken Sie zurück auf dem Blatt **Benutzer – Alle Benutzer** auf den Eintrag, der das neu erstellte Gastbenutzerkonto repräsentiert.

1. Klicken Sie auf dem Blatt **az104-01b-aaduser1 – Profil** auf **Gruppen**.

1. Klicken Sie auf **+ Mitgliedschaft hinzufügen**, und fügen Sie das Gastbenutzerkonto der Gruppe **IT-Lab-Administratoren** hinzu.


## Aufgabe 5: Bereinigen von Ressourcen

> **Hinweis**: Denken Sie daran, alle neu erstellten Azure-Ressourcen zu entfernen, die Sie nicht mehr verwenden. Durch das Entfernen nicht verwendeter Ressourcen wird sichergestellt, dass keine unerwarteten Kosten anfallen. Obwohl in diesem Fall keine zusätzlichen Kosten für Mandanten und die zugehörigen Objekte anfallen, sollten Sie in Erwägung ziehen, die in diesem Lab erstellten Benutzerkonten, die Gruppenkonten und den Mandanten zu entfernen.

 > **Hinweis**: Machen Sie sich keine Sorgen, wenn die Labressourcen nicht sofort entfernt werden können. Mitunter haben Ressourcen Abhängigkeiten, sodass der Löschvorgang länger dauert. Es gehört zu den üblichen Administratoraufgaben, die Ressourcennutzung zu überwachen. Überprüfen Sie also regelmäßig Ihre Ressourcen im Portal darauf, wie es um die Bereinigung bestellt ist. 

1. Suchen Sie im **Azure-Portal** in der Suchleiste nach **Microsoft Entra ID**. Wählen Sie unter **Verwalten** die Option **Lizenzen** aus. Wählen Sie in **Lizenzen** unter **Verwalten** die Option **Alle Produkte** und dann in der Liste den Eintrag **Microsoft Entra ID Premium P2** aus. Fahren Sie fort, indem Sie **Lizenzierte Benutzer** auswählen. Wählen Sie die Benutzerkonten **az104-01a-aaduser1** und **az104-01a-aaduser2** aus, denen Sie in diesem Lab Lizenzen zugewiesen haben. Klicken Sie auf **Lizenz entfernen**, und bestätigen Sie den Vorgang bei Aufforderung durch Klicken auf **Ja**.

1. Navigieren Sie im Azure-Portal zum Blatt **Benutzer – Alle Benutzer**, und klicken Sie auf den Eintrag, der das Gastbenutzerkonto **az104-01b-aaduser1** repräsentiert. Klicken Sie auf dem Blatt **az104-01b-aaduser1 – Profil** auf **Löschen**, und bestätigen Sie den Vorgang bei Aufforderung durch Klicken auf **OK**.

1. Wiederholen Sie diese Schrittfolge, um die übrigen Benutzerkonten zu löschen, die Sie in diesem Lab erstellt haben.

1. Navigieren Sie im Azure-Portal zum Blatt **Benutzer – Alle Benutzer**, und wählen Sie die Gruppen aus, die Sie in diesem Lab erstellt haben. Klicken Sie auf **Löschen**, und bestätigen Sie den Vorgang bei Aufforderung durch Klicken auf **OK**.

1. Zeigen Sie im Azure-Portal das Blatt des Mandanten „Contoso-Lab“ an, indem Sie die Schaltfläche **Verzeichnis + Abonnement** (direkt rechts neben der Schaltfläche „Cloud Shell“) in der Symbolleiste des Azure-Portals verwenden.

1. Navigieren Sie zum Blatt **Benutzer – Alle Benutzer**, und klicken Sie auf den Eintrag, der das Benutzerkonto **az104-01b-aaduser1** repräsentiert. Klicken Sie auf dem Blatt **az104-01b-aaduser1 – Profil** auf **Löschen**, und bestätigen Sie den Vorgang bei Aufforderung durch Klicken auf **OK**.

1. Navigieren Sie zum Blatt **Contoso Lab – Übersicht** des Contoso Lab-Mandanten, klicken Sie auf **Mandanten verwalten** , aktivieren Sie auf dem nächsten Bildschirm das Kontrollkästchen neben **Contoso Lab**, klicken Sie auf **Löschen**, dann auf dem Blatt **Mandant „Contoso Labs löschen“?** auf den Link **Berechtigung zum Löschen von Azure-Ressourcen anfordern**. Legen Sie auf dem Blatt **Eigenschaften** die Option **Zugriffsverwaltung für Azure-Ressourcen** auf **Ja** fest, und klicken Sie auf **Speichern**.

1. Navigieren Sie zurück zum Blatt **Mandant „Contoso-Lab“ löschen**, und klicken Sie auf **Aktualisieren**, dann auf **Löschen**.

> **Hinweis:** Wenn es bei einem Mandanten eine Testlizenz gibt, müssten Sie bis zum Ablauf dieser Lizenz warten, bevor Sie den Mandanten löschen könnten. Dies würde keine zusätzlichen Kosten verursachen.

#### Überprüfung

In diesem Lab haben Sie die folgenden Aufgaben ausgeführt:

- Benutzer erstellt und konfiguriert
- Gruppen mit zugewiesener und dynamischer Mitgliedschaft erstellt
- Einen Mandanten erstellt
- Gastbenutzer verwaltet 
