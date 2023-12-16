---
lab:
  title: "Lab\_02a: Verwalten von Abonnements und RBAC"
  module: Administer Governance and Compliance
---

# Lab 02a – Verwalten von Abonnements und RBAC
# Lab-Handbuch für Kursteilnehmer

## Labanforderungen

Für dieses Lab werden Berechtigungen zum Erstellen von Benutzern, zum Erstellen benutzerdefinierter RBAC-Rollen (Azure Role Based Access Control) sowie Berechtigungen zum Zuweisen dieser Rollen zu Benutzern benötigt. Möglicherweise stellen nicht alle Lab-Hoster diese Funktion zur Verfügung. Erkundigen Sie sich bei Ihrem Kursleiter nach der Verfügbarkeit dieses Labs.

## Labszenario

Um die Verwaltung der Azure-Ressourcen von Contoso zu verbessern, wurden Sie mit der Implementierung der folgenden Funktionalität beauftragt:

- Erstellen einer Verwaltungsgruppe, die alle Azure-Abonnements von Contoso enthält.

- Erteilen von Berechtigungen zum Übermitteln von Supportanfragen für alle Abonnements in der Verwaltungsgruppe an einen bestimmten Benutzer. Die Berechtigungen dieses Benutzers müssen beschränkt werden auf: 

    - Erstellen von Supportanfragetickets
    - Anzeigen von Ressourcengruppen

**Hinweis:** Eine **[interaktive Labsimulation](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%202)** ist verfügbar, mit der Sie dieses Lab in Ihrem eigenen Tempo durcharbeiten können. Möglicherweise liegen geringfügige Unterschiede zwischen der interaktiven Simulation und dem gehosteten Lab vor, aber die dargestellten Kernkonzepte und Ideen sind identisch.

## Ziele

Dieses Lab deckt Folgendes ab:

+ Aufgabe 1: Implementieren von Verwaltungsgruppen
+ Aufgabe 2: Erstellen von benutzerdefinierten RBAC-Rollen 
+ Aufgabe 3: Zuweisen von RBAC-Rollen


## Geschätzte Zeit: 30 Minuten

## Architekturdiagramm

![image](../media/lab02aentra.png)


### Anweisungen

## Übung 1

## Aufgabe 1: Implementieren von Verwaltungsgruppen

In dieser Aufgabe erstellen und konfigurieren Sie Verwaltungsgruppen. 

1. Melden Sie sich am [**Azure-Portal**](http://portal.azure.com) an.

1. Suchen Sie nach der Option **Verwaltungsgruppen**, und wählen Sie diese aus, um zum Blatt **Verwaltungsgruppen** zu navigieren.

1. Überprüfen Sie die Meldungen oben auf dem Blatt **Verwaltungsgruppen**. Wenn die Meldung **Sie sind als Verzeichnisadministrator registriert, aber haben keine ausreichenden Berechtigungen zum Zugriff auf die Stammverwaltungsgruppe** angezeigt wird, führen Sie die folgenden Schritte aus:

    1. Suchen Sie im Azure-Portal nach **Microsoft Entra ID**, und wählen Sie sie aus.
    
    1.  Wählen Sie auf dem Blatt mit den Eigenschaften Ihres Mandanten im vertikalen Menü auf der linken Seite im Abschnitt **Verwalten** die Option **Eigenschaften** aus.
    
    1.  Wählen Sie auf dem Blatt **Eigenschaften** Ihres Mandanten im Abschnitt **Zugriffsverwaltung für Azure-Ressourcen** die Option **Ja** und dann **Speichern** aus.
    
    1.  Navigieren Sie zurück zum Blatt **Verwaltungsgruppen**, und wählen Sie **Aktualisieren** aus.

1. Klicken Sie auf dem Blatt **Verwaltungsgruppen** auf **+ Erstellen**.

    >**Hinweis**: Wenn Sie noch keine Verwaltungsgruppen erstellt haben, wählen Sie **Mit der Verwendung von Verwaltungsgruppen beginnen** aus.

1. Erstellen Sie eine neue Verwaltungsgruppe mit den folgenden Einstellungen:

    | Einstellung | Wert |
    | --- | --- |
    | Verwaltungsgruppen-ID | **az104-02-mg1** |
    | Anzeigename der Verwaltungsgruppe | **az104-02-mg1** |

1. Klicken Sie in der Liste der Verwaltungsgruppen auf den Eintrag, der die neu erstellte Verwaltungsgruppe repräsentiert.

1. Klicken Sie auf dem Blatt **az104-02-mg1** auf **Abonnements**. 

1. Klicken Sie auf dem Blatt **az104-02-mg1 \| Abonnements** auf **+ Hinzufügen**. Wählen Sie auf dem Blatt **Abonnement hinzufügen** in der Dropdownliste **Abonnement** das Abonnement aus, das Sie in diesem Lab verwenden, und klicken Sie auf **Speichern**.

    >**Hinweis**: Kopieren Sie auf dem Blatt **az104-02-mg1 \| Abonnements** die ID Ihres Azure-Abonnements in die Zwischenablage. Sie werden dies in der nächsten Aufgabe benötigen.

## Aufgabe 2: Erstellen von benutzerdefinierten RBAC-Rollen

In dieser Aufgabe erstellen Sie eine Definition einer benutzerdefinierten RBAC-Rolle.

1. Öffnen Sie auf dem Lab-Computer die Datei **\\Allfiles\\Labs\\02\\az104-02a-customRoleDefinition.json** in Editor, und überprüfen Sie ihren Inhalt:

   ```json
   {
      "Name": "Support Request Contributor (Custom)",
      "IsCustom": true,
      "Description": "Allows to create support requests",
      "Actions": [
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Support/*"
      ],
      "NotActions": [
      ],
      "AssignableScopes": [
          "/providers/Microsoft.Management/managementGroups/az104-02-mg1",
          "/subscriptions/SUBSCRIPTION_ID"
      ]
   }
   ```
    > **Hinweis:** Wenn Sie nicht genau wissen, wo die Dateien in Ihrer Labumgebung lokal gespeichert werden, fragen Sie Ihren Kursleiter.

1. Ersetzen Sie den Platzhalter `SUBSCRIPTION_ID` in der JSON-Datei durch die Abonnement-ID, die Sie in die Zwischenablage kopiert haben, und speichern Sie die Änderung.

1. Öffnen Sie im Azure-Portal den Bereich **Cloud Shell**, indem Sie auf das Symbolleistensymbol rechts neben dem Textfeld für die Suche klicken.

1. Wenn Sie aufgefordert werden, entweder **Bash** oder **PowerShell** auszuwählen, wählen Sie **PowerShell** aus. 

    >**Hinweis**: Wenn Sie **Cloud Shell** zum ersten Mal starten und die Meldung **Für Sie wurde kein Speicher bereitgestellt** angezeigt wird, wählen Sie das in diesem Lab verwendete Abonnement aus, und klicken Sie dann auf **Speicher erstellen**. 

1. Klicken Sie auf der Symbolleiste des Cloud Shell-Bereichs auf das Symbol **Dateien hochladen/herunterladen**, klicken Sie im Dropdownmenü auf **Hochladen**, und laden Sie die Datei **\\Allfiles\\Labs\\02\\az104-02a-customRoleDefinition.json** in das Cloud Shell-Basisverzeichnis hoch.

1. Führen Sie im Cloud Shell-Bereich den folgenden Befehl aus, um die benutzerdefinierte Rollendefinition zu erstellen:

   ```powershell
   New-AzRoleDefinition -InputFile $HOME/az104-02a-customRoleDefinition.json
   ```

1. Schließen Sie den Cloud Shell-Bereich.

## Aufgabe 3: Zuweisen von RBAC-Rollen

In dieser Aufgabe erstellen Sie einen Benutzer, weisen ihm die RBAC-Rolle zu, die Sie in der vorherigen Aufgabe erstellt haben, und überprüfen, ob der Benutzer die in der RBAC-Rollendefinition angegebene Aufgabe ausführen kann.

1. Suchen Sie im Azure-Portal nach **Microsoft Entra ID**, und wählen Sie sie aus. Klicken Sie auf **Benutzer** und dann auf **+Neuer Benutzer**.

1. Erstellen Sie einen neuen Benutzer mit den folgenden Einstellungen (übernehmen Sie für andere Einstellungen die Standardwerte):

    | Einstellung | Wert |
    | --- | --- |
    | Benutzername | **az104-02-aaduser1**|
    | Name | **az104-02-aaduser1**|
    | Kennwort selbst erstellen | enabled |
    | Erstes Kennwort | **Bereitstellen eines sicheren Kennworts** |

    >**Hinweis**: Kopieren Sie den vollständigen **Benutzernamen** über **In Zwischenablage kopieren** in die Zwischenablage. Sie benötigen ihn später in diesem Lab.

1. Navigieren Sie im Azure-Portal zurück zur Verwaltungsgruppe **az104-02-mg1**, und zeigen Sie deren **Details** an.

1. Klicken Sie auf **Access Control (IAM)** , dann auf **+ Hinzufügen** und wählen Sie anschließend **Rollenzuweisung hinzufügen**. Suchen Sie auf der Registerkarte **Rolle** nach **Supportanfrage-Mitwirkender (Benutzerdefiniert)**. 

    >**Hinweis**: Wenn Ihre benutzerdefinierte Rolle nicht sichtbar ist, kann es bis zu 10 Minuten dauern, bis die benutzerdefinierte Rolle nach der Erstellung angezeigt wird.

1. Wählen Sie die **Rolle** aus, und klicken Sie auf **Weiter**. Klicken Sie auf der Registerkarte **Mitglieder** auf **+ Mitglieder auswählen**, und **wählen** Sie das Benutzerkonto az104-***********************.**********.onmicrosoft.com aus. Klicken Sie auf **Weiter** und dann auf **Überprüfen und Zuweisen**.

1. Öffnen Sie ein **InPrivate**-Browserfenster, und melden Sie sich beim [Azure-Portal](https://portal.azure.com) mit dem neu erstellten Benutzerkonto an. Wenn Sie aufgefordert werden, das Kennwort zu aktualisieren, ändern Sie das Kennwort für den Benutzer.

    >**Hinweis**: Anstatt den Benutzernamen einzugeben, können Sie den Inhalt der Zwischenablage einfügen.

1. Wählen Sie im **InPrivate**-Browserfenster im Azure-Portal die Option **Ressourcengruppen** aus, und stellen Sie sicher, dass der Benutzer „az104-02-aaduser1“ alle Ressourcengruppen sehen kann.

1. Stellen Sie im **InPrivate**-Browserfenster im Azure-Portal anhand der Option **Alle Ressourcen** sicher, dass der Benutzer „az104-02-aaduser1“ keine Ressourcen sehen kann.

1. Wählen Sie im **InPrivate**-Browserfenster im Azure-Portal die Option **Hilfe + Support** aus, und klicken Sie dann auf **+ Supportanfrage erstellen**. 

1. Geben Sie im **InPrivate**-Browserfenster auf der Registerkarte **Problembeschreibung/Zusammenfassung** des Blatts **Hilfe + Support – Neue Supportanfrage** den Text **Grenzwerte für Dienste und Abonnements** in das Feld für die Zusammenfassung ein, und wählen Sie den Problemtyp **Grenzwerte für Dienste und Abonnements (Kontingente)** . Beachten Sie, dass das in dieser Übung verwendete Abonnement in der Dropdownliste **Abonnement** aufgeführt ist.

    >**Hinweis**: Das Vorhandensein des in diesem Lab verwendeten Abonnements in der Dropdownliste **Abonnement** zeigt an, dass das von Ihnen verwendete Konto über die erforderlichen Berechtigungen zum Erstellen der abonnementspezifischen Supportanfrage verfügt.

    >**Hinweis**: Wenn Ihnen die Option **Grenzwerte für Dienste und Abonnements (Kontingente)** nicht angezeigt wird, melden Sie sich beim Azure-Portal ab und wieder an.

1. Fahren Sie nicht mit dem Erstellen der Supportanfrage fort. Melden Sie sich stattdessen über das Azure-Portal als Benutzer „az104-02-aaduser1“ an, und schließen Sie das InPrivate-Browserfenster.

## Aufgabe 4: Bereinigen der Ressourcen

   >**Hinweis**: Denken Sie daran, alle neu erstellten Azure-Ressourcen zu entfernen, die Sie nicht mehr verwenden. Durch das Entfernen von ungenutzten Ressourcen wird sichergestellt, dass keine unerwarteten Gebühren anfallen. Beachten Sie jedoch, dass die in diesem Lab erstellten Ressourcen keine zusätzlichen Kosten verursachen.

   >**Hinweis:** Machen Sie sich keine Sorgen, wenn die Labressourcen nicht sofort entfernt werden können. Mitunter haben Ressourcen Abhängigkeiten, sodass der Löschvorgang länger dauert. Es gehört zu den üblichen Administratoraufgaben, die Ressourcennutzung zu überwachen. Überprüfen Sie also regelmäßig Ihre Ressourcen im Portal darauf, wie es um die Bereinigung bestellt ist.

1. Suchen Sie im Azure-Portal nach **Microsoft Entra ID**, und wählen Sie sie aus. Klicken Sie auf **Benutzer**.

1. Klicken Sie auf dem Blatt **Benutzer – Alle Benutzer** auf **az104-02-aaduser1**.

1. Kopieren Sie auf dem Blatt **az104-02-aaduser1 – Profil** den Wert des Attributs **Objekt-ID**.

1. Starten Sie im Azure-Portal im Bereich **Cloud Shell** eine **PowerShell**-Sitzung.

1. Führen Sie im Cloud Shell-Bereich den folgenden Befehl aus, um die Zuweisung der benutzerdefinierten Rollendefinition zu entfernen (ersetzen Sie den Platzhalter `[object_ID]` durch den Wert des Attributs **Objekt-ID** des Benutzerkontos **az104-02-aaduser1**, den Sie zuvor in dieser Aufgabe kopiert haben):

   ```powershell
   
    $scope = (Get-AzRoleDefinition -Name 'Support Request Contributor (Custom)').AssignableScopes | Where-Object {$_ -like '*managementgroup*'}
    
    Remove-AzRoleAssignment -ObjectId '[object_ID]' -RoleDefinitionName 'Support Request Contributor (Custom)' -Scope $scope
   ```

1. Führen Sie im Cloud Shell-Bereich den folgenden Befehl aus, um die benutzerdefinierte Rollendefinition zu entfernen:

   ```powershell
   Remove-AzRoleDefinition -Name 'Support Request Contributor (Custom)' -Force
   ```

1. Navigieren Sie im Azure-Portal zurück zum Blatt **Benutzer – Alle Benutzer** in **Microsoft Entra ID**, und löschen Sie das Benutzerkonto **az104-02-aaduser1**.

1. Navigieren Sie im Azure-Portal zurück zum Blatt **Verwaltungsgruppen**. 

1. Wählen Sie auf dem Blatt **Verwaltungsgruppen** das Symbol mit den **Auslassungspunkten** neben Ihrem Abonnement unter der Verwaltungsgruppe **az104-02-mg1** aus. Wählen Sie dann **Verschieben** aus, um das Abonnement in die **Stammverwaltungsgruppe des Mandanten** zu verschieben.

   >**Hinweis**: Wahrscheinlich handelt es sich bei der Zielverwaltungsgruppe um die **Stammverwaltungsgruppe des Mandanten**, sofern Sie vor der Durchführung dieses Labs keine benutzerdefinierte Verwaltungsgruppenhierarchie erstellt haben.
   
1. Klicken Sie auf **Aktualisieren**, um zu überprüfen, ob das Abonnement erfolgreich in die **Stammverwaltungsgruppe des Mandanten** verschoben wurde.

1. Navigieren Sie zurück zum Blatt **Verwaltungsgruppen**, klicken Sie auf das Symbol mit den **Auslassungspunkten** rechts neben der Verwaltungsgruppe **az104-02-mg1**, und klicken Sie auf **Löschen**.
  >**Hinweis:** Wenn Sie die **Stammverwaltungsgruppe des Tenants** nicht löschen können, befindet sich das **Azure-Abonnement** wahrscheinlich unter der Verwaltungsgruppe. Sie müssen das **Azure-Abonnement** aus der **Verwaltungsgruppe des Mandantenstamms** verschieben und dann die Gruppe löschen.

## Überprüfung

In diesem Lab haben Sie die folgenden Aufgaben ausgeführt:

- Implementieren von Verwaltungsgruppen
- Erstellen von benutzerdefinierten RBAC-Rollen 
- Zuweisen von RBAC-Rollen
