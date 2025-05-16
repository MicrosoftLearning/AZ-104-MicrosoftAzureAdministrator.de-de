---
lab:
  title: 'Lab 02b: Verwalten der Governance über Azure Policy'
  module: Administer Governance and Compliance
---

# Lab 02b – Verwalten der Governance über eine Azure-Richtlinie

## Einführung in das Lab

In diesem Lab erfahren Sie, wie Sie die Governancepläne Ihrer Organisation implementieren. Sie erfahren, wie Azure-Richtlinien sicherstellen können, dass operative Entscheidungen in der gesamten Organisation durchgesetzt werden. Sie erfahren, wie Sie das Taggen von Ressourcen verwenden, um die Berichterstellung zu verbessern. 

Für dieses Lab wird ein Azure-Abonnement benötigt. Ihr Abonnementtyp kann sich auf die Verfügbarkeit von Features in diesem Lab auswirken. Sie können die Region ändern, aber in den Schritten wird die Region **USA, Osten** verwendet. 

## Geschätzte Zeit: 30 Minuten

## Labszenario

Der Cloudspeicherbedarf Ihrer Organisation ist im letzten Jahr erheblich gewachsen. Während einer kürzlich durchgeführten Überprüfung haben Sie eine beträchtliche Anzahl von Ressourcen entdeckt, für die weder Besitzer, Projekt noch Kostenstelle definiert sind. Um die Verwaltung von Azure-Ressourcen in Ihrer Organisation zu verbessern, entscheiden Sie sich, die folgenden Funktionalitäten zu implementieren:

- Anwenden von Ressourcentags zum Anfügen wichtiger Metadaten an Azure-Ressourcen

- Erzwingen der Verwendung von Ressourcentags für neue Ressourcen mithilfe der Azure-Richtlinie

- Aktualisieren vorhandener Ressourcen mit Ressourcentags

- Verwenden von Ressourcensperren zum Schutz konfigurierter Ressourcen

## Interaktive Labsimulationen

Für dieses Thema stehen mehrere hilfreiche interaktive Labsimulationen zur Verfügung. In der Simulation können Sie sich in Ihrem eigenen Tempo durch ein ähnliches Szenario klicken. Es gibt zwar Unterschiede zwischen der interaktiven Simulation und diesem Lab, aber viele der Kernkonzepte sind identisch. Ein Azure-Abonnement ist nicht erforderlich. 

+ [Verwalten von Ressourcensperren](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2015). Fügen Sie eine Ressourcensperre hinzu, und testen Sie, um dies zu bestätigen.
  
+ [Erstellen einer Azure-Richtlinie](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2017). Erstellen Sie eine Azure-Richtlinie, welche den Speicherort einschränkt, in dem sich Ressourcen befinden können. Erstellen Sie eine neue Ressource, und stellen Sie sicher, dass die Richtlinie erzwungen wird. 

+ [Verwalten der Governance über eine Azure-Richtlinie](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%203). Erstellen Sie Tags über das Azure-Portal und weisen Sie diese zu. Erstellen Sie eine Azure-Richtlinie, die Taggen erfordert. Korrigieren Sie nicht konformer Ressourcen.

## Architekturdiagramm

![Diagramm der Aufgabenarchitektur.](../media/az104-lab02b-architecture.png)

## Stellenqualifikationen

+ Aufgabe 1: Erstellen und Zuweisen von Tags über das Azure-Portal.
+ Aufgabe 2: Erzwingen des Taggens mithilfe von Azure Policy.
+ Aufgabe 3: Anwenden des Taggens über Azure Policy.
+ Aufgabe 4: Konfigurieren und Testen von Ressourcensperren. 

## Aufgabe 1: Zuweisen von Tags über das Azure-Portal

In dieser Aufgabe erstellen Sie ein Tag und weisen es über das Azure-Portal einer Azure-Ressourcengruppe zu. Tags sind eine kritische Komponente einer Governancestrategie, wie im Microsoft Well-Architected Framework und im Cloud Adoption Framework beschrieben. Tags können es Ihnen ermöglichen, Ressourcenbesitzer, Verfallsdaten, Gruppenkontakte und andere Name/Wert-Paare, die Ihre Organisation für wichtig hält, schnell zu identifizieren. Für diesen Vorgang weisen Sie ein Tag zur Identifizierung der Ressourcenkostenstelle zu. 

1. Melden Sie sich beim **Azure-Portal** - `https://portal.azure.com` an.
      
1. Suchen Sie nach `Resource groups`, und wählen Sie diese Option aus.

1. Wählen Sie in den Ressourcengruppen **+ Erstellen** aus.

    | Einstellung | Wert |
    | --- | --- |
    | Abonnementname | Ihr Abonnement |
    | Ressourcengruppenname | `az104-rg2` |
    | Standort | **USA, Osten** |

    >**Hinweis:** Für jedes Lab in diesem Kurs werden Sie eine neue Ressourcengruppe erstellen. Auf diese Weise können Sie Ihre Labressourcen schnell suchen und verwalten. 

1. Wählen Sie **Weiter** und wechseln Sie zur Registerkarte **Tags**. Geben Sie Informationen für ein neues Tag ein.

    | Einstellung | Wert |
    | --- | --- |
    | Name | Kostenstelle |
    | Wert | 000 |

1. Klicken Sie auf **Review + Create** (Überprüfen und erstellen) und dann auf **Create** (Erstellen).

## Aufgabe 2: Erzwingen des Taggens mithilfe von Azure Policy

In dieser Aufgabe weisen Sie der Ressourcengruppe die integrierte Richtlinie *Tag und zugehöriger Wert für Ressourcen erforderlich* zu und werten das Ergebnis aus. Azure Policy kann verwendet werden, um die Konfiguration zu erzwingen, und in diesem Fall die Governance für Ihre Azure-Ressourcen. 

1. Suchen Sie im Azure-Portal nach `Policy` und wählen Sie es aus. 

1. Wählen Sie im Blatt **Dokumenterstellung** die Option **Definitionen** aus. Nehmen Sie sich einen Moment Zeit, um die Liste der [integrierten Richtliniendefinitionen](https://learn.microsoft.com/azure/governance/policy/samples/built-in-policies) zu durchsuchen, die Ihnen zur Verfügung stehen. Beachten Sie, dass Sie auch nach einer Definition suchen können.

    ![Screenshot der Richtliniendefinition.](../media/az104-lab02b-policytags.png)

1. Suchen Sie nach der integrierten Richtlinie `Require a tag and its value on resources`. Wählen Sie die Richtlinie aus und nehmen Sie sich eine Minute Zeit, um die Definition zu lesen. 

1. Wählen Sie **Richtlinie zuweisen** aus.

1. Geben Sie den **Bereich** an, indem Sie auf die Schaltfläche mit den Auslassungspunkte klicken, und wählen Sie die folgenden Werte aus. Klicken Sie auf **Auswählen**, wenn Sie fertig sind. 

    | Einstellung | Wert |
    | --- | --- |
    | Abonnement | *Ihr Abonnement* |
    | Ressourcengruppe | **az104-rg2** |

    >**Hinweis:** Sie können Richtlinien auf Ebene Verwaltungsgruppe, Abonnement oder Ressourcengruppe zuweisen. Sie haben außerdem die Möglichkeit, Ausschlüsse anzugeben, z. B. einzelne Abonnements, Ressourcengruppen oder Ressourcen. In diesem Szenario wollen wir das Tag für alle Ressourcen in der Ressourcengruppe hinzufügen.

1. Konfigurieren Sie auf dem Blatt **Grundlagen** die Eigenschaften der Zuweisung, indem Sie die folgenden Einstellungen angeben (übernehmen Sie für andere Einstellungen die Standardwerte):

    | Einstellung | Wert |
    | --- | --- |
    | Zuweisungsname | Kostenstellen-Tag und dessen Wert für Ressourcen erforderlich |
    | Beschreibung | `Require Cost Center tag and its value on all resources in the resource group`|
    | Durchsetzung von Richtlinien | Aktiviert |

    >**Hinweis**: Der **Zuweisungsname** wird automatisch mit dem ausgewählten Richtliniennamen aufgefüllt, kann aber geändert werden. Die Angabe einer **Beschreibung**ist optional. Beachten Sie, dass Sie die Richtlinie jederzeit deaktivieren können. 

1. Klicken Sie auf **Weiter**, und legen Sie die **Parameter** wie folgt fest:

    | Einstellung | Wert |
    | --- | --- |
    | Tagname | `Cost Center` |
    | Tagwert | `000` |

1. Klicken Sie auf **Weiter**, und sehen Sie sich die Registerkarte **Korrektur** an. Lassen Sie das Kontrollkästchen **Verwaltete Identität erstellen** deaktiviert. 

1. Klicken Sie auf **Überprüfen und erstellen** und dann auf **Erstellen**.

    >**Hinweis:** Jetzt werden Sie überprüfen, ob die neue Richtlinienzuweisung wirksam ist, indem Sie versuchen, ein Azure Storage-Konto in der Ressourcengruppe zu erstellen. Sie werden das Speicherkonto erstellen, ohne das erforderliche Tag hinzuzufügen. 
    
    >**Hinweis:** Es kann zwischen 5 und 10 Minuten dauern, bis die Richtlinie wirksam wird.

1. Suchen Sie im Portal nach `Storage Accounts` und wählen Sie es aus, und wählen Sie **+ Erstellen** aus. 

1. Füllen Sie auf der Registerkarte **Grundlagen** des Blatts **Speicherkonto erstellen** die Konfiguration vollständig aus.

    | Einstellung | Wert |
    | --- | --- |
    | Resource group | **az104-rg2** |
    | Speicherkontoname | *eine beliebige weltweit eindeutige Kombination aus 3 bis 24 Kleinbuchstaben und Ziffern, beginnend mit einem Buchstaben* |

1. Wählen Sie **Überprüfen** aus, und klicken Sie dann auf **Erstellen**.

1. Sie erhalten eine Meldung, dass die **Validierung** fehlgeschlagen ist. Sehen Sie sich die Meldung an, um den Grund für den Fehler zu ermitteln. Überprüfen Sie, ob die Fehlermeldung darauf hinweist, dass die Ressourcenbereitstellung von der Richtlinie nicht zugelassen wurde. 

    ![Screenshot des Richtlinienfehlers „unzulässig“.](../media/az104-lab02b-policyerror.png) 

>**Hinweis**: Wenn Sie auf die Registerkarte **Unformatierte Fehlermeldung** klicken, erhalten Sie weitere Informationen zum Fehler, einschließlich des Namens der Rollendefinition **Ein Tag und sein Wert auf Ressourcen erforderlich**. Die Bereitstellung war nicht erfolgreich, weil das zu Speicherkonto, das Sie zu erstellen versuchten, kein Tag mit dem Namen **Kostenstelle** mit dem auf **Standard** festgelegten Wert enthielt.

## Aufgabe 3: Anwenden des Taggings mithilfe einer Azure-Richtlinie

In dieser Aufgabe werden wir die neue Richtliniendefinition verwenden, um alle nicht konformen Ressourcen zu korrigieren. In diesem Szenario werden alle untergeordneten Ressourcen einer Ressourcengruppe das Tag **Kostenstelle** erben, das für die Ressourcengruppe definiert wurde.

1. Suchen Sie im Azure-Portal nach `Policy` und wählen Sie es aus. 

1. Klicken Sie im Abschnitt **Erstellung** auf **Zuweisungen**. 

1. Klicken Sie in der Liste der Zuweisungen auf das Symbol mit den Auslassungspunkten in der Zeile, die die Richtlinienzuweisung **Ein Tag und sein Wert auf Ressourcen erforderlich** darstellt und verwenden Sie den Menüpunkt **Zuweisung löschen**, um die Zuweisung zu löschen.

1. Klicken Sie auf **Richtlinie zuweisen**, und geben Sie den **Bereich** an, indem Sie auf die Schaltfläche mit den Auslassungspunkte klicken. Wählen Sie die folgenden Werte aus:

    | Einstellung | Wert |
    | --- | --- |
    | Subscription | Ihr Azure-Abonnement |
    | Ressourcengruppe | `az104-rg2` |

1. Um die **Richtliniendefinition**anzugeben, klicken Sie auf die Schaltfläche mit den Auslassungspunkten, und suchen Sie dann nach `Inherit a tag from the resource group if missing` und wählen Sie dies aus.

1. Wählen Sie **Hinzufügen** aus, und konfigurieren Sie dann die verbleibenden Eigenschaften **Grundlagen** der Zuordnung.

    | Einstellung | Wert |
    | --- | --- |
    | Zuweisungsname | `Inherit the Cost Center tag and its value 000 from the resource group if missing` |
    | Beschreibung | `Inherit the Cost Center tag and its value 000 from the resource group if missing` |
    | Durchsetzung von Richtlinien | Aktiviert |

1. Klicken Sie zweimal auf **Weiter**, und legen Sie die **Parameter** wie folgt fest:

    | Einstellung | Wert |
    | --- | --- |
    | Tag-Name | `Cost Center` |

1. Klicken Sie auf **Weiter** und konfigurieren Sie auf der Registerkarte **Korrektur** die folgenden Einstellungen (übernehmen Sie für andere Einstellungen die Standardwerte):

    | Einstellung | Wert |
    | --- | --- |
    | Erstellen eines Wartungstask | enabled |
    | Zu korrigierende Richtlinie | **Tag von der Ressourcengruppe erben, falls nicht vorhanden** |

    >**Hinweis**: Diese Richtliniendefinition umfasst die Auswirkung **Modify**. Daher ist eine verwaltete Identität erforderlich. 

    ![Screenshot der Seite „Richtlinienkorrektur“. ](../media/az104-lab02b-policyremediation.png) 

1. Klicken Sie auf **Überprüfen + erstellen** und dann auf **Erstellen**.

    >**Hinweis:** Um zu überprüfen, dass die neue Richtlinienzuweisung wirksam ist, werden Sie ein weiteres Azure Storage-Konto in derselben Ressourcengruppe erstellen, ohne das erforderliche Tag explizit hinzuzufügen. 
    
    >**Hinweis:** Es kann zwischen 5 und 10 Minuten dauern, bis die Richtlinie wirksam wird.

1. Suchen Sie nach `Storage Account` und wählen Sie es aus, und klicken Sie auf **+ Erstellen**. 

1. Wechseln Sie zum Blatt **Speicherkonto erstellen**, und überprüfen Sie anhand der Registerkarte **Grundlagen**, dass Sie die Ressourcengruppe verwenden, auf die die Richtlinie angewendet wurde. Geben Sie anschließend die folgenden Einstellungen an (übernehmen Sie für andere Einstellungen die Standardwerte), und klicken Sie auf **Überprüfen**:

    | Einstellung | Wert |
    | --- | --- |
    | Speicherkontoname | *eine beliebige weltweit eindeutige Kombination aus 3 bis 24 Kleinbuchstaben und Ziffern, beginnend mit einem Buchstaben* |

1. Vergewissern Sie sich, dass die Überprüfung dieses Mal erfolgreich war, und klicken Sie auf **Erstellen**.

1. Klicken Sie nach der Bereitstellung des neuen Speicherkontos auf **Zur Ressource wechseln**.

1. Beachten Sie auf dem Blatt **Tags**, dass das Tag **Kostenstelle** mit dem Wert **000** automatisch der Ressource zugewiesen wurde.

    >**Schon gewusst?** Wenn Sie im Portal nach **Tags** suchen und diese auswählen, können Sie die Ressourcen mit einem bestimmten Tag anzeigen. 

## Aufgabe 4: Konfigurieren und Testen von Ressourcensperren

In dieser Aufgabe konfigurieren und testen Sie eine Ressourcensperre. Sperren verhindern entweder Löschungen oder Änderungen einer Ressource. 

1. Suchen Sie nach ihrer Ressourcengruppe, und wählen Sie diese aus.
   
1. Wählen Sie auf dem Blatt **Einstellungen** die Option **Sperren** aus.

1. Wählen Sie **Hinzufügen** aus, und vervollständigen Sie die Informationen zur Ressourcensperre. Wenn Sie fertig sind, wählen Sie **Ok** aus. 

    | Einstellung | Wert |
    | --- | --- |
    | Sperrenname | `rg-lock` |
    | Sperrtyp | **löschen** (beachten Sie die Auswahl für schreibgeschützt) |
    
1. Navigieren Sie zum Blatt **Übersicht** der Ressourcengruppe, und wählen Sie **Ressourcengruppe löschen** aus.

1. Geben Sie im Textfeld **Name der Ressourcengruppe eingeben, um das Löschen zu bestätigen** den Ressourcengruppennamen, `az104-rg2`, ein. Beachten Sie, dass Sie den Ressourcengruppennamen kopieren und einfügen können. 

1. Beachten Sie die Warnung: Das Löschen dieser Ressourcengruppe und ihrer abhängigen Ressourcen ist eine dauerhafte Aktion und kann nicht rückgängig macht werden. Klicken Sie auf **Löschen**.

1. Sie sollten eine Benachrichtigung erhalten, die das Löschen verweigert. 

    ![Screenshot der Nachricht „Fehler beim Löschen“.](../media/az104-lab02b-failuretodelete.png) 

    >**Hinweis:** Sie müssen die Sperre entfernen, wenn Sie die Ressourcengruppe löschen möchten. 
    
## Bereinigen Ihrer Ressourcen

Wenn Sie mit **Ihrem eigenen Abonnement** arbeiten, nehmen Sie sich eine Minute Zeit, um die Labressourcen zu löschen. Dadurch wird sichergestellt, dass Ressourcen freigegeben und Kosten minimiert werden. Die einfachste Möglichkeit zum Löschen der Labressourcen besteht darin, die Ressourcengruppe des Labs zu löschen. 

+ Wählen Sie im Azure-Portal die Ressourcengruppe und dann **Ressourcengruppe löschen** aus, **geben Sie den Ressourcengruppennamen ein**, und klicken Sie dann auf **Löschen**.
+ Bei Verwendung von Azure PowerShell: `Remove-AzResourceGroup -Name resourceGroupName`.
+ Bei Verwendung der Befehlszeilenschnittstelle: `az group delete --name resourceGroupName`.

## Erweitern Ihrer Lernerfahrung mit Copilot
Copilot kann Sie beim Erlernen der Verwendung von Azure-Skripttools unterstützen. Copilot kann Sie auch in Bereichen unterstützen, die nicht im Lab behandelt werden oder in denen Sie weitere Informationen benötigen. Öffnen Sie einen Edge-Browser, und wählen Sie „Copilot“ (rechts oben) aus, oder navigieren Sie zu *copilot.microsoft.com*. Nehmen Sie sich einige Minuten Zeit, um diese Prompts auszuprobieren.
+ Welche Azure PowerShell- und CLI-Befehle sind zum Hinzufügen und Löschen von Ressourcensperren in einer Ressourcengruppe verfügbar?
+ Stelle Gemeinsamkeiten und Unterschiede zwischen Azure Policy und Azure RBAC in einer Tabelle dar.
+ Welche Schritte müssen Erzwingen von Azure Policy und zum Korrigieren von inkompatiblen Ressourcen ausgeführt werden?
+ Wie erhalte ich einen Bericht über Azure-Ressourcen mit bestimmten Tags?

## Weiterlernen im eigenen Tempo

+ [Entwerfen einer Governancestrategie für Unternehmen](https://learn.microsoft.com/training/modules/enterprise-governance/). Verwenden Sie RBAC und Azure Policy, um den Zugriff auf Ihre Azure-Lösungen einzuschränken, und bestimmen Sie, welche Methode für Ihre Sicherheitsziele die richtige ist.

## Wichtige Erkenntnisse

Herzlichen Glückwunsch zum erfolgreichen Abschluss des Labs. Hier sind die wichtigsten Erkenntnisse für dieses Lab. 

+ Azure-Tags sind Metadaten, die aus einem Schlüssel-Wert-Paar bestehen. Tags beschreiben eine bestimmte Ressource in Ihrer Umgebung. Insbesondere ermöglicht Ihnen das Tagging in Azure, Ihre Ressourcen auf logische Weise zu kennzeichnen.
+ Azure Policy richtet Konventionen für Ressourcen ein. Richtliniendefinitionen beschreiben Bedingungen für die Ressourcencompliance und die Maßnahmen, die ergriffen werden, wenn eine Bedingung erfüllt ist. Eine Bedingung vergleicht ein Feld oder einen Wert einer Ressourceneigenschaft mit einem erforderlichen Wert. Es gibt viele integrierte Richtliniendefinitionen, und Sie können die Richtlinien anpassen. 
+ Das Wartungsaufgabenfeature von Azure Policy wird verwendet, um Ressourcen basierend auf einer Definition und Zuweisen in die Konformität zu bringen. Ressourcen, die mit einer modify- oder deployIfNotExist-Definitionszuweisung nicht kompatibel sind, können mithilfe einer Wartungsaufgabe in die Konformität gebracht werden.
+ Sie können eine Ressourcensperre für ein Abonnement, eine Ressourcengruppe oder eine Ressource konfigurieren. Die Sperre kann eine Ressource vor versehentlichen Löschungen und Änderungen durch Benutzer schützen. Die Sperre setzt jegliche Benutzerberechtigungen außer Kraft.
+ Azure Policy ist die Sicherheitspraxis vor der Bereitstellung. RBAC und Ressourcensperren sind die Sicherheitspraxis nach der Bereitstellung.

