---
lab:
  title: 'Lab 06: Implementieren einer Netzwerkdatenverkehrs-Verwaltung'
  module: Administer Network Traffic Management
---

# Übung 06 - Implementieren einer Netzwerk- Datenverkehrsverwaltung

## Einführung in das Lab

In diesem Lab lernen Sie, wie Sie einen öffentlichen Load Balancer und ein Application Gateway konfigurieren und testen.

Für dieses Lab wird ein Azure-Abonnement benötigt. Ihr Abonnementtyp kann sich auf die Verfügbarkeit von Features in diesem Lab auswirken. Die Region kann geändert werden. In den Schritten wird allerdings die Region **USA, Osten** verwendet.

## Geschätzte Zeit: 50 Minuten

## Labszenario

Ihre Organisation verfügt über eine öffentliche Website. Sie müssen einen Lastenausgleich eingehender öffentlicher Anforderungen über verschiedene VMs vornehmen. Außerdem müssen Sie Bilder und Videos von verschiedenen VMs bereitstellen. Sie planen die Implementierung einer Azure Load Balancer-Instanz und eines Azure Application Gateway. Alle Ressourcen befinden sich in derselben Region.

## Stellenqualifikationen

+ Aufgabe 1: Verwenden einer Vorlage zum Bereitstellen einer Infrastruktur.
+ Aufgabe 2: Konfigurieren von Azure Load Balancer.
+ Aufgabe 3: Eine Azure Application Gateway-Instanz zu konfigurieren.

## Aufgabe 1: Bereitstellen einer Infrastruktur unter Verwendung einer Vorlage

In dieser Aufgabe werden Sie eine Vorlage verwenden, um ein virtuelles Netzwerk, eine Netzwerksicherheitsgruppe und drei Virtual Machines bereitzustellen.

1. Laden Sie die Lab-Dateien **\\Allfiles\\Lab06** (Vorlage und Parameter) herunter.

1. Melden Sie sich beim **Azure-Portal** - `https://portal.azure.com` an.

1. Suchen Sie nach `Deploy a custom template`, und wählen Sie diese Option aus.

1. Wählen Sie auf der Seite für die benutzerdefinierte Bereitstellung die Option **Eigene Vorlage im Editor erstellen** aus.

1. Wählen Sie auf der Seite „Vorlage bearbeiten“ die Option **Datei laden** aus.

1. Suchen Sie die Datei**\\Allfiles\\Lab06\\az104-06-vms-template.json** und wählen diese aus, und wählen Sie **Öffnen** aus.

1. Wählen Sie **Speichern**.

1. Wählen Sie **Parameter bearbeiten** aus, und laden Sie die Datei **\\Allfiles\\Lab06\\az104-06-vms-parameters.json**.

1. Wählen Sie **Speichern**.

1. Verwenden Sie die folgenden Informationen, um die Felder auf der Seite für die benutzerdefinierte Bereitstellung zu vervollständigen und alle anderen Felder auf dem Standardwert zu belassen.

    | Einstellung       | Wert         |
    | ---           | ---           |
    | Subscription  | Ihr Azure-Abonnement |
    | Resource group | `az104-rg6` (Wählen Sie bei Bedarf **Neu erstellen** aus) |
    | Kennwort      | Bereitstellen eines sicheren Kennworts |

    >**Hinweis:** Wenn Sie eine Fehlermeldung erhalten, dass die VM-Größe nicht verfügbar ist, wählen Sie eine SKU aus, die in Ihrem Abonnement verfügbar ist und mindestens 2 Kerne aufweist.

1. Wählen Sie **Überprüfen + erstellen** und anschließend **Erstellen** aus.

    >**Hinweis:** Warten Sie, bis die Bereitstellung abgeschlossen ist, bevor Sie mit der nächsten Aufgabe fortfahren. Die Bereitstellung sollte ungefähr 5 Minuten dauern.

    >**Hinweis:** Überprüfen Sie die Ressourcen, die bereitgestellt werden. Es wird ein virtuelles Netzwerk mit drei Subnetzen geben. Jedes Subnetz wird über eine VM verfügen.

## Aufgabe 2: Konfigurieren einer Azure Load Balancer-Instanz

In dieser Aufgabe implementieren Sie eine Azure Load Balancer-Instanz vor den beiden Azure-VMs im virtuellen Netzwerk. Load Balancer-Instanzen in Azure bieten Layer 4-Konnektivität über Ressourcen hinweg, z. B. VMs. Die Load Balancer-Konfiguration umfasst eine Front-End-IP-Adresse zum Akzeptieren von Verbindungen, einen Back-End-Pool und Regeln, die definieren, wie Verbindungen den Lastenausgleich durchlaufen sollen.

## Architekturdiagramm – Load Balancer

>**Hinweis:** Beachten Sie, dass die Load Balancer-Instanz auf zwei VMs im selben virtuellen Netzwerk verteilt.

![Diagramm der Aufgaben im Lab.](../media/az104-lab06-lb-architecture.png)

1. Suchen Sie im Azure-Portal nach `Load balancers` und wählen dies aus, und klicken Sie im Blatt **Lastenausgleichsmodule** auf **+ Erstellen**.

1. Erstellen Sie einen Lastenausgleich mit den folgenden Einstellungen (belassen Sie andere auf ihren Standardwerten), und klicken Sie dann auf **Weiter: Front-End-IP-Konfiguration**:

    | Einstellung | Wert |
    | --- | --- |
    | Subscription | Ihr Azure-Abonnement |
    | Resource group | **az104-rg6** |
    | Name | `az104-lb` |
    | Region | Die **gleiche** Region, in der Sie die VMs bereitgestellt haben |
    | SKU  | **Standard** |
    | Typ | **Public** |
    | Tarif | **Regional** |

     ![Screenshot der Seite „Lastenausgleich erstellen“.](../media/az104-lab06-create-lb1.png)

1. Klicken Sie auf der Registerkarte **Front-End-IP-Konfiguration** und dann auf **Front-End-IP-Konfiguration hinzufügen**. Wählen Sie die folgende Einstellung aus:  

    | Einstellung | Wert |
    | --- | --- |
    | Name | `az104-fe` |
    | IP-Typ | IP-Adresse |
    | Gateway Load Balancer | Keine |
    | Öffentliche IP-Adresse | Wählen Sie **Neu erstellen** aus (verwenden Sie die Anweisungen im nächsten Schritt) |

1. Verwenden Sie im Popupmenü **Öffentliche IP-Adresse hinzufügen** die folgenden Einstellungen, bevor Sie zweimal auf **Speichern** klicken. Wenn Sie fertig sind, klicken Sie auf **Weiter: Back-End-Pools**.

    | Einstellung | Wert |
    | --- | --- |
    | Name | `az104-lbpip` |
    | SKU | Standard |
    | Tarif | Länderspezifisch |
    | Zuweisung | statischen |
    | Routingpräferenz | **Microsoft Network** |

    >**Hinweis:** Die Standard-SKU stellt eine statische IP-Adresse bereit. Statische IP-Adressen werden mit der Ressource zugewiesen und freigegeben, wenn die Ressource gelöscht wird.  

1. Klicken Sie auf der Registerkarte **Back-End-Pools** auf **Back-End-Pool hinzufügen**, und geben Sie folgende Einstellungen an (übernehmen Sie für andere Einstellungen die Standardwerte). Klicken Sie auf **Hinzufügen** und dann auf **Speichern**. Klicken Sie auf **Weiter: Eingehende Regeln**.

    | Einstellung | Wert |
    | --- | --- |
    | Name | `az104-be` |
    | Virtuelles Netzwerk | **az104-06-vnet1** |
    | Konfiguration des Back-End-Pools | **NIC** |
    | Auf **Hinzufügen** klicken, um einen virtuellen Computer hinzuzufügen |  |
    | az104-06-vm0 | **Kontrollkästchen aktivieren** |
    | az104-06-vm1 | **Kontrollkästchen aktivieren** |

1. Wenn Sie Zeit haben, sehen Sie sich die anderen Registerkarten an, und klicken Sie dann auf **Überprüfen und erstellen**. Stellen Sie sicher, dass keine Überprüfungsfehler vorhanden sind, und klicken Sie dann auf **Erstellen**.

1. Warten Sie, bis der Lastenausgleich bereitgestellt wurde, und klicken Sie dann auf **Zu Ressource wechseln**.

**Hinzufügen einer Regel um zu bestimmen, wie eingehender Datenverkehr verteilt wird**

1. Wählen Sie Blatt **Einstellungen** die Option **Lastenausgleichsregeln** aus.

1. Wählen Sie **+ Hinzufügen**. Fügen Sie eine Lastenausgleichsregel mit den folgenden Einstellungen hinzu (übernehmen Sie für andere Einstellungen die Standardwerte).  Während Sie die Regel konfigurieren, verwenden Sie die Informationssymbole, um mehr über jede Einstellung zu erfahren. Klicken Sie abschließend auf **Speichern**.

    | Einstellung | Wert |
    | --- | --- |
    | Name | `az104-lbrule` |
    | IP-Version | **IPv4** |
    | Front-End-IP-Adresse | **az104-fe** |
    | Back-End-Pool | **az104-be** |
    | Protokoll | **TCP** |
    | Port | `80` |
    | Back-End-Port | `80` |
    | Integritätstest | **Neu erstellen** |
    | Name | `az104-hp` |
    | Protokoll | **TCP** |
    | Port | `80` |
    | Intervall | `5` |
    | Fenster zum Erstellen von Integritätstests schließen | **Speichern** |
    | Sitzungspersistenz | **None** |
    | Leerlaufzeitüberschreitung (Minuten) | `4` |
    | TCP-Zurücksetzung | **Disabled** |
    | Unverankerte IP | **Deaktiviert** |
    | Übersetzung der Quellnetzwerkadresse (SNAT) für ausgehenden Datenverkehr | **Empfohlen** |

1. Wählen Sie auf der Load Balancer-Seite die Option **Frontend-IP-Konfiguration** aus. Kopieren Sie die öffentliche IP-Adresse.

1. Öffnen Sie eine weitere Browserregisterkarte, und navigieren Sie zu der IP-Adresse. Vergewissern Sie sich, dass im Browserfenster die Meldung **Hello World from az104-06-vm0** oder **Hello World from az104-06-vm1** angezeigt wird.

1. Aktualisieren Sie das Fenster, um die Änderungen der Nachricht an den anderen virtuellen Computer zu überprüfen. Das zeigt, dass der Lastenausgleich durch die virtuellen Computer weitergeleitet wird.

    > **Hinweis**: Möglicherweise müssen Sie mehrmals aktualisieren oder ein neues Browserfenster im InPrivate-Modus öffnen.

## Aufgabe 3: Konfigurieren eines Azure Application Gateway

In dieser Aufgabe implementieren Sie ein Azure Application Gateway vor zwei Azure-VMs. Ein Application Gateway bietet Layer 7-Lastenausgleich, Web Application Firewall (WAF), SSL-Beendigung und End-to-End-Verschlüsselung zu den im Back-End-Pool definierten Ressourcen. Das Application Gateway leitet Bilder an eine VM und Videos an die andere VM weiter.

## Architekturdiagramm – Application Gateway

>**Hinweis:** Dieses Application Gateway arbeitet im selben virtuellen Netzwerk wie die Load Balancer-Instanz. Dies ist in einer Produktionsumgebung nicht unbedingt üblich.

![Diagramm der Aufgaben im Lab.](../media/az104-lab06-gw-architecture.png)

1. Suchen Sie im Azure-Portal nach `Virtual networks` und wählen es aus.

1. Wählen Sie auf dem Blatt **Virtuelle Netzwerke** in der Liste der virtuellen Netzwerke **az104-06-vnet1** aus.

1. Wählen Sie auf dem Blatt des virtuellen Netzwerks **az104-06-vnet1** im Abschnitt **Einstellungen** **Subnetze** und dann **+ Subnetz** aus.

1. Fügen Sie ein Subnetz mit den folgenden Einstellungen hinzu (belassen Sie andere auf ihren Standardwerten).

    | Einstellung | Wert |
    | --- | --- |
    | Name | `subnet-appgw` |
    | Startadresse| `10.60.3.224` |
    | Size | `/27` – Stellen Sie sicher, dass die **Startadresse** immer noch **10.60.3.224** lautet.|

1. Klicken Sie auf **Hinzufügen**.

    > **Hinweis:** Dieses Subnetz wird vom Azure Application Gateway verwendet werden. Das Application Gateway erfordert ein dediziertes Subnetz der Größe /27 oder höher.

1. Suchen Sie im Azure-Portal nach `Application gateways` und wählen es aus, und klicken Sie auf dem Blatt **Application Gateways** auf **+ Erstellen**.

1. Geben Sie auf der Registerkarte **Grundlagen** die folgenden Einstellungen an (übernehmen Sie für andere Einstellungen die Standardwerte):

    | Einstellung | Wert |
    | --- | --- |
    | Subscription | Ihr Azure-Abonnement |
    | Resource group | `az104-rg6` |
    | Name des Anwendungsgateways | `az104-appgw` |
    | Region | Die **gleiche** Azure-Region, die Sie in Aufgabe 1 verwendet haben |
    | Tarif | **Standard V2** |
    | Aktivieren der automatischen Skalierung | **Nein** |
    | Anzahl von Instanzen | `2` |
    | HTTP2 | **Disabled** |
    | Virtuelles Netzwerk | **az104-06-vnet1** |
    | Subnetz | **subnet-appgw (10.60.3.224/27)** |

1. Klicken Sie auf **Weiter: Front-Ends >** , und geben Sie die folgenden Einstellungen an (übernehmen Sie für andere Einstellungen die Standardwerte). Klicken Sie abschließend auf **OK**.

    | Einstellung | Wert |
    | --- | --- |
    | Typ der Front-End-IP-Adresse | **Public** |
    | Öffentliche IP-Adresse| **Neu hinzufügen** |
    | Name | `az104-gwpip` |
    | Verfügbarkeitszone | **1** |

    >**Hinweis:** Das Application Gateway kann sowohl eine öffentliche als auch eine private IP-Adresse haben.
 
1. Klicken Sie unten auf der Seite auf **Weiter: Back-Ends >** und dann auf **Back-End-Pool hinzufügen**. Geben Sie die folgenden Einstellungen an (übernehmen Sie für andere Einstellungen die Standardwerte). Wenn Sie fertig sind, klicken Sie auf **Hinzufügen**.

    | Einstellung | Wert |
    | --- | --- |
    | Name | `az104-appgwbe` |
    | Hinzufügen eines Back-End-Pools ohne Ziele | **Nein** |
    | Virtueller Computer | **az104-06-nic1 (10.60.1.4)** |
    | Virtueller Computer | **az104-06-nic2 (10.60.2.4)** |

1. Klicken Sie auf **Back-End-Pool hinzufügen**. Dies ist der Back-End-Pool für **Bilder**. Geben Sie die folgenden Einstellungen an (übernehmen Sie für andere Einstellungen die Standardwerte). Wenn Sie fertig sind, klicken Sie auf **Hinzufügen**.

    | Einstellung | Wert |
    | --- | --- |
    | Name | `az104-imagebe` |
    | Hinzufügen eines Back-End-Pools ohne Ziele | **Nein** |
    | Virtueller Computer | **az104-06-nic1 (10.60.1.4)** |

1. Klicken Sie auf **Back-End-Pool hinzufügen**. Dies ist der Back-End-Pool für **Video**. Geben Sie die folgenden Einstellungen an (übernehmen Sie für andere Einstellungen die Standardwerte). Wenn Sie fertig sind, klicken Sie auf **Hinzufügen**.

    | Einstellung | Wert |
    | --- | --- |
    | Name | `az104-videobe` |
    | Hinzufügen eines Back-End-Pools ohne Ziele | **Nein** |
    | Virtueller Computer | **az104-06-nic2 (10.60.2.4)** |

1. Klicken Sie auf **Weiter: Konfiguration >** und dann auf **Routingregel hinzufügen**. Vervollständigen Sie die Informationen.

    | Einstellung | Wert |
    | --- | --- |
    | Regelname | `az104-gwrule` |
    | Priority | `10` |
    | Name des Listeners | `az104-listener` |
    | Front-End-IP | **Öffentliche IPv4-Adresse** |
    | Protokoll | **HTTP** |
    | Port | `80` |
    | Listenertyp | **Grundlegend** |

1. Wechseln zur Registerkarte **Back-End-Ziele**. Wählen Sie **Hinzufügen** aus, nachdem Sie die grundlegenden Informationen vervollständigt haben.

   | Einstellung | Wert |
    | --- | --- |
    | Back-End-Ziel | `az104-appgwbe` |
    | Back-End-Einstellungen | `az104-http` (neu erstellen) |

   >**Hinweis:** Nehmen Sie sich eine Minute Zeit, um die Informationen über **Cookie-basierte Affinität** und **Verbindungsausgleich** zu lesen.

1. Wählen Sie im Abschnitt **Pfadbasiertes Routing** die Option **Fügen Sie mehrere Ziele zum Erstellen einer pfadbasierten Regel hinzu** aus. Sie werden zwei Regeln erstellen. Wählen Sie nach der ersten Regel auf **Hinzufügen**, und dann nach der zweiten Regel ebenfalls **Hinzufügen** aus. 

    **Regel – Routing an das Back-End für Bilder**

    | Einstellung | Wert |
    | --- | --- |
    | Pfad | `/image/*` |
    | Zielname | `images` |
    | Back-End-Einstellungen | **az104-http** |
    | Back-End-Ziel | `az104-imagebe` |

    **Regel – Routing an das Back-End für Videos**

    | Einstellung | Wert |
    | --- | --- |
    | Pfad | `/video/*` |
    | Zielname | `videos` |
    | Back-End-Einstellungen | **az104-http** |
    | Back-End-Ziel | `az104-videobe` |

1. Achten Sie darauf, Ihre Änderungen zu überprüfen und wählen Sie dann **Weiter: Tags >** aus. Es sind keine Änderungen erforderlich.

1. Klicken Sie auf **Weiter: Überprüfen + Erstellen >** und klicken Sie dann auf **Erstellen**.

    > **Hinweis**: Warten Sie, bis die Application Gateway-Instanz erstellt wurde. Dies wird ungefähr 5-10 Minuten dauern. Während Sie warten, sollten Sie einige der eigenverantwortlichen Trainingslinks am Ende dieser Seite überprüfen.

1. Suchen Sie nach der Bereitstellung des Anwendungsgateways nach **az104-appgw**, und wählen Sie es aus.

1. Wählen Sie in der Ressource **Application Gateway** im Abschnitt **Überwachung** die Option **Back-End-Integrität** aus.

1. Stellen Sie sicher, dass auf beiden Servern im Back-End-Pool **Fehlerfrei** angezeigt wird.

1. Kopieren Sie auf dem Blatt **Übersicht** den Wert der **öffentlichen Frontend-IP-Adresse**.

1. Starten Sie ein weiteres Browserfenster, und testen Sie diese URL – `http://<frontend ip address>/image/`.

1. Stellen Sie sicher, dass Sie an den Bildserver (vm1) weitergeleitet werden.

1. Starten Sie ein weiteres Browserfenster, und testen Sie diese URL – `http://<frontend ip address>/video/`.

1. Stellen Sie sicher, dass Sie zum Videoserver (vm2) weitergeleitet werden.

> **Hinweis**: Möglicherweise müssen Sie mehrmals aktualisieren oder ein neues Browserfenster im InPrivate-Modus öffnen.

## Bereinigen Ihrer Ressourcen

Wenn Sie mit **Ihrem eigenen Abonnement** arbeiten, nehmen Sie sich eine Minute Zeit, um die Labressourcen zu löschen. Dadurch wird sichergestellt, dass Ressourcen freigegeben und Kosten minimiert werden. Die einfachste Möglichkeit zum Löschen der Labressourcen besteht darin, die Ressourcengruppe des Labs zu löschen. 

+ Wählen Sie im Azure-Portal die Ressourcengruppe und dann **Ressourcengruppe löschen** aus, **geben Sie den Ressourcengruppennamen ein**, und klicken Sie dann auf **Löschen**.
+ Bei Verwendung von Azure PowerShell: `Remove-AzResourceGroup -Name resourceGroupName`.
+ Bei Verwendung der Befehlszeilenschnittstelle: `az group delete --name resourceGroupName`.

## Erweitern Ihrer Lernerfahrung mit Copilot

Copilot kann Sie beim Erlernen der Verwendung von Azure-Skripttools unterstützen. Copilot kann Sie auch in Bereichen unterstützen, die nicht im Lab behandelt werden oder in denen Sie weitere Informationen benötigen. Öffnen Sie einen Edge-Browser, und wählen Sie „Copilot“ (rechts oben) aus, oder navigieren Sie zu *copilot.microsoft.com*. Nehmen Sie sich einige Minuten Zeit, um diese Prompts auszuprobieren.

+ Zeige die Gemeinsamkeiten und Unterschiede zwischen Azure Load Balancer mit Azure Application Gateway an. Helfen Sie mir bei der Entscheidung, in welchen Szenarien ich ein bestimmtes Produkt verwenden sollte.
+ Welche Tools stehen zur Problembehandlung für Verbindungen mit einem Azure Load Balancer zur Verfügung? 
+ Welche grundlegenden Schritte müssen zum Konfigurieren von Azure Application Gateway ausgeführt werden? Stellen Sie eine allgemeine Checkliste bereit. 
+ Erstellen Sie eine Tabelle, in der drei Azure-Lösungen für den Lastenausgleich hervorgehoben werden. Für jede Lösung werden unterstützte Protokolle, Routingrichtlinien, Sitzungsaffinität und TLS-Auslagerung angezeigt.
  
## Weiterlernen im eigenen Tempo

+ [Verbessern der Skalierbarkeit und Resilienz von Anwendungen mithilfe von Azure Load Balancer](https://learn.microsoft.com/training/modules/improve-app-scalability-resiliency-with-load-balancer/). In diesem Modul werden die verschiedenen Lastenausgleichsmodule von Azure beschrieben und es wird erörtert, welche Azure Load Balancer-Lösung Ihre Anforderungen erfüllt.
+ [Vornehmen eines Lastenausgleichs für Ihren Webdienstdatenverkehr mit Application Gateway](https://learn.microsoft.com/training/modules/load-balance-web-traffic-with-application-gateway/). Verbessern Sie die Anwendungsresilienz durch Verteilung der Auslastung auf mehrere Server, und verwenden Sie das pfadbasierte Routing zum Leiten des Webdatenverkehrs.

## Wichtige Erkenntnisse

Herzlichen Glückwunsch zum erfolgreichen Abschluss des Labs. Dies sind die wichtigsten Punkte für dieses Lab.

+ Azure Load Balancer ist eine ausgezeichnete Wahl für die Verteilung von Netzwerkdatenverkehr auf mehrere VMs auf der Transportebene (OSI-Layer 4 – TCP und UDP).
+ Öffentliche Load Balancer-Instanzen werden verwendet, um einen Lastausgleich für den eingehenden Internetdatenverkehr Ihrer virtuellen Computer vorzunehmen. Ein interner (oder privater) Lastenausgleich wird verwendet, wenn private IP-Adressen nur am Front-End benötigt werden.
+ Der Basic-Lastenausgleich ist für Anwendungen mit geringer Größe vorgesehen, die keine Hochverfügbarkeit oder Redundanz benötigen. Der Standard-Lastenausgleich ist für hohe Leistung und extrem niedrige Wartezeit vorgesehen.
+ Azure Application Gateway ist ein Lastenausgleich für Webdatenverkehr auf Schicht 7, mit dem Sie eingehenden Datenverkehr für Ihre Webanwendungen verwalten können.
+ Die Ebene „Application Gateway Standard“ bietet alle L7-Funktionen, einschließlich Lastenausgleich, die WAF-Ebene fügt eine Firewall hinzu, um auf bösartigen Datenverkehr zu überprüfen.
+ Ein Application Gateway kann Routingentscheidungen basierend auf zusätzlichen Attributen einer HTTP-Anforderung treffen, beispielsweise dem URI-Pfad oder den Hostheadern.
