---
demo:
  title: "Demo\_02: Verwalten von Governance und Compliance"
  module: Administer Governance and Compliance
---

# 02: Verwalten von Governance und Compliance

## Konfigurieren von Abonnements

In diesem Abschnitt gibt es keine formelle Demonstration. 

**Referenz:** [Erstellen eines zusätzlichen Azure-Abonnements](https://docs.microsoft.com/azure/cost-management-billing/manage/create-subscription)

## Konfigurieren von Azure Policy

In dieser Demo verwenden wir Azure-Richtlinien.

**Referenz:** [Tutorial: Erstellen von Richtlinien zur Erzwingung von Compliance – Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Zuweisen einer Richtlinie**

1.  Öffnen Sie das Azure-Portal.

2.  Suchen Sie nach  **Richtlinie**, und wählen Sie diese Option aus.

3.  Wählen Sie **Zuweisungen** und dann **Richtlinie zuweisen** aus.

5.  Besprechen Sie den  **Bereich** . Er bestimmt, für welche Ressourcen oder Ressourcengruppe die Richtlinienzuweisung erzwungen wird.

6.  Wählen Sie die Auslassungspunkte (...) der Option **Richtliniendefinition** aus, um die Liste mit den verfügbaren Definitionen zu öffnen. Nehmen Sie sich einen Moment Zeit, um die integrierten Richtliniendefinitionen zu überprüfen.

7.  Suchen Sie nach der Richtlinie **Zulässige Standorte**, und wählen Sie sie aus. Mit dieser Richtlinie können Sie die Speicherorte einschränken, die Ihre Organisation beim Bereitstellen von Ressourcen angeben kann.

8.  Navigieren Sie zur Registerkarte **Parameter** , und wählen Sie im Dropdownmenü mindestens einen zulässigen Standort aus.

9.  Klicken Sie auf **Überprüfen und erstellen** und dann auf **Erstellen** , um die Richtlinie zu erstellen.

**Erstellen und Zuweisen einer Initiativendefinition**

1.  Kehren Sie zurück zur Seite „Azure Policy“, und wählen Sie unter „Erstellung“ die Option **Definitionen** aus.

2.  Wählen Sie oben auf der Seite **Initiativendefinition** aus.

3.  Geben Sie einen **Namen** und eine **Beschreibung** an.

4.  **Erstellen Sie eine neue** Kategorie.

5.   **Fügen Sie** im rechten Panel die Richtlinie **Zulässige Standorte** hinzu.

6.  Fügen Sie eine weitere Richtlinie Ihrer Wahl hinzu.

7.  **Speichern** Sie Ihre Änderungen, und **weisen Sie** dann Ihre Initiativendefinition Ihrem Abonnement zu.

**Überprüfen der Compliance**

1.  Navigieren Sie zurück zur Seite des Azure Policy-Diensts.

2.  Wählen Sie **Compliance** aus.

3.  Überprüfen Sie den Status Ihrer Richtlinie und Definition.

**Überprüfen auf Wartungsaufgaben**

1.  Navigieren Sie zurück zur Seite des Azure Policy-Diensts.

2.  Wählen Sie **Korrektur** aus.

3.  Überprüfen Sie alle aufgeführten Wartungsaufgaben.

4. Wenn Sie Zeit haben, entfernen Sie die Richtlinie und die Initiative. 

## Konfigurieren der rollenbasierten Zugriffssteuerung

In dieser Demo erfahren Sie mehr über Rollenzuweisungen.

**Referenz:** [Tutorial: Gewähren des Zugriffs auf Azure-Ressourcen für Benutzer*innen über das Azure-Portal – Azure RBAC](https://docs.microsoft.com/azure/role-based-access-control/quickstart-assign-role-user-portal)

**Referenz:** [Schnellstart: Überprüfen des Zugriffs von Benutzer*innen auf Azure-Ressourcen – Azure RBAC](https://docs.microsoft.com/azure/role-based-access-control/check-access)

**Auffinden des Blatts „Zugriffssteuerung“**

1.  Greifen Sie auf das Azure-Portal zu, und wählen Sie eine Ressourcengruppe aus. Notieren Sie sich, welche Ressourcengruppe Sie verwenden.

2.  Wählen Sie das Blatt **Zugriffssteuerung (IAM)**  aus.

3.  Dieses Blatt ist für viele verschiedene Ressourcen verfügbar, damit Sie Berechtigungen steuern können.

**Überprüfen von Rollenberechtigungen**

1.  Wählen Sie die Registerkarte **Rollen** (oben) aus.

1.  Sehen Sie sich die große Anzahl von integrierten Rollen an, die verfügbar sind.

1.  Doppelklicken Sie auf eine Rolle, und wählen Sie  **Berechtigungen** (oben) aus.

1.  Führen Sie einen Drilldown für die Rolle durch, bis die Aktionen  **Lesen, Schreiben und Löschen** für diese Rolle angezeigt werden.

1.  Kehren Sie zum Blatt  **Zugriffssteuerung (IAM)**  zurück.

**Hinzufügen einer Rollenzuweisung**

1.  Erstellen Sie einen Benutzer, oder wählen Sie einen vorhandenen Benutzer aus.

1.  Wählen Sie **Rollenzuweisung hinzufügen** und dann eine Rolle aus. Beispiel: *owner*.

1.  Wählen Sie **Zugriff überprüfen** aus.

1.  Gehen Sie noch einmal auf die Benutzerberechtigungen ein.

1.  Beachten Sie, dass Sie **Zuweisungen verweigern** können.
