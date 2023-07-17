---
demo:
  title: "Demo\_04: Verwalten von virtuellen Netzwerken"
  module: Administer Virtual Networking
---

# 04: Verwalten von virtuellen Netzwerken

## Konfigurieren virtueller Netzwerke

In dieser Demo erstellen Sie virtuelle Netzwerke.

**Referenz:** [Schnellstart: Erstellen eines virtuellen Netzwerks – Azure-Portal](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

## Erstellen eines virtuellen Netzwerks im Portal

1.  Melden Sie sich beim Azure-Portal an, und suchen Sie nach  **Virtuelle Netzwerke**.

1.  Erstellen Sie ein virtuelles Netzwerk, und erläutern Sie die grundlegenden Einstellungen. Stellen Sie sicher, dass mindestens ein Subnetz erstellt wird. 

1.  Überprüfen Sie, ob das virtuelle Netzwerk erstellt wurde.

## Konfigurieren von Netzwerksicherheitsgruppen

In dieser Demo erkunden Sie Netzwerksicherheitsgruppen (NSGs) und Dienstendpunkte.

**Referenz:** [Einschränken des Zugriffs auf PaaS-Ressourcen – Tutorial – Azure-Portal](https://docs.microsoft.com/azure/virtual-network/tutorial-restrict-network-access-to-resources)

**Erstellen einer Netzwerksicherheitsgruppe**

1. Öffnen Sie das Azure-Portal.

1. Navigieren Sie zum Eintrag **Netzwerksicherheitsgruppen**, und wählen Sie ihn aus.

1. Erstellen Sie eine NSG, und erläutern Sie die Einstellungen. 
 
1. Warten Sie, bis die neue NSG bereitgestellt wurde.

**Erkunden von Eingangs- und Ausgangsregeln**

1. Wählen Sie Ihre neue NSG aus.

1. Diskutieren Sie, wie die NSG Subnetzen oder Netzwerkschnittstellen zugeordnet werden kann.

1. Erörtern Sie die zweckmäßigen Eingangs- und Ausgangsregeln.  

1. Überprüfen Sie die Standardeingangs- und -ausgangsregeln. 

1. Erstellen Sie eine neue Regel, und erläutern Sie die Einstellungen. Erläutern Sie insbesondere die Dienstauswahl (z. B. HTTPS) und die Prioritätseinstellungen. 

## Konfigurieren von Azure DNS

In dieser Demo erkunden Sie Azure DNS.

**Referenz:** [Tutorial: Hosten Ihrer Domäne und Unterdomäne – Azure DNS](https://docs.microsoft.com/azure/dns/dns-delegate-domain-azure-dns)


**Erstellen einer DNS-Zone**

1. Öffnen Sie das Azure-Portal.

1. Suchen Sie nach dem **DNS-Zonendienst** .

1. Erstellen Sie eine **DNS-Zone**, und erläutern Sie den Zweck der Zone. Für einen Namen können Sie contoso.internal.com verwenden.

1.  Warten Sie, bis die DNS-Zone erstellt wurde. Möglicherweise müssen Sie die Seite **aktualisieren** .

**Hinzufügen eines DNS-Ressourceneintragssatzes**

**Referenz:** [Tutorial: Erstellen eines Aliaseintrags zum Verweisen auf einen Zonenressourceneintrag](https://learn.microsoft.com/azure/dns/tutorial-alias-rr)

1. Nachdem Ihre Zone erstellt wurde, wählen Sie **+Datensatzgruppe** aus.

1. Verwenden Sie die Dropdownliste **Typ** , um die verschiedenen Arten von Einträgen anzuzeigen. Überprüfen Sie, wie die verschiedenen Datensatztypen verwendet werden. Beachten Sie, wie sich die Datensatzinformationen ändern, wenn Sie verschiedene Datensatztypen auswählen.

1. Erstellen Sie als Beispiel einen **A**-Datensatz. 

