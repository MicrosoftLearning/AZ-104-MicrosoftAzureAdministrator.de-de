---
lab:
  title: 02b – Verwalten der Governance über eine Azure-Richtlinie
  module: Module 02 - Governance and Compliance
---

# <a name="lab-02b---manage-governance-via-azure-policy"></a>Lab 02b – Verwalten der Governance über eine Azure-Richtlinie
# <a name="student-lab-manual"></a>Lab-Handbuch für Kursteilnehmer

## <a name="lab-scenario"></a>Labszenario

Um die Verwaltung der Azure-Ressourcen von Contoso zu verbessern, wurden Sie mit der Implementierung der folgenden Funktionalität beauftragt:

- Tagging von Ressourcengruppen, die ausschließlich Infrastrukturressourcen enthalten (z. B. Cloud Shell-Speicherkonten)

- Sicherstellen, dass nur ordnungsgemäß gekennzeichnete Infrastrukturressourcen zu Infrastrukturressourcengruppen hinzugefügt werden können

- Korrigieren nicht konformer Ressourcen 

## <a name="objectives"></a>Ziele

In diesem Lab werden folgende Aufgaben ausgeführt:

+ Aufgabe 1: Erstellen und Zuweisen von Tags über das Azure-Portal
+ Aufgabe 2: Erzwingen des Taggings mithilfe einer Azure-Richtlinie
+ Aufgabe 3: Anwenden des Taggings mithilfe einer Azure-Richtlinie

## <a name="estimated-timing-30-minutes"></a>Geschätzte Zeit: 30 Minuten

## <a name="architecture-diagram"></a>Architekturdiagramm

![image](../media/lab02b.png)

## <a name="instructions"></a>Anweisungen

### <a name="exercise-1"></a>Übung 1

#### <a name="task-1-assign-tags-via-the-azure-portal"></a>Aufgabe 1: Zuweisen von Tags über das Azure-Portal

In dieser Aufgabe erstellen Sie ein Tag und weisen es über das Azure-Portal einer Azure-Ressourcengruppe zu.

1. Starten Sie im Azure-Portal im Bereich **Cloud Shell** eine **PowerShell**-Sitzung.

    >**Hinweis**: Wenn Sie **Cloud Shell** zum ersten Mal starten und die Meldung **Für Sie wurde kein Speicher bereitgestellt** angezeigt wird, wählen Sie das in diesem Lab verwendete Abonnement aus, und klicken Sie dann auf **Speicher erstellen**. 

1. Führen Sie in Cloud Shell folgenden Befehl aus, um den Namen des von Cloud Shell verwendeten Speicherkontos zu identifizieren:

   ```powershell
   df
   ```

1. Beachten Sie in der Ausgabe des Befehls den ersten Teil des vollqualifizierten Pfads, der das Cloud Shell-Basislaufwerk angibt (hier markiert als `xxxxxxxxxxxxxx`):

   ```
   //xxxxxxxxxxxxxx.file.core.windows.net/cloudshell   (..)  /usr/csuser/clouddrive
   ```

1. Suchen Sie im Azure-Portal nach der Option **Speicherkonten**, und wählen Sie sie aus. Klicken Sie anschließend in der Speicherkontenliste auf den Eintrag, der das Speicherkonto repräsentiert, das Sie im vorherigen Schritt identifiziert haben.

1. Klicken Sie auf dem Blatt „Speicherkonto“ auf den Link, der den Namen der Ressourcengruppe repräsentiert, die das Speicherkonto enthält.

    **Hinweis**: Notieren Sie sich den Namen der Ressourcengruppe, in der sich das Speicherkonto befindet. Sie benötigen den Namen später im Lab.

1. Klicken Sie auf dem Blatt „Ressourcengruppe“ neben **Tags** auf **Bearbeiten**, um neue Tags zu erstellen.

1. Erstellen Sie ein Tag mit den folgenden Einstellungen, und wenden Sie Ihre Änderung an:

    | Einstellung | Wert |
    | --- | --- |
    | Name | **Rolle** |
    | Wert | **Infra** |

1. Navigate back to the storage account blade. Review the <bpt id="p1">**</bpt>Overview<ept id="p1">**</ept> information and note that the new tag was not automatically assigned to the storage account. 

#### <a name="task-2-enforce-tagging-via-an-azure-policy"></a>Aufgabe 2: Erzwingen des Taggings mithilfe einer Azure-Richtlinie

In dieser Aufgabe weisen Sie der Ressourcengruppe die integrierte Richtlinie *Tag und zugehöriger Wert für Ressourcen erforderlich* zu und werten das Ergebnis aus. 

1. Suchen Sie im Azure-Portal nach der Option **Richtlinie**, und wählen Sie sie aus. 

1. In the <bpt id="p1">**</bpt>Authoring<ept id="p1">**</ept> section, click <bpt id="p2">**</bpt>Definitions<ept id="p2">**</ept>. Take a moment to browse through the list of built-in policy definitions that are available for you to use. List all built-in policies that involve the use of tags by selecting the <bpt id="p1">**</bpt>Tags<ept id="p1">**</ept> entry (and de-selecting all other entries) in the <bpt id="p2">**</bpt>Category<ept id="p2">**</ept> drop-down list. 

1. Klicken Sie auf den Eintrag, der die integrierte Richtlinie **Tag und zugehöriger Wert für Ressourcen erforderlich** repräsentiert, und überprüfen Sie die zugehörige Definition.

1. Klicken Sie auf dem Blatt für die Definition der integrierten Richtlinie **Tag und zugehöriger Wert für Ressourcen erforderlich** auf **Zuweisen**.

1. Geben Sie den **Bereich** an, indem Sie auf die Schaltfläche mit den Auslassungspunkte klicken, und wählen Sie die folgenden Werte aus:

    | Einstellung | Wert |
    | --- | --- |
    | Subscription | Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden. |
    | Ressourcengruppe | Der Name der Ressourcengruppe, in der sich das Cloud Shell-Konto befindet, das Sie in der vorherigen Aufgabe identifiziert haben. |

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: A scope determines the resources or resource groups where the policy assignment takes effect. You could assign policies on the management group, subscription, or resource group level. You also have the option of specifying exclusions, such as individual subscriptions, resource groups, or resources (depending on the assignment scope). 

1. Konfigurieren Sie auf dem Blatt **Grundlagen** die Eigenschaften der Zuweisung, indem Sie die folgenden Einstellungen angeben (übernehmen Sie für andere Einstellungen die Standardwerte):

    | Einstellung | Wert |
    | --- | --- |
    | Zuweisungsname | **Tag „Role“ mit Wert „Infra“ erforderlich**|
    | BESCHREIBUNG | **Anforderung eines Tags „Role“ mit dem Wert „Infra“ für alle Ressourcen in der Cloud Shell-Ressourcengruppe**|
    | Durchsetzung von Richtlinien | Aktiviert |

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: The <bpt id="p2">**</bpt>Assignment name<ept id="p2">**</ept> is automatically populated with the policy name you selected, but you can change it. You can also add an optional <bpt id="p1">**</bpt>Description<ept id="p1">**</ept>. <bpt id="p1">**</bpt>Assigned by<ept id="p1">**</ept> is automatically populated based on the user name creating the assignment. 

1. Klicken Sie auf **Weiter**, und legen Sie die **Parameter** wie folgt fest:

    | Einstellung | Wert |
    | --- | --- |
    | Tag-Name | **Rolle** |
    | Tagwert | **Infra** |

1. Klicken Sie auf **Weiter**, und sehen Sie sich die Registerkarte **Korrektur** an. Lassen Sie das Kontrollkästchen **Verwaltete Identität erstellen** deaktiviert. 

    >**Hinweis**: Diese Einstellung kann verwendet werden, wenn die Richtlinie oder Initiative die Auswirkung **deployIfNotExists** oder **Modify** enthält.

1. Klicken Sie auf **Überprüfen + erstellen** und dann auf **Erstellen**.

    >**Hinweis**: Um sicherzustellen, dass die neue Richtlinienzuweisung wirksam ist, erstellen Sie nun ein weiteres Azure Storage-Konto in derselben Ressourcengruppe, ohne das erforderliche Tag explizit hinzuzufügen. 
    
    >**Hinweis**: Es kann zwischen 5 und 15 Minuten dauern, bis die Richtlinie wirksam wird.

1. Navigieren Sie zurück zum Blatt der Ressourcengruppe, die das Speicherkonto für das Cloud Shell-Basislaufwerk hostet, das Sie in der vorherigen Aufgabe identifiziert haben.

1. Klicken Sie auf dem Blatt „Ressourcengruppe“ auf **+ Erstellen**. Suchen Sie nach **Speicherkonto**, und klicken Sie auf **+ Erstellen**. 

1. Wechseln Sie zum Blatt **Speicherkonto erstellen**, und überprüfen Sie anhand der Registerkarte **Grundlagen**, dass Sie die Ressourcengruppe verwenden, auf die die Richtlinie angewendet wurde. Geben Sie anschließend die folgenden Einstellungen an (übernehmen Sie für andere Einstellungen die Standardwerte), klicken Sie auf **Überprüfen + erstellen** und dann auf **Erstellen**:

    | Einstellung | Wert |
    | --- | --- |
    | Speicherkontoname | Eine beliebige weltweit eindeutige Kombination aus 3 bis 24 Kleinbuchstaben und Ziffern, beginnend mit einem Buchstaben. |

1. Once you create the deployment, you should see the <bpt id="p1">**</bpt>Deployment failed<ept id="p1">**</ept> message in the <bpt id="p2">**</bpt>Notifications<ept id="p2">**</ept> list of the portal. From the <bpt id="p1">**</bpt>Notifications<ept id="p1">**</ept> list, navigate to the deployment overview and click the <bpt id="p2">**</bpt>Deployment failed. Click here for details<ept id="p2">**</ept> message to identify the reason for the failure. 

    >**Hinweis**: Überprüfen Sie, ob die Fehlermeldung darauf hinweist, dass die Ressourcenbereitstellung aufgrund einer Richtlinie nicht zugelassen wurde. 

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: By clicking the <bpt id="p2">**</bpt>Raw Error<ept id="p2">**</ept> tab, you can find more details about the error, including the name of the role definition <bpt id="p3">**</bpt>Require Role tag with Infra value<ept id="p3">**</ept>. The deployment failed because the storage account you attempted to create did not have a tag named <bpt id="p1">**</bpt>Role<ept id="p1">**</ept> with its value set to <bpt id="p2">**</bpt>Infra<ept id="p2">**</ept>.

#### <a name="task-3-apply-tagging-via-an-azure-policy"></a>Aufgabe 3: Anwenden des Taggings mithilfe einer Azure-Richtlinie

In dieser Aufgabe wird eine andere Richtliniendefinition verwendet, um alle nicht konformen Ressourcen zu korrigieren. 

1. Suchen Sie im Azure-Portal nach der Option **Richtlinie**, und wählen Sie sie aus. 

1. Klicken Sie im Abschnitt **Erstellung** auf **Zuweisungen**. 

1. Klicken Sie in der Liste der Zuweisungen auf das Symbol mit den Auslassungspunkten in der Zeile, die die Richtlinienzuweisung **Tag „Role“ mit Wert „Infra“ erforderlich** darstellt. Verwenden Sie den Menüpunkt **Zuweisung löschen**, um die Zuweisung zu löschen.

1. Klicken Sie auf **Richtlinie zuweisen**, und geben Sie den **Bereich** an, indem Sie auf die Schaltfläche mit den Auslassungspunkte klicken. Wählen Sie die folgenden Werte aus:

    | Einstellung | Wert |
    | --- | --- |
    | Subscription | Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden. |
    | Ressourcengruppe | Der Name der Ressourcengruppe, in der sich das Cloud Shell-Konto befindet, das Sie in der ersten Aufgabe identifiziert haben. |

1. Um die **Richtliniendefinition** festzulegen, klicken Sie auf die Schaltfläche mit den Auslassungspunkten und wählen dann **Tag von der Ressourcengruppe erben, falls nicht vorhanden** aus.

1. Konfigurieren Sie auf dem Blatt **Grundlagen** die übrigen Eigenschaften der Zuweisung, indem Sie die folgenden Einstellungen angeben (übernehmen Sie für andere Einstellungen die Standardwerte):

    | Einstellung | Wert |
    | --- | --- |
    | Zuweisungsname | **Role-Tag und zugehörigen Infra-Wert von der Cloud Shell-Ressourcengruppe erben, falls nicht vorhanden**|
    | BESCHREIBUNG | **Role-Tag und zugehörigen Infra-Wert von der Cloud Shell-Ressourcengruppe erben, falls nicht vorhanden**|
    | Durchsetzung von Richtlinien | Aktiviert |

1. Klicken Sie auf **Weiter**, und legen Sie die **Parameter** wie folgt fest:

    | Einstellung | Wert |
    | --- | --- |
    | Tag-Name | **Rolle** |

1. Klicken Sie auf **Weiter** und konfigurieren Sie auf der Registerkarte **Korrektur** die folgenden Einstellungen (übernehmen Sie für andere Einstellungen die Standardwerte):

    | Einstellung | Wert |
    | --- | --- |
    | Erstellen eines Wartungstask | enabled |
    | Zu korrigierende Richtlinie | **Tag vom Abonnement erben, falls nicht vorhanden** |

    >**Hinweis**: Diese Richtliniendefinition umfasst die Auswirkung **Modify**.

1. Klicken Sie auf **Überprüfen + erstellen** und dann auf **Erstellen**.

    >**Hinweis**: Um sicherzustellen, dass die neue Richtlinienzuweisung wirksam ist, erstellen Sie ein weiteres Azure Storage-Konto in derselben Ressourcengruppe, ohne das erforderliche Tag explizit hinzuzufügen. 
    
    >**Hinweis**: Es kann zwischen 5 und 15 Minuten dauern, bis die Richtlinie wirksam wird.

1. Navigieren Sie zurück zum Blatt der Ressourcengruppe, die das Speicherkonto für das Cloud Shell-Basislaufwerk hostet, das Sie in der ersten Aufgabe identifiziert haben.

1. Klicken Sie auf dem Blatt „Ressourcengruppe“ auf **+ Erstellen**. Suchen Sie nach **Speicherkonto**, und klicken Sie auf **+ Erstellen**. 

1. Wechseln Sie zum Blatt **Speicherkonto erstellen**, und überprüfen Sie anhand der Registerkarte **Grundlagen**, dass Sie die Ressourcengruppe verwenden, auf die die Richtlinie angewendet wurde. Geben Sie anschließend die folgenden Einstellungen an (übernehmen Sie für andere Einstellungen die Standardwerte), und klicken Sie auf **Überprüfen + erstellen**:

    | Einstellung | Wert |
    | --- | --- |
    | Speicherkontoname | Eine beliebige weltweit eindeutige Kombination aus 3 bis 24 Kleinbuchstaben und Ziffern, beginnend mit einem Buchstaben. |

1. Vergewissern Sie sich, dass die Überprüfung dieses Mal erfolgreich war, und klicken Sie auf **Erstellen**.

1. Klicken Sie nach der Bereitstellung des neuen Speicherkontos auf die Schaltfläche **Zur Ressource wechseln**. Beachten Sie auf dem Blatt **Übersicht** des neu erstellte Speicherkontos, dass der Ressource automatisch das Tag **Role** mit dem Wert **Infra** zugewiesen wurde.

#### <a name="task-4-clean-up-resources"></a>Aufgabe 4: Bereinigen der Ressourcen

   ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Remember to remove any newly created Azure resources that you no longer use. Removing unused resources ensures you will not see unexpected charges, although keep in mind that Azure policies do not incur extra cost.
   
   ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>:  Don't worry if the lab resources cannot be immediately removed. Sometimes resources have dependencies and take a longer time to delete. It is a common Administrator task to monitor resource usage, so just periodically review your resources in the Portal to see how the cleanup is going. 

1. Suchen Sie im Portal nach der Option **Richtlinie**, und wählen Sie sie aus.

1. Klicken Sie im Abschnitt **Erstellen** auf **Zuweisungen**. Klicken Sie auf das Symbol mit den Auslassungspunkten rechts neben der Zuweisung, die Sie in der vorherigen Aufgabe erstellt haben, und klicken Sie auf **Zuweisung löschen**. 

1. Suchen Sie im Portal nach der Option **Speicherkonten**, und wählen Sie sie aus.

1. In the list of storage accounts, select the resource group corresponding to the storage account you created in the last task of this lab. Select <bpt id="p1">**</bpt>Tags<ept id="p1">**</ept> and click <bpt id="p2">**</bpt>Delete<ept id="p2">**</ept> (Trash can to the right) to the <bpt id="p3">**</bpt>Role:Infra<ept id="p3">**</ept> tag and press <bpt id="p4">**</bpt>Apply<ept id="p4">**</ept>. 

1. Click <bpt id="p1">**</bpt>Overview<ept id="p1">**</ept> and click <bpt id="p2">**</bpt>Delete<ept id="p2">**</ept> on the top of the storage account blade. When prompted for the confirmation, in the <bpt id="p1">**</bpt>Delete storage account<ept id="p1">**</ept> blade, type the name of the storage account to confirm and click <bpt id="p2">**</bpt>Delete<ept id="p2">**</ept>. 

#### <a name="review"></a>Überprüfung

In diesem Lab haben Sie die folgenden Aufgaben ausgeführt:

- Erstellen und Zuweisen von Tags über das Azure-Portal
- Erzwingen des Taggings mithilfe einer Azure-Richtlinie
- Anwenden des Taggings mithilfe einer Azure-Richtlinie
