---
lab:
  title: 01 – Verwalten von Azure Directory-Identitäten
  module: Administer Identity
---

# <a name="lab-01---manage-azure-active-directory-identities"></a>Lab 01 – Verwalten von Azure Directory-Identitäten

# <a name="student-lab-manual"></a>Lab-Handbuch für Kursteilnehmer

## <a name="lab-scenario"></a>Labszenario

In order to allow Contoso users to authenticate by using Azure AD, you have been tasked with provisioning users and group accounts. Membership of the groups should be updated automatically based on the user job titles. You also need to create a test Azure AD tenant with a test user account and grant that account limited permissions to resources in the Contoso Azure subscription.

## <a name="objectives"></a>Ziele

Dieses Lab deckt Folgendes ab:

+ Aufgabe 1: Erstellen und Konfigurieren von Azure AD-Benutzern
+ Aufgabe 2: Erstellen von Azure AD-Gruppen mit zugewiesener und dynamischer Mitgliedschaft
+ Aufgabe 3: Erstellen eines Azure Active Directory-Mandanten (Azure AD) (Optional: Problem mit der Laborumgebung)
+ Aufgabe 4: Verwalten von Azure AD-Gastbenutzer*innen (Optional: Problem mit der Laborumgebung)

## <a name="estimated-timing-30-minutes"></a>Geschätzte Zeit: 30 Minuten

## <a name="architecture-diagram"></a>Architekturdiagramm
![image](../media/lab01.png)

## <a name="instructions"></a>Anweisungen

### <a name="exercise-1"></a>Übung 1

#### <a name="task-1-create-and-configure-azure-ad-users"></a>Aufgabe 1: Erstellen und Konfigurieren von Azure AD-Benutzern

In dieser Aufgabe erstellen und konfigurieren Sie Azure AD-Benutzer.

>**Hinweis**: Wenn Sie zuvor die Testlizenz für Azure AD Premium auf diesem Azure AD-Mandanten verwendet haben, benötigen Sie einen neuen Azure AD-Mandanten und führen die Aufgabe 2 nach Aufgabe 3 in diesem neuen Azure AD-Mandanten aus.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Suchen Sie im Azure-Portal nach **Azure Active Directory**, und wählen Sie es aus.

1. Scrollen Sie auf dem Blatt „Azure Active Directory“ nach unten zum Abschnitt **Verwalten**, klicken Sie auf **Benutzereinstellungen**, und überprüfen Sie die verfügbaren Konfigurationsoptionen.

1. Klicken Sie auf dem Blatt „Azure Active Directory“ im Abschnitt **Verwalten** auf **Benutzer** und dann auf Ihr Benutzerkonto, um die Einstellungen für das zugehörige **Profil** anzuzeigen. 

1. Klicken Sie auf **Bearbeiten**, und legen Sie im Abschnitt **Einstellungen** den **Nutzungsstandort** auf **USA** fest. Klicken Sie dann auf **Speichern**, um die Änderung zu übernehmen.

    >**Hinweis**: Diese Änderung ist erforderlich, um Ihrem Benutzerkonto später in dieser Übung eine Azure AD Premium P2-Lizenz zuzuweisen.

1. Navigieren Sie zurück zum Blatt **Benutzer – Alle Benutzer**, und klicken Sie dann auf **+ Neuer Benutzer**.

1. Erstellen Sie einen neuen Benutzer mit den folgenden Einstellungen (übernehmen Sie für andere Einstellungen die Standardwerte):

    | Einstellung | Wert |
    | --- | --- |
    | Benutzername | **az104-01a-aaduser1** |
    | Name | **az104-01a-aaduser1** |
    | Kennwort selbst erstellen | enabled |
    | Erstes Kennwort | **Bereitstellen eines sicheren Kennworts** |
    | Verwendungsstandort | **USA** |
    | Berufsbezeichnung | **Cloudadministrator** |
    | Department | **IT** |

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: <bpt id="p2">**</bpt>Copy to clipboard<ept id="p2">**</ept> the full <bpt id="p3">**</bpt>User Principal Name<ept id="p3">**</ept> (user name plus domain). You will need it later in this task.

1. Klicken Sie in der Liste der Benutzer auf das neu erstellte Benutzerkonto, um das zugehörige Blatt anzuzeigen.

1. Überprüfen Sie die im Abschnitt **Verwalten** verfügbaren Optionen. Beachten Sie, dass Sie die dem Benutzerkonto zugewiesenen Azure AD-Rollen sowie die Berechtigungen des Benutzerkontos für Azure-Ressourcen identifizieren können.

1. Klicken Sie im Abschnitt **Verwalten** auf **Zugewiesene Rollen** und dann auf die Schaltfläche **+ Zuweisung hinzufügen**. Weisen Sie **az104-01a-aaduser1** die Rolle **Benutzeradministrator** zu.

    >**Hinweis**: Sie können Azure AD-Rollen auch bei der Bereitstellung eines neuen Benutzers zuweisen.

1. Open an <bpt id="p1">**</bpt>InPrivate<ept id="p1">**</ept> browser window and sign in to the <bpt id="p2">[</bpt>Azure portal<ept id="p2">](https://portal.azure.com)</ept> using the newly created user account. When prompted to update the password, change the password to a secure password of your choosing. 

    >**Hinweis**: Anstatt den Benutzernamen (einschließlich des Domänennamens) einzugeben, können Sie den Inhalt der Zwischenablage einfügen.

1. Klicken Sie im Azure-Portal im **InPrivate**-Browserfenster auf **Azure Active Directory**.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: While this user account can access the Azure Active Directory tenant, it does not have any access to Azure resources. This is expected, since such access would need to be granted explicitly by using Azure Role-Based Access Control. 

1. Scrollen Sie im **InPrivate**-Browserfenster auf dem Azure AD-Blatt nach unten zum Abschnitt **Verwalten**, und klicken Sie auf **Benutzereinstellungen**. Beachten Sie, dass Sie über keinerlei Berechtigungen zum Ändern von Konfigurationsoptionen verfügen.

1. Scrollen Sie im **InPrivate**-Browserfenster auf dem Azure AD-Blatt nach unten zum Abschnitt **Verwalten**, klicken Sie auf **Benutzer** und dann auf **+Neuer Benutzer**.

1. Erstellen Sie einen neuen Benutzer mit den folgenden Einstellungen (übernehmen Sie für andere Einstellungen die Standardwerte):

    | Einstellung | Wert |
    | --- | --- |
    | Benutzername | **az104-01a-aaduser2** |
    | Name | **az104-01a-aaduser2** |
    | Kennwort selbst erstellen | enabled |
    | Erstes Kennwort | **Bereitstellen eines sicheren Kennworts** |
    | Verwendungsstandort | **USA** |
    | Berufsbezeichnung | **Systemadministrator** |
    | Department | **IT** |

1. Melden Sie sich als Benutzer „az104-01a-aaduser1“ über das Azure-Portal an, und schließen Sie das InPrivate-Browserfenster.

#### <a name="task-2-create-azure-ad-groups-with-assigned-and-dynamic-membership"></a>Aufgabe 2: Erstellen von Azure AD-Gruppen mit zugewiesener und dynamischer Mitgliedschaft

In dieser Aufgabe erstellen Sie Azure Active Directory-Gruppen mit zugewiesener und dynamischer Mitgliedschaft.

1. Navigieren Sie im Azure-Portal, in dem Sie mit Ihrem **Benutzerkonto** angemeldet sind, zurück zum Blatt **Übersicht** für den Azure AD-Mandanten. Klicken Sie dann im Abschnitt **Verwalten** auf **Lizenzen**.

    >**Hinweis**: Für die Implementierung dynamischer Gruppen werden Azure AD Premium P1- oder P2-Lizenzen benötigt.

1. Klicken Sie im Bereich **Verwalten** auf **Alle Produkte**.

1. Klicken Sie auf **+ Testen/Kaufen**, und aktivieren Sie die kostenlose Testversion von Azure AD Premium P2.

1. Aktualisieren Sie das Browserfenster, um zu überprüfen, ob die Aktivierung erfolgreich war. 

 ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: It can take up to 10 minutes for the licenses to activate. Continue refreshing the page until it appears. Do not proceed until the licenses have been activated.

1. Wählen Sie auf dem Blatt **Lizenzen – Alle Produkte** den Eintrag **Azure Active Directory Premium P2**, und weisen Sie Ihrem Benutzerkonto und den beiden neu erstellten Benutzerkonten alle Lizenzoptionen von Azure AD Premium P2 zu.

1. Navigieren Sie im Azure-Portal zurück zum Blatt des Azure AD-Mandanten, und klicken Sie auf **Gruppen**.

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

1. Um Contoso-Benutzern die Authentifizierung mit Azure AD zu ermöglichen, wurden Sie mit der Bereitstellung von Benutzer- und Gruppenkonten beauftragt. 

1. Klicken Sie zurück auf dem Blatt **Gruppen – Alle Gruppen** des Azure AD-Mandanten auf die Schaltfläche **+ Neue Gruppe**, und erstellen Sie eine neue Gruppe mit den folgenden Einstellungen:

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

1. Die Mitgliedschaft in den Gruppen soll automatisch auf Grundlage der Tätigkeitsbezeichnungen der Benutzer aktualisiert werden. 

1. Klicken Sie zurück auf dem Blatt **Gruppen – Alle Gruppen** des Azure AD-Mandanten auf die Schaltfläche **+ Neue Gruppe**, und erstellen Sie eine neue Gruppe mit den folgenden Einstellungen:

    | Einstellung | Wert |
    | --- | --- |
    | Gruppentyp | **Sicherheit** |
    | Gruppenname | **IT-Lab-Administratoren** |
    | Gruppenbeschreibung | **IT-Lab-Administratoren von Contoso** |
    | Mitgliedschaftstyp | **Zugewiesen** |
    
1. Klicken Sie auf **Keine Mitglieder ausgewählt**.

1. Suchen Sie auf dem Blatt **Mitglieder hinzufügen** die Gruppen **IT-Cloudadministratoren** und **IT-Systemadministratoren**, und klicken Sie dann auf dem Blatt **Neue Gruppe** auf **Erstellen**.

1. Sie müssen außerdem einen Azure AD-Testmandanten mit einem Testbenutzerkonto erstellen und diesem Konto eingeschränkte Berechtigungen für Ressourcen im Contoso-Azure-Abonnement gewähren.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: You might experience delays with updates of the dynamic membership groups. To expedite the update, navigate to the group blade, display its <bpt id="p1">**</bpt>Dynamic membership rules<ept id="p1">**</ept> blade, <bpt id="p2">**</bpt>Edit<ept id="p2">**</ept> the rule listed in the <bpt id="p3">**</bpt>Rule syntax<ept id="p3">**</ept> textbox by adding a whitespace at the end, and <bpt id="p4">**</bpt>Save<ept id="p4">**</ept> the change.

1. Navigate back to the <bpt id="p1">**</bpt>Groups - All groups<ept id="p1">**</ept> blade, click the entry representing the <bpt id="p2">**</bpt>IT System Administrators<ept id="p2">**</ept> group and, on then display its <bpt id="p3">**</bpt>Members<ept id="p3">**</ept> blade. Verify that the <bpt id="p1">**</bpt>az104-01a-aaduser2<ept id="p1">**</ept> appears in the list of group members.

#### <a name="task-3-create-an-azure-active-directory-ad-tenant-optional---lab-environment-issue"></a>Aufgabe 3: Erstellen eines Azure Active Directory-Mandanten (Azure AD) (Optional: Problem mit Laborumgebung)

In dieser Aufgabe erstellen Sie einen neuen Azure AD-Mandanten.

   ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: There is a known issue with the Captcha verification in the lab environment. If you experience this issue, please skip both this task and the next. We are working on a solution.

1. Suchen Sie im Azure-Portal nach **Azure Active Directory**, und wählen Sie es aus.

1. Klicken Sie auf **Mandanten verwalten**, dann im nächsten Bildschirm auf **+ Erstellen**, und geben Sie die folgende Einstellung an:

    | Einstellung | Wert |
    | --- | --- |
    | Verzeichnistyp | **Azure Active Directory** |
    
1. Klicken Sie auf **Weiter: Konfiguration**.

    | Einstellung | Wert |
    | --- | --- |
    | Name der Organisation | **Contoso-Lab** |
    | Name der Anfangsdomäne | Ein beliebiger gültiger DNS-Name, der aus Kleinbuchstaben und Ziffern besteht und mit einem Buchstaben beginnt. | 
    | Land/Region | **USA** |

   > <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: The <bpt id="p2">**</bpt>Initial domain name<ept id="p2">**</ept> should not be a legitimate name that potentially matches your organization or another. The green check mark in the <bpt id="p1">**</bpt>Initial domain name<ept id="p1">**</ept> text box will indicate that the domain name you typed in is valid and unique.

1. Klicken Sie auf **Überprüfen + erstellen** und dann auf **Erstellen**.

1. Zeigen Sie das Blatt des neu erstellten Azure AD-Mandanten an, indem Sie auf den Link **Klicken Sie hier, um zu Ihrem neuen Mandanten zu navigieren: Contoso-Lab** oder auf die Schaltfläche **Verzeichnis + Abonnement** (direkt rechts neben der Schaltfläche „Cloud Shell“) in der Symbolleiste des Azure-Portals klicken.

#### <a name="task-4-manage-azure-ad-guest-users"></a>Aufgabe 4: Verwalten von Azure AD-Gastbenutzern

In dieser Aufgabe erstellen Sie Azure AD-Gastbenutzer und gewähren ihnen Zugriff auf Ressourcen in einem Azure-Abonnement.

1. Klicken Sie im Azure-Portal, das den Azure AD-Mandanten „Contoso-Lab“ anzeigt, im Abschnitt **Verwalten** auf **Benutzer** und dann auf **+ Neuer Benutzer**.

1. Erstellen Sie einen neuen Benutzer mit den folgenden Einstellungen (übernehmen Sie für andere Einstellungen die Standardwerte):

    | Einstellung | Wert |
    | --- | --- |
    | Benutzername | **az104-01b-aaduser1** |
    | Name | **az104-01b-aaduser1** |
    | Kennwort selbst erstellen | enabled |
    | Erstes Kennwort | **Bereitstellen eines sicheren Kennworts** |
    | Berufsbezeichnung | **Systemadministrator** |
    | Department | **IT** |

1. Klicken Sie auf die das neu erstellte Profil.

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: <bpt id="p2">**</bpt>Copy to clipboard<ept id="p2">**</ept> the full <bpt id="p3">**</bpt>User Principal Name<ept id="p3">**</ept> (user name plus domain). You will need it later in this task.

1. Wechseln Sie zurück zu Ihrem Azure AD-Standardmandanten, indem Sie die Schaltfläche **Verzeichnis + Abonnement** (direkt rechts neben der Schaltfläche „Cloud Shell“) in der Symbolleiste des Azure-Portals verwenden.

1. Navigieren Sie zurück zum Blatt **Benutzer – Alle Benutzer**, und klicken Sie dann auf **+ Neuer Gastbenutzer**.

1. Laden Sie einen neuen Gastbenutzer mit den folgenden Einstellungen ein (übernehmen Sie für andere Einstellungen die Standardwerte):

    | Einstellung | Wert |
    | --- | --- |
    | Name | **az104-01b-aaduser1** |
    | E-Mail-Adresse | Der Benutzerprinzipalname, den Sie zu einem früheren Zeitpunkt in dieser Aufgabe kopiert haben |
    | Verwendungsstandort | **USA** |
    | Berufsbezeichnung | **Lab-Administrator** |
    | Department | **IT** |

1. Klicken Sie auf **Einladen**. 

1. Klicken Sie zurück auf dem Blatt **Benutzer – Alle Benutzer** auf den Eintrag, der das neu erstellte Gastbenutzerkonto repräsentiert.

1. Klicken Sie auf dem Blatt **az104-01b-aaduser1 – Profil** auf **Gruppen**.

1. Klicken Sie auf **+ Mitgliedschaft hinzufügen**, und fügen Sie das Gastbenutzerkonto der Gruppe **IT-Lab-Administratoren** hinzu.


#### <a name="task-5-clean-up-resources"></a>Aufgabe 5: Bereinigen von Ressourcen

> <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Remember to remove any newly created Azure resources that you no longer use. Removing unused resources ensures you will not incur unexpected costs. While, in this case, there are no additional charges associated with Azure Active Directory tenants and their objects, you might want to consider removing the user accounts, the group accounts, and the Azure Active Directory tenant you created in this lab.

 > <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>:  Don't worry if the lab resources cannot be immediately removed. Sometimes resources have dependencies and take a longer time to delete. It is a common Administrator task to monitor resource usage, so just periodically review your resources in the Portal to see how the cleanup is going. 

1. In the <bpt id="p1">**</bpt>Azure Portal<ept id="p1">**</ept> search for <bpt id="p2">**</bpt>Azure Active Directory<ept id="p2">**</ept> in the search bar. Within <bpt id="p1">**</bpt>Azure Active Directory<ept id="p1">**</ept> under <bpt id="p2">**</bpt>Manage<ept id="p2">**</ept> select <bpt id="p3">**</bpt>Licenses<ept id="p3">**</ept>. Once at <bpt id="p1">**</bpt>Licenses<ept id="p1">**</ept> under <bpt id="p2">**</bpt>Manage<ept id="p2">**</ept> select <bpt id="p3">**</bpt>All Products<ept id="p3">**</ept> and then select <bpt id="p4">**</bpt>Azure Active Directory Premium P2<ept id="p4">**</ept> item in the list. Proceed by then selecting <bpt id="p1">**</bpt>Licensed Users<ept id="p1">**</ept>. Select the user accounts <bpt id="p1">**</bpt>az104-01a-aaduser1<ept id="p1">**</ept> and <bpt id="p2">**</bpt>az104-01a-aaduser2<ept id="p2">**</ept> to which you assigned licenses in this lab, click <bpt id="p3">**</bpt>Remove license<ept id="p3">**</ept>, and, when prompted to confirm, click <bpt id="p4">**</bpt>Yes<ept id="p4">**</ept>.

1. Navigieren Sie im Azure-Portal zum Blatt **Benutzer – Alle Benutzer**, und klicken Sie auf den Eintrag, der das Gastbenutzerkonto **az104-01b-aaduser1** repräsentiert. Klicken Sie auf dem Blatt **az104-01b-aaduser1 – Profil** auf **Löschen**, und bestätigen Sie den Vorgang bei Aufforderung durch Klicken auf **OK**.

1. Wiederholen Sie diese Schrittfolge, um die übrigen Benutzerkonten zu löschen, die Sie in diesem Lab erstellt haben.

1. Navigieren Sie im Azure-Portal zum Blatt **Benutzer – Alle Benutzer**, und wählen Sie die Gruppen aus, die Sie in diesem Lab erstellt haben. Klicken Sie auf **Löschen**, und bestätigen Sie den Vorgang bei Aufforderung durch Klicken auf **OK**.

1. Zeigen Sie im Azure-Portal das Blatt des Azure AD-Mandanten „Contoso-Lab“ an, indem Sie die Schaltfläche **Verzeichnis + Abonnement** (direkt rechts neben der Schaltfläche „Cloud Shell“) in der Symbolleiste des Azure-Portals verwenden.

1. Navigieren Sie zum Blatt **Benutzer – Alle Benutzer**, und klicken Sie auf den Eintrag, der das Benutzerkonto **az104-01b-aaduser1** repräsentiert. Klicken Sie auf dem Blatt **az104-01b-aaduser1 – Profil** auf **Löschen**, und bestätigen Sie den Vorgang bei Aufforderung durch Klicken auf **OK**.

1. Navigate to the <bpt id="p1">**</bpt>Contoso Lab - Overview<ept id="p1">**</ept> blade of the Contoso Lab Azure AD tenant, click <bpt id="p2">**</bpt>Manage tenants<ept id="p2">**</ept> and then on the next screen, select the box next to <bpt id="p3">**</bpt>Contoso Lab<ept id="p3">**</ept>, click <bpt id="p4">**</bpt>Delete<ept id="p4">**</ept>, on the <bpt id="p5">**</bpt>Delete tenant 'Contoso Labs'?<ept id="p5">**</ept> blade, click the <bpt id="p1">**</bpt>Get permission to delete Azure resources<ept id="p1">**</ept> link, on the <bpt id="p2">**</bpt>Properties<ept id="p2">**</ept> blade of Azure Active Directory, set <bpt id="p3">**</bpt>Access management for Azure resources<ept id="p3">**</ept> to <bpt id="p4">**</bpt>Yes<ept id="p4">**</ept> and click <bpt id="p5">**</bpt>Save<ept id="p5">**</ept>.

1. Navigieren Sie zurück zum Blatt **Mandant „Contoso-Lab“ löschen**, und klicken Sie auf **Aktualisieren**, dann auf **Löschen**.

> <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: If a tenant has a trial license, then you would have to wait for the trial license expiration before you could delete the tenant. This would not incur any additional cost.

#### <a name="review"></a>Überprüfung

In diesem Lab haben Sie die folgenden Aufgaben ausgeführt:

- Erstellen und Konfigurieren von Azure AD-Benutzern
- Erstellen von Azure AD-Gruppen mit zugewiesener und dynamischer Mitgliedschaft
- Erstellen eines Azure AD-Mandanten
- Verwalten von Azure AD-Gastbenutzern 
