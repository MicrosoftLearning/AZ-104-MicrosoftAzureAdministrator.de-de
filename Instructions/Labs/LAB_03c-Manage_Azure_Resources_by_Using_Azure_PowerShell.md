---
lab:
  title: 03c – Verwalten von Azure-Ressourcen mithilfe von Azure PowerShell
  module: Module 03 - Azure Administration
---

# <a name="lab-03c---manage-azure-resources-by-using-azure-powershell"></a>Lab 03c – Verwalten von Azure-Ressourcen mithilfe von Azure PowerShell
# <a name="student-lab-manual"></a>Lab-Handbuch für Kursteilnehmer

## <a name="lab-scenario"></a>Labszenario

Now that you explored the basic Azure administration capabilities associated with provisioning resources and organizing them based on resource groups by using the Azure portal and Azure Resource Manager templates, you need to carry out the equivalent task by using Azure PowerShell. To avoid installing Azure PowerShell modules, you will leverage PowerShell environment available in Azure Cloud Shell.

## <a name="objectives"></a>Ziele

Dieses Lab deckt Folgendes ab:

+ Aufgabe 1: Starten einer PowerShell-Sitzung in Azure Cloud Shell
+ Aufgabe 2: Erstellen einer Ressourcengruppe und eines verwalteten Azure-Datenträgers mithilfe von Azure PowerShell
+ Aufgabe 3: Konfigurieren des verwalteten Datenträgers mithilfe von Azure PowerShell

## <a name="estimated-timing-20-minutes"></a>Geschätzte Zeit: 20 Minuten

## <a name="architecture-diagram"></a>Architekturdiagramm

![image](../media/lab03c.png)

## <a name="instructions"></a>Anweisungen

> <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>:  Always create your own secure password for any virtual machine or user account you create. If the virtual machine is created for you, use <bpt id="p1">**</bpt>Reset password<ept id="p1">**</ept> in the Portal to update the password. 

### <a name="exercise-1"></a>Übung 1

#### <a name="task-1-start-a-powershell-session-in-azure-cloud-shell"></a>Aufgabe 1: Starten einer PowerShell-Sitzung in Azure Cloud Shell

In dieser Aufgabe öffnen Sie eine PowerShell-Sitzung in Cloud Shell. 

1. Öffnen Sie **Azure Cloud Shell** im Portal, indem Sie oben rechts im Azure-Portal auf das entsprechende Symbol klicken.

1. Wenn Sie aufgefordert werden, entweder **Bash** oder **PowerShell** auszuwählen, wählen Sie **PowerShell** aus. 

    >**Hinweis**: Wenn Sie **Cloud Shell** zum ersten Mal starten und die Meldung **Für Sie wurde kein Speicher bereitgestellt** angezeigt wird, wählen Sie das in diesem Lab verwendete Abonnement aus, und klicken Sie dann auf **Speicher erstellen**. 

1. Wenn Sie dazu aufgefordert werden, klicken Sie auf **Speicher erstellen**, und warten Sie, bis der Azure Cloud Shell-Bereich angezeigt wird. 

1. Stellen Sie sicher, dass **PowerShell** im Dropdownmenü oben links im Cloud Shell-Bereich angezeigt wird.

#### <a name="task-2-create-a-resource-group-and-an-azure-managed-disk-by-using-azure-powershell"></a>Aufgabe 2: Erstellen einer Ressourcengruppe und eines verwalteten Azure-Datenträgers mithilfe von Azure PowerShell

In dieser Aufgabe erstellen Sie eine Ressourcengruppe und einen verwalteten Azure-Datenträger, indem Sie eine Azure PowerShell-Sitzung in Cloud Shell verwenden.

1. Um eine Ressourcengruppe in derselben Azure-Region zu erstellen, in der sich auch die im vorherigen Lab erstellte Ressourcengruppe **az104-03b-rg1** befindet, führen Sie in der PowerShell-Sitzung in Cloud Shell Folgendes aus:

   ```powershell
   $location = (Get-AzResourceGroup -Name az104-03b-rg1).Location

   $rgName = 'az104-03c-rg1'

   New-AzResourceGroup -Name $rgName -Location $location
   ```
1. Um Eigenschaften der neu erstellten Ressourcengruppe abzurufen, führen Sie folgenden Befehl aus:

   ```powershell
   Get-AzResourceGroup -Name $rgName
   ```
1. Um einen neuen verwalteten Datenträger mit denselben Merkmalen zu erstellen, die Sie in den vorherigen Labs dieses Moduls für Datenträger verwendet haben, führen Sie Folgendes aus:

   ```powershell
   $diskConfig = New-AzDiskConfig `
    -Location $location `
    -CreateOption Empty `
    -DiskSizeGB 32 `
    -Sku Standard_LRS

   $diskName = 'az104-03c-disk1'

   New-AzDisk `
    -ResourceGroupName $rgName `
    -DiskName $diskName `
    -Disk $diskConfig
   ```

1. Um Eigenschaften des neu erstellten Datenträgers abzurufen, führen Sie folgenden Befehl aus:

   ```powershell
   Get-AzDisk -ResourceGroupName $rgName -Name $diskName
   ```

#### <a name="task-3-configure-the-managed-disk-by-using-azure-powershell"></a>Aufgabe 3: Konfigurieren des verwalteten Datenträgers mithilfe von Azure PowerShell

In dieser Aufgabe verwalten Sie die Konfiguration des verwalteten Azure-Datenträgers mithilfe einer Azure PowerShell-Sitzung in Cloud Shell. 

1. Um die Größe des verwalteten Azure-Datenträgers auf **64 GB** zu erhöhen, führen Sie in der PowerShell-Sitzung in Cloud Shell den folgenden Befehl aus:

   ```powershell
   New-AzDiskUpdateConfig -DiskSizeGB 64 | Update-AzDisk -ResourceGroupName $rgName -DiskName $diskName
   ```

1. Um die Wirksamkeit der Änderung zu überprüfen, führen Sie folgenden Befehl aus:

   ```powershell
   Get-AzDisk -ResourceGroupName $rgName -Name $diskName
   ```

1. Führen Sie Folgendes aus, um die aktuelle SKU als **Standard_LRS** zu überprüfen:

   ```powershell
   (Get-AzDisk -ResourceGroupName $rgName -Name $diskName).Sku
   ```

1. Um die SKU für die Datenträgerleistung in **Premium_LRS** zu ändern, führen Sie in der PowerShell-Sitzung in Cloud Shell den folgenden Befehl aus:

   ```powershell
   New-AzDiskUpdateConfig -Sku Premium_LRS | Update-AzDisk -ResourceGroupName $rgName -DiskName $diskName
   ```

1. Um die Wirksamkeit der Änderung zu überprüfen, führen Sie folgenden Befehl aus:

   ```powershell
   (Get-AzDisk -ResourceGroupName $rgName -Name $diskName).Sku
   ```

#### <a name="clean-up-resources"></a>Bereinigen von Ressourcen

   ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Do not delete resources you deployed in this lab. You will reference them in the next lab of this module.

#### <a name="review"></a>Überprüfung

In diesem Lab haben Sie die folgenden Aufgaben ausgeführt:

- Starten einer PowerShell-Sitzung in Azure Cloud Shell
- Erstellen einer Ressourcengruppe und eines verwalteten Azure-Datenträgers mithilfe von Azure PowerShell
- Konfigurieren des verwalteten Datenträgers mithilfe von Azure PowerShell
