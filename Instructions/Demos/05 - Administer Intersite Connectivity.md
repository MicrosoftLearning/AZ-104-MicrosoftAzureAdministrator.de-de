---
demo:
  title: 'Demo 05: Verwalten der standortübergreifenden Konnektivität'
  module: Administer Intersite Connectivity
---

# 05: Verwalten der standortübergreifenden Konnektivität

## Konfigurieren des VNet-Peerings

**Hinweis:**  Für diese Demo benötigen Sie zwei virtuelle Netzwerke.

**Referenz:** [Herstellen von Verbindungen zwischen virtuellen Netzwerken mittels VNet-Peering – Tutorial](https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal)

**Konfigurieren des VNet-Peerings für das erste virtuelle Netzwerk**

1. Wählen Sie im  **Azure-Portal** das erste virtuelle Netzwerk aus. Überprüfen Sie den Wert des Peerings. 

1. Wählen Sie unter  **Einstellungen** die Option  **Peerings** und **+ Neues Peering hinzufügen** aus.

1. Konfigurieren Sie das Peering des zweiten virtuellen Netzwerks. Verwenden Sie die Informationssymbole, um die verschiedenen Einstellungen zu überprüfen. 

1. Wenn das Peering abgeschlossen ist, überprüfen Sie den **Peeringstatus**. 

**Konfigurieren des VNet-Peerings für das zweite virtuelle Netzwerk**

1. Wählen Sie im  **Azure-Portal** das zweite virtuelle Netzwerk aus.

1. Wählen Sie unter  **Einstellungen** die Option  **Peerings** aus.

1. Beachten Sie, dass automatisch ein Peering erstellt wurde.  Beachten Sie, dass der  **Peeringstatus**  **Verbunden** lautet.

1. Im Lab erstellen die Lernenden Peerings und testen die Verbindung zwischen virtuellen Computern. 

## Konfigurieren von Netzwerkrouting und Endpunkten

In dieser Demo erhalten wir Informationen zum Erstellen einer Routingtabelle, zum Definieren einer benutzerdefinierten Route sowie zum Zuordnen der Route zu einem Subnetz.

**Hinweis:**  Für diese Demo ist ein virtuelles Netzwerk mit mindestens einem Subnetz erforderlich.

**Referenz:** [Weiterleiten von Netzwerkdatenverkehr: Tutorial – Azure-Portal](https://learn.microsoft.com/azure/virtual-network/tutorial-create-route-table-portal#create-a-route-table)

**Erstellen Sie eine Routingtabelle**

1. Wenn Sie Zeit haben, sehen Sie sich das Tutorialdiagramm an. Erläutern Sie, warum Sie eine benutzerdefinierte Route erstellen müssen. 

1. Öffnen Sie das Azure-Portal.

1. Suchen Sie nach **Routingtabellen**, und wählen Sie diese Option aus. Besprechen Sie, wann **verteilte Gatewayrouten** verwendet werden sollen. 

1. Erstellen Sie eine Routingtabelle, und erläutern Sie alle ungewöhnlichen Einstellungen. 

1. Warten Sie, bis die neue Routingtabelle bereitgestellt wurde.

**Hinzufügen einer Route**

1.  Wählen Sie Ihre neue Routingtabelle und dann  **Routen** aus.

1.  Erstellen Sie eine neue **Route**. Diskutieren Sie die verschiedenen **verfügbaren Hoptypen**. 

1.  Erstellen Sie die neue Route, und warten Sie, bis die Ressource bereitgestellt wird.
 
**Zuordnen einer Routingtabelle zu einem Subnetz**

1.  Navigieren Sie zu dem Subnetz, das Sie der Routingtabelle zuordnen möchten.

1.  Wählen Sie **Routingtabelle** aus, und wählen Sie Ihre neue Routingtabelle aus. 

1.  **Speichern** Sie Ihre Änderungen.

