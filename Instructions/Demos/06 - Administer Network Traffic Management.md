---
demo:
  title: "Demo\_06: Verwalten von Netzwerkdatenverkehr"
  module: Administer Network Traffic Management
---


# 06: Verwalten von Netzwerkdatenverkehr

## Konfigurieren von Azure Load Balancer

In dieser Demonstration erfahren Sie, wie Sie einen öffentlichen Lastenausgleich erstellen. 

**Hinweis:**  Für diese Demo ist ein virtuelles Netzwerk mit mindestens einem Subnetz erforderlich. 

**Referenz:** [Schnellstart: Erstellen eines öffentlichen Lastenausgleichs für den Lastenausgleich virtueller Computer über das Azure-Portal](https://learn.microsoft.com/azure/load-balancer/quickstart-load-balancer-standard-public-portal)

**Zeigen Sie das Feature „Auswahlhilfe“ des Portals.**

1. Öffnen Sie das Azure-Portal.

1. Suchen Sie nach Lastenausgleich, und wählen Sie die Option **Lastenausgleich > Auswahlhilfe** aus.

1. Verwenden Sie den Assistenten, um verschiedene Szenarios durchzugehen.
   
**Erstellen eines Lastenausgleichs**

1. Fahren Sie im Azure-Portal fort.

1. Suchen Sie nach **Lastenausgleich**, und wählen Sie diese Option aus. **Erstellen** Sie einen Lastenausgleich. 

1. Besprechen Sie auf der Registerkarte **Grundeinstellungen** die Angaben **SKU**, **Typ** und **Tarif**.

1. Besprechen Sie auf der Registerkarte **Front-End-IP-Konfiguration** die Verwendung einer öffentlichen IP-Adresse.

1. Wählen Sie auf der Registerkarte **Back-End-Pools** das virtuelle Netzwerk mit IP-Adressbereich aus.

1. Erstellen Sie auf der Registerkarte **Eingangsregeln** eine Lastenausgleichsregel. Besprechen Sie Parameter wie **Protokoll**, **Ports**, **Integritätstests** und **Sitzungspersistenz**. 


## Konfigurieren von Azure Application Gateway

In dieser Demonstration lernen Sie, wie Sie eine Azure Application Gateway-Instanz erstellen. 

**Hinweis:** Erstellen Sie der Einfachheit halber neue virtuelle Netzwerke und Subnetze, während Sie die Konfiguration durchlaufen. 

**Referenz:** [Schnellstart: Weiterleiten von Webdatenverkehr per Azure Application Gateway – Azure-Portal](https://learn.microsoft.com/azure/application-gateway/quick-create-portal)

**Erstellen Sie die Azure Application Gateway-Instanz.**

1. Öffnen Sie das Azure-Portal.

1. Suchen und wählen Sie **Azure Application Gateway** aus.

1. **Erstellen** Sie ein neues Gateway.

1. Besprechen Sie auf der Registerkarte **Grundeinstellungen** die Optionen **Tarife**, **Automatische Skalierung** und **Instanzen**.

1. Besprechen Sie auf der Registerkarte **Front-Ends** die IP-Adresstypen.

1. Besprechen Sie auf der Registerkarte **Back-Ends**, wann ein leerer Back-End-Pool verwendet werden soll.

1. Besprechen Sie auf der Registerkarte **Konfiguration** die Routingregeln. Vergleichen Sie diese mit den Lastenausgleichsregeln.

1. Erläutern Sie, dass nach dem Erstellen des Gateways die Back-End-Ziele hinzugefügt und getestet werden sollten. 
