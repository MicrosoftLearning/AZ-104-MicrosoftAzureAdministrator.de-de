---
lab:
  title: 'Lab 04: Implementieren von virtuellen Netzwerken'
  module: Implement Virtual Networking
---

# Lab 04: Implementieren von virtuellen Netzwerken

## Einführung in das Lab

Dieses Lab ist das erste von drei Labs, die sich mit virtuellen Netzwerken beschäftigen. In diesem Lab lernen Sie die Grundlagen von virtuellen Netzwerken und Subnetzen kennen. Sie erfahren, wie Sie Ihr Netzwerk mit Netzwerksicherheitsgruppen und Anwendungssicherheitsgruppen schützen. Sie erfahren außerdem mehr über DNS-Zonen und -Einträge. 

Für dieses Lab wird ein Azure-Abonnement benötigt. Ihr Abonnementtyp kann sich auf die Verfügbarkeit von Features in diesem Lab auswirken. Die Region kann geändert werden. In den Schritten wird allerdings die Region **USA, Osten** verwendet.

## Geschätzte Dauer: 50 Minuten

## Labszenario 

Ihre globale Organisation plant die Implementierung virtueller Netzwerke. Das unmittelbare Ziel ist es, alle vorhandenen Ressourcen aufzunehmen. Die Organisation befindet sich jedoch in einer Wachstumsphase und möchte sicherstellen, dass zusätzliche Kapazitäten für das Wachstum vorhanden sind.

Das virtuelle Netzwerk **CoreServicesVnet** verfügt über die größte Anzahl von Ressourcen. Es wird ein großes Wachstum erwartet, daher ist ein großer Adressraum für dieses virtuelle Netzwerk erforderlich.

Das virtuelle Netzwerk **ManufacturingVnet** enthält Systeme für den Betrieb der Fertigungsanlagen. Die Organisation erwartet für Ihre Systeme eine große Anzahl interner verbundener Geräte zum Abrufen von Daten. 

## Interaktive Labsimulationen

Für dieses Thema stehen mehrere hilfreiche interaktive Labsimulationen zur Verfügung. In der Simulation können Sie sich in Ihrem eigenen Tempo durch ein ähnliches Szenario klicken. Es gibt zwar Unterschiede zwischen der interaktiven Simulation und diesem Lab, aber viele der Kernkonzepte sind identisch. Ein Azure-Abonnement ist nicht erforderlich. 

+ [Sicherer Netzwerkdatenverkehr.](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2013) Erstellen Sie eine VM, ein virtuelles Netzwerk und eine Netzwerksicherheitsgruppe. Fügen Sie Netzwerksicherheitsgruppen-Regeln hinzu, um Datenverkehr zuzulassen und zu verweigern.
  
+ [Erstellen eines einfachen virtuellen Netzwerks.](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%204) Erstellen Sie ein virtuelles Netzwerk mit zwei VMs. Demonstrieren Sie, dass die VMs kommunizieren können. 

+ [Entwerfen und Implementieren eines virtuellen Netzwerks in Azure.](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Design%20and%20implement%20a%20virtual%20network%20in%20Azure) Erstellen Sie eine Ressourcengruppe und virtuelle Netzwerke mit Subnetzen.  

+ [Implementieren von virtuellen Netzwerken.](https://mslabs.cloudguides.com/en-us/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%208) Erstellen und konfigurieren Sie ein virtuelles Netzwerk, stellen Sie VMs bereit und konfigurieren Sie Netzwerksicherheitsgruppen und Azure DNS.

## Architekturdiagramm

![Netzwerklayout](../media/az104-lab04-architecture.png)

Diese virtuellen Netzwerke und Subnetze sind auf eine Weise strukturiert, die vorhandene Ressourcen unterstützt und gleichzeitig das geplante Wachstum ermöglicht. Erstellen Sie diese virtuellen Netzwerke und Subnetze als Grundlage für Ihre Netzwerkinfrastruktur.

>**Schon gewusst?** Es empfiehlt sich, überlappende IP-Adressbereiche zu vermeiden, um Probleme zu reduzieren und die Problembehandlung zu vereinfachen. Überlappung ist ein Problem im gesamten Netzwerk, ob in der Cloud oder lokal. Viele Organisationen entwerfen ein unternehmensweites IP-Adressierungsschema, um Überlappung zu vermeiden und zukünftiges Wachstum zu berücksichtigen.

## Stellenqualifikationen

+ Aufgabe 1: Erstellen eines virtuellen Netzwerks mit Subnetzen mithilfe des Portals
+ Aufgabe 2: Erstellen eines virtuellen Netzwerks und von Subnetzen mithilfe einer Vorlage
+ Aufgabe 3: Erstellen und Konfigurieren der Kommunikation zwischen einer Anwendungssicherheitsgruppe und einer Netzwerksicherheitsgruppe
+ Aufgabe 4: Konfigurieren öffentlicher und privater Azure DNS-Zonen
  
## Aufgabe 1: Erstellen eines virtuellen Netzwerks mit Subnetzen mithilfe des Portals

Die Organisation plant ein starkes Wachstum für Kerndienste. In dieser Aufgabe erstellen Sie das virtuelle Netzwerk und die zugehörigen Subnetze für die vorhandenen Ressourcen und das geplante Wachstum. In dieser Aufgabe verwenden Sie das Azure-Portal. 

1. Melden Sie sich beim **Azure-Portal** - `https://portal.azure.com` an.
   
1. Suchen Sie nach `Virtual Networks`, und wählen Sie diese Option aus.

1. Wählen Sie auf der Seite „Virtuelle Netzwerke“ die Option **Erstellen** aus.

1. Füllen Sie die Registerkarte **Grundlagen** für „CoreServicesVnet“ aus.  

    |  **Option**         | **Wert**            |
    | ------------------ | -------------------- |
    | Ressourcengruppe     | `az104-rg4` (Bei Bedarf neu erstellen) |
    | Name               | `CoreServicesVnet`     |
    | Region             | (US) **USA, Osten**         |

1. Wechseln Sie zur Registerkarte **IP-Adressen**.

    |  **Option**         | **Wert**            |
    | ------------------ | -------------------- |

    | IPv4-Adressraum | `10.20.0.0/16` (Einträge trennen)    |

1. Wählen Sie **+ Subnetz hinzufügen** aus. Geben Sie die Namen- und Adressinformationen für jedes Subnetz ein. Achten Sie darauf, für jedes neue Subnetz **Hinzufügen** auszuwählen. 

    | **Subnetz**             | **Option**           | **Wert**              |
    | ---------------------- | -------------------- | ---------------------- |
    | SharedServicesSubnet   | Subnetzname          | `SharedServicesSubnet`   |
    |                        | Startadresse     | `10.20.10.0`          |
    |                        | Größe                 | `/24` |
    | DatabaseSubnet         | Subnetzname          | `DatabaseSubnet`         |
    |                        | Startadresse     | `10.20.20.0`        |
    |                        | Größe                 | `/24` |

    >**Hinweis:** Jedes virtuelle Netzwerk muss über mindestens ein Subnetz verfügen. Beachten Sie, dass immer fünf IP-Adressen reserviert werden. Berücksichtigen Sie dies daher in Ihrer Planung. 

1. Um die Erstellung von „CoreServicesVnet“ und der zugehörigen Subnetze abzuschließen, wählen Sie **Überprüfen + erstellen** aus.

1. Vergewissern Sie sich, dass Ihre Konfiguration erfolgreich überprüft wurde, und klicken Sie dann auf **Erstellen**.

1. Warten Sie, bis das virtuelle Netzwerk bereitgestellt wurde, und wählen Sie dann **Zu Ressource wechseln** aus.

1. Nehmen Sie sich eine Minute Zeit, um den **Adressraum** und die **Subnetze** zu überprüfen. Beachten Sie die anderen Optionen auf dem Blatt **Einstellungen**. 

1. Wählen Sie im Abschnitt **Automatisierung** die Option **Vorlage exportieren** aus, und warten Sie dann, bis die Vorlage generiert wurde.

1. Sie müssen die Vorlage **herunterladen**.

1. Navigieren Sie auf dem lokalen Computer zum Ordner **Downloads**, und wählen Sie für die Dateien in der heruntergeladenen ZIP-Datei die Option **Alle extrahieren** aus. 

1. Stellen Sie vor dem Fortfahren sicher, dass Sie über die Datei **template.json** verfügen. Sie verwenden diese Vorlage, um „ManufacturingVnet“ in der nächsten Aufgabe zu erstellen. 
 
## Aufgabe 2: Erstellen eines virtuellen Netzwerks und von Subnetzen mithilfe einer Vorlage

In dieser Aufgabe erstellen Sie das virtuelle Netzwerk „ManufacturingVnet“ und zugehörige Subnetze. Die Organisation rechnet mit einem Wachstum der Produktionsbüros, sodass die Subnetze für das erwartete Wachstum dimensioniert werden. Für diese Aufgabe verwenden Sie eine Vorlage, um die Ressourcen zu erstellen. 

1. Suchen Sie die Datei **template.json**, die in der vorherigen Aufgabe exportiert wurde. Sie sollte sich im Ordner **Downloads** befinden.

1. Bearbeiten Sie die Datei in einem Editor Ihrer Wahl. Viele Editoren verfügen über das Feature *Alle Vorkommen ändern*. Stellen Sie bei Verwendung von Visual Studio Code sicher, dass Sie in einem **vertrauenswürdigen Fenster** und nicht im **eingeschränkten Modus** arbeiten. Details finden Sie im Architekturdiagramm. 

### Ändern des virtuellen Netzwerks „ManufacturingVnet“

1. Ersetzen Sie alle Vorkommen von **CoreServicesVnet** durch `ManufacturingVnet`. 

1. Ersetzen Sie alle Vorkommen von **10.20.0.0** durch `10.30.0.0`. 

### Ändern der ManufacturingVnet-Subnetze

1. Ändern Sie alle Vorkommen von **SharedServicesSubnet** in `SensorSubnet1`.

1. Und ändern Sie alle Vorkommen von **10.20.10.0/24** in `10.30.20.0/24`.

1. Ändern Sie alle Vorkommen von **DatabaseSubnet** in `SensorSubnet2`.

1. Und ändern Sie alle Vorkommen von **10.20.20.0/24** in `10.30.21.0/24`.

1. Lesen Sie die Datei noch einmal, und vergewissern Sie sich, dass alles korrekt aussieht.

1. Klicken Sie auf **Speichern**, um die Änderungen zu speichern.

>**Hinweis:** Es gibt eine fertige Vorlagendatei im Verzeichnis mit den Labdateien. 

### Ändern der Parameterdatei

1. Suchen Sie die Datei **parameters.json**, die in der vorherigen Aufgabe exportiert wurde. Sie sollte sich im Ordner **Downloads** befinden.

1. Bearbeiten Sie die Datei in einem Editor Ihrer Wahl.

1. Ersetzen Sie das eine Vorkommen von **CoreServicesVnet** durch `ManufacturingVnet`.

1. **Speichern** Sie die Änderungen.
   
### Bereitstellen der benutzerdefinierten Vorlage

1. Suchen Sie im Portal nach **Benutzerdefinierte Vorlage bereitstellen**, und wählen Sie diese Option aus.

1. Wählen Sie **Eigene Vorlage im Editor erstellen** und dann **Datei laden** aus.

1. Wählen Sie die Datei **templates.json** Datei mit den Änderungen für ManufacturingVnet und dann **Speichern** aus.

1. Wählen Sie **Überprüfen + erstellen** und danach **Erstellen** aus.

1. Warten Sie, bis die Vorlage bereitgestellt wurde, und vergewissern Sie sich (im Portal), dass das virtuelle Netzwerk „ManufacturingVnet“ und die Subnetze erstellt wurden.

>**Hinweis:** Wenn Sie die Bereitstellung mehrmals durchführen müssen, stellen Sie möglicherweise fest, dass einige Ressourcen erfolgreich abgeschlossen wurden und die Bereitstellung fehlschlägt. Sie können diese Ressourcen manuell entfernen und den Vorgang wiederholen. 
   
## Aufgabe 3: Erstellen und Konfigurieren der Kommunikation zwischen einer Anwendungssicherheitsgruppe und einer Netzwerksicherheitsgruppe

In dieser Aufgabe erstellen Sie eine Anwendungssicherheitsgruppe und eine Netzwerksicherheitsgruppe. Die NSG verfügt über eine Sicherheitsregel für eingehenden Datenverkehr, die Datenverkehr von der ASG zulässt. Die NSG verfügt außerdem über eine Ausgangsregel, die den Zugriff auf das Internet verweigert. 

### Erstellen der Anwendungssicherheitsgruppe (ASG)

1. Suchen Sie im Azure-Portal nach `Application security groups` und wählen Sie es aus.

1. Klicken Sie auf **Erstellen**, und geben Sie die grundlegenden Informationen ein.

    | Einstellung | Wert |
    | -- | -- |
    | Abonnement | *Ihr Abonnement* |
    | Ressourcengruppe | **az104-rg4** |
    | Name | `asg-web` |
    | Region | **USA, Osten**  |

1. Klicken Sie auf **Überprüfen + erstellen** und nach der Validierung auf **Erstellen**.

### Erstellen Sie die Netzwerksicherheitsgruppe, und ordnen Sie sie dem ASG-Subnetz zu.

1. Suchen Sie im Azure-Portal nach `Network security groups` und wählen Sie es aus.

1. Wählen Sie **+ Erstellen** aus, und geben Sie Informationen auf der Registerkarte **Grundlagen** an. 

    | Einstellung | Wert |
    | -- | -- |
    | Abonnement | *Ihr Abonnement* |
    | Ressourcengruppe | **az104-rg4** |
    | Name | `myNSGSecure` |
    | Region | **USA, Osten**  |

1. Klicken Sie auf **Überprüfen + erstellen** und nach der Validierung auf **Erstellen**.

1. Klicken Sie nach der Bereitstellung der NSG auf **Zu Ressource wechseln**.

1. Klicken Sie unter **Einstellungen** auf **Subnetze** und dann auf **Zuordnen**.

    | Einstellung | Wert |
    | -- | -- |
    | Virtuelles Netzwerk | **CoreServicesVnet (az104-rg4)** |
    | Subnetz | **SharedServicesSubnet** |

1. Klicken Sie auf **OK**, um die Zuordnung zu speichern.

### Konfigurieren einer Sicherheitsregel für eingehenden Datenverkehr zum Zulassen von ASG-Datenverkehr

1. Verwenden Sie weiterhin Ihre NSG. Wählen Sie im Bereich **Einstellungen** die Option **Sicherheitsregeln für eingehenden Datenverkehr** aus.

1. Überprüfen Sie die Standardeingangsregeln. Beachten Sie, dass nur andere virtuelle Netzwerke und Lastenausgleichsmodule Zugriff haben.

1. Wählen Sie **+ Hinzufügen**.

1. Geben Sie auf dem Blatt **Eingangssicherheitsregel hinzufügen** die folgenden Informationen ein, um eine Portregel für eingehenden Datenverkehr hinzuzufügen. Diese Regel lässt ASG-Datenverkehr zu. Wenn Sie fertig sind, wählen Sie **Hinzufügen** aus.

    | Einstellung | Wert |
    | -- | -- |
    | `Source` | **Anwendungssicherheitsgruppe** |
    | Quell-Anwendungssicherheitsgruppen | **asg-web** |
    | Quellportbereiche |  * |
    | Destination | **Alle** |
    | Dienst | **Benutzerdefiniert** (Andere Auswahlmöglichkeiten beachten) |
    | Zielportbereiche | **80, 443** |
    | Protokoll | **TCP** |
    | Aktion | **Zulassen** |
    | Priorität | **100** |
    | Name | `AllowASG` |

### Konfigurieren einer NSG-Regel für ausgehenden Datenverkehr, die den Internetzugriff verweigert

1. Nachdem Sie Ihre NSG-Regel für eingehenden Datenverkehr erstellt haben, wählen Sie **Sicherheitsregeln für ausgehenden Datenverkehr** aus. 

1. Beachten Sie die Regel **AllowInternetOutboundRule**. Beachten Sie außerdem, dass die Regel nicht gelöscht werden kann und die Priorität 65001 ist.

1. Wählen Sie **+Hinzufügen**, aus und konfigurieren Sie dann eine Ausgangsregel, die den Zugriff auf das Internet verweigert. Wenn Sie fertig sind, wählen Sie **Hinzufügen** aus.

    | Einstellung | Wert |
    | -- | -- |
    | Quelle | **Alle** |
    | Quellportbereiche |  * |
    | Destination | **Diensttag** |
    | Zieldiensttag | **Internet** |
    | Dienst | **Benutzerdefiniert** |
    | Zielportbereiche | **8080** |
    | Protocol | **Alle** |
    | Aktion | **Deny** (Verweigern) |
    | Priority | **4096** |
    | Name | **DenyAnyCustom8080Outbound** |


## Aufgabe 4: Konfigurieren öffentlicher und privater Azure DNS-Zonen

In dieser Aufgabe erstellen und konfigurieren Sie öffentliche und private DNS-Zonen. 

### Konfigurieren einer öffentlichen DNS-Zone

Sie können Azure DNS zum Auflösen der Hostnamen in Ihrer öffentlichen Domäne konfigurieren. Wenn Sie beispielsweise den Domänennamen „contoso.xyz“ von einer Domänennamen-Registrierungsstelle erworben haben, können Sie Azure DNS für das Hosten der Domäne `contoso.com` und das Auflösen von „www.contoso.xyz“ in die IP-Adresse Ihres Webservers oder Ihrer Web-App konfigurieren.

1. Suchen Sie im Portal die Option `DNS zones`, und wählen Sie sie aus.

1. Wählen Sie **+ Erstellen** aus.

1. Konfigurieren Sie die Registerkarte **Grundlagen**.

    | Eigenschaft | Wert    |
    |:---------|:---------|
    | Abonnement | **Wählen Sie Ihr Abonnement aus** |
    | Resource group | **az04-rg4** |
    | Name | `contoso.com` (Falls reserviert, Namen anpassen) |
    | Region |**USA, Osten** (Infosymbol überprüfen) |

1. Wählen Sie **Überprüfen + erstellen** und danach **Erstellen** aus.
   
1. Warten Sie, bis die DNS-Zone bereitgestellt wurde, und wählen Sie dann **Zu Ressource wechseln** aus.

1. Beachten Sie auf dem Blatt **Übersicht** die Namen der vier Azure DNS-Namenserver, die der Zone zugewiesen sind. **Kopieren** Sie eine der Namenserveradressen. Sie benötigen sie in einem späteren Schritt. 
  
1. Wählen Sie **+ Datensatzgruppe** aus. Sie fügen für jedes virtuelle Netzwerk, das Unterstützung für die Namensauflösung privater Zonen benötigt, einen Eintrag für VNET-Verknüpfungen hinzu.

    | Eigenschaft | Wert    |
    |:---------|:---------|
    | Name | **www** |
    | type | **A** |
    | TTL | **1** |
    | IP-Adresse | **10.1.1.4** |

>**Hinweis:**  In einem realen Szenario würden Sie die öffentliche IP-Adresse Ihres Webservers eingeben.

1. Wählen Sie **OK** aus, und überprüfen Sie, ob für **contoso.com** ein A-Eintragssatz namens **www** angegeben ist.

1. Öffnen Sie eine Eingabeaufforderung, und führen Sie den folgenden Befehl aus:

   ```sh
   nslookup www.contoso.com <name server name>
   ```
1. Überprüfen Sie, ob der Hostname „www.contoso.com“ in die angegebene IP-Adresse aufgelöst wird. Dadurch wird sichergestellt, dass die Namensauflösung ordnungsgemäß funktioniert.

### Konfigurieren einer privaten DNS-Zone

Eine private DNS-Zone bietet Namensauflösungsdienste innerhalb virtueller Netzwerke. Eine private DNS-Zone ist nur von den virtuellen Netzwerken aus zugänglich, mit denen sie verknüpft ist. Ein Zugriff über das Internet ist nicht möglich. 

1. Suchen Sie im Portal die Option `Private dns zones`, und wählen Sie sie aus.

1. Wählen Sie **+ Erstellen** aus.

1. Geben Sie auf der Registerkarte **Grundeinstellungen** von „Private DNS-Zone erstellen“ die Informationen aus der folgenden Tabelle ein:

    | Eigenschaft | Wert    |
    |:---------|:---------|
    | Abonnement | **Wählen Sie Ihr Abonnement aus** |
    | Resource group | **az04-rg4** |
    | Name | `private.contoso.com` (Anpassen, falls eine Umbenennung erforderlich war) |
    | Region |**USA, Osten** |

1. Wählen Sie **Überprüfen + erstellen** und danach **Erstellen** aus.
   
1. Warten Sie, bis die DNS-Zone bereitgestellt wurde, und wählen Sie dann **Zu Ressource wechseln** aus.

1. Beachten Sie auf dem Blatt **Übersicht**, dass keine Namenservereinträge vorhanden sind. 

1. Klicken Sie auf **VNet-Verknüpfungen** und anschließend auf **+ Hinzufügen**. 

    | Eigenschaft | Wert    |
    |:---------|:---------|
    | Linkname | `manufacturing-link` |
    | Virtuelles Netzwerk | `ManufacturingVnet` |

1. Wählen Sie **OK** aus, und warten Sie, bis der Link erstellt wurde. 

1. Wählen Sie auf dem Blatt **Übersicht** die Option **+ Eintragssatz** aus. Sie fügen nun für jede VM, die Unterstützung für die Namensauflösung privater Zonen benötigt, einen Eintrag hinzu.

    | Eigenschaft | Wert    |
    |:---------|:---------|
    | Name | **sensorvm** |
    | type | **A** |
    | TTL | **1** |
    | IP-Adresse | **10.1.1.4** |

 >**Hinweis:**  In einem realen Szenario geben Sie die IP-Adresse für eine bestimmte Fertigungs-VM ein.

## Bereinigen Ihrer Ressourcen

Wenn Sie mit **Ihrem eigenen Abonnement** arbeiten, nehmen Sie sich eine Minute Zeit, um die Labressourcen zu löschen. Dadurch wird sichergestellt, dass Ressourcen freigegeben und Kosten minimiert werden. Die einfachste Möglichkeit zum Löschen der Labressourcen besteht darin, die Ressourcengruppe des Labs zu löschen. 

+ Wählen Sie im Azure-Portal die Ressourcengruppe und dann **Ressourcengruppe löschen** aus, **geben Sie den Ressourcengruppennamen ein**, und klicken Sie dann auf **Löschen**.
+ Bei Verwendung von Azure PowerShell: `Remove-AzResourceGroup -Name resourceGroupName`.
+ Bei Verwendung der Befehlszeilenschnittstelle: `az group delete --name resourceGroupName`.
 
## Wichtige Erkenntnisse

Herzlichen Glückwunsch zum erfolgreichen Abschluss des Labs. Hier sind die wichtigsten Erkenntnisse für dieses Lab. 

+ Ein virtuelles Netzwerk ist eine Darstellung Ihres eigenen Netzwerks in der Cloud. 
+ Beim Entwerfen virtueller Netzwerke empfiehlt es sich, überlappende IP-Adressbereiche zu vermeiden. Dadurch werden Probleme reduziert, und die Problembehandlung wird vereinfacht.
+ Ein Subnetz ist ein Bereich von IP-Adressen im virtuellen Netzwerk. Sie können ein virtuelles Netzwerk aus Organisations- und Sicherheitsgründen in mehrere Subnetze unterteilen.
+ Eine Netzwerksicherheitsgruppe enthält Sicherheitsregeln, die Netzwerkdatenverkehr zulassen oder verweigern. Es gibt Standardregeln für eingehenden und ausgehenden Datenverkehr, die Sie an Ihre Anforderungen anpassen können.
+ Anwendungssicherheitsgruppen werden verwendet, um Servergruppen mit einer gemeinsamen Funktion zu schützen, etwa Webserver oder Datenbankserver.
+ Azure DNS ist ein Hostingdienst für DNS-Domänen, der Namensauflösung bietet. Sie können Azure DNS zum Auflösen der Hostnamen in Ihrer öffentlichen Domäne konfigurieren.  Sie können private DNS-Zonen auch verwenden, um VMs in Ihren virtuellen Azure-Netzwerken DNS-Namen zuzuweisen.

## Weiterlernen im eigenen Tempo

+ [Einführung in Azure Virtual Network](https://learn.microsoft.com/training/modules/introduction-to-azure-virtual-networks/): Sie entwerfen und implementieren Azure-Kernnetzwerkinfrastruktur, z. B. virtuelle Netzwerke, öffentliche und private IP-Adressen, DNS, Peering virtueller Netzwerke, Routing und Azure Virtual NAT.
+ [Entwerfen eines IP-Adressierungsschemas.](https://learn.microsoft.com/training/modules/design-ip-addressing-for-azure/) Sie ermitteln die Funktionen der privaten und öffentlichen IP-Adressierung von Azure-Netzwerken und lokalen Netzwerken.
+ [Schützen und Isolieren des Zugriffs auf Azure-Ressourcen mithilfe von Netzwerksicherheitsgruppen und Dienstendpunkten.](https://learn.microsoft.com/training/modules/secure-and-isolate-with-nsg-and-service-endpoints/) Netzwerksicherheitsgruppen und Dienstendpunkte unterstützen Sie dabei, Ihre VMs (virtuelle Computer) und Azure-Dienste vor nicht autorisiertem Netzwerkzugriff zu schützen.
+ [Hosten Ihrer Domäne in Azure DNS.](https://learn.microsoft.com/training/modules/host-domain-azure-dns/) Erstellen Sie eine DNS-Zone für Ihren Domänennamen. Erstellen Sie DNS-Einträge, um die Domain einer IP-Adresse zuzuordnen. Testen Sie, ob der Domänenname in Ihren Webserver aufgelöst wird.
  
