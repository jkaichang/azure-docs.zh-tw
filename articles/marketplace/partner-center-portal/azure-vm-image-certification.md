---
title: 測試從 VHD 部署的虛擬機器（VM）-Azure Marketplace
description: 了解如何在商業 Marketplace 中測試並提交虛擬機器供應項目。
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: iqshahmicrosoft
ms.author: iqshah
ms.date: 07/29/2020
ms.openlocfilehash: 36c497a180070358332997649e768999c78935cb
ms.sourcegitcommit: 0b8320ae0d3455344ec8855b5c2d0ab3faa974a3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2020
ms.locfileid: "87433111"
---
# <a name="test-virtual-machine-vm-deployed-from-vhd"></a>測試從 VHD 部署的虛擬機器（VM）

本文說明如何從上一節（[建立 AZURE VM 技術資產）](create-azure-vm-technical-asset.md)中建立的一般化 VHD 映射部署和測試 Azure 虛擬機器（VM），以確保 VHD 映射符合 Azure Marketplace 發佈需求。

完成這些步驟以產生相容性報告，以證明您的 VHD 映射可用於 Azure Marketplace。

1. 建立及部署遠端 VM 管理所需的憑證，以 Azure Key Vault。
2. 從[建立 AZURE vm 技術資產](create-azure-vm-technical-asset.md)中建立的一般化 VHD 映射，部署 Azure VM。
3. 在已部署的 VM 上執行測試，以確保 VHD 映射已準備好發行並用於部署 Vm。

## <a name="running-scripts"></a>執行腳本

本文包含三個要在 PowerShell 中執行的腳本。 桌面 PowerShell 的效果最佳，但是 Azure Cloud Shell 也可以與選取的 PowerShell 選項（視窗的左上方）搭配使用。

1. 請確定 PowerShell 已設定為執行腳本。

    - 一律使用 [以**系統管理員身分執行**] 選項開啟 PowerShell。
    - 請確定您可以執行下列腳本： `Set-ExecutionPolicy` 和 `RemoteSigned` 。

2. [安裝 Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)。

3. 安裝 Azure PowerShell Az 模組。
    1. **選項 A**：尚未安裝任何模組。
        - `Install-Module -Name Az -AllowClobber -Scope AllUsers`

        如需詳細資訊，請參閱[安裝 Azure PowerShell 模組](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-4.2.0)。

    2. **選項 B**：目前正在使用 AzureRM 模組。

        - 卸載-AzureRM
        - Install-Module-Name Az-AllowClobber-Scope AllUsers
        - AzureRmAlias-Scope CurrentUser

        如需詳細資訊，請參閱[將 Azure PowerShell 從 AzureRM 移轉至 Az](https://docs.microsoft.com/powershell/azure/migrate-from-azurerm-to-az?view=azps-4.2.0)。

4. 儲存會話參數。

本節中的腳本會使用會話變數/參數。 如果您關閉會話，則會清除參數。 我們建議使用一個會話來執行所有腳本，以避免參數值錯誤。 如果無法這麼做，您必須在開啟新的會話時重新初始化參數，特別是針對稍後的腳本。

## <a name="create-and-deploy-certificates-for-azure-key-vault"></a>建立並部署 Azure Key Vault 的憑證

此節描述如何建立並部署設定 Windows 遠端管理 (WinRM) 和 Azure 託管虛擬機器連線所需的自我簽署憑證。

### <a name="create-certificates-for-azure-key-vault"></a>建立 Azure Key Vault 的憑證

此程序由三個步驟組成：

1. 建立安全性憑證。
2. 建立 Azure Key Vault 來儲存憑證。
3. 將憑證儲存到金鑰保存庫。

您可以針對這項工作使用新的或現有的 Azure 資源群組。

#### <a name="create-the-security-certificate"></a>建立安全性憑證

執行此腳本，以在本機資料夾中建立憑證檔案（.pfx）。 憑證屬於已規劃的 Azure VM，將從您的 VHD 映射進行部署。 VM 將需要腳本參數所指定的名稱、位置和密碼。 編輯下列 Azure PowerShell**認證建立腳本**，為數據表中顯示的腳本參數指定正確的值。

| **參數** | **說明** |
| --- | --- |
| $certroopath | 儲存 .pfx 檔案所在的本機資料夾。 |
| $location | 其中一個 Azure 標準地理位置。 |
| $vmName | 已規劃的 Azure 虛擬機器名稱。 |
| $certname | 憑證的名稱；必須符合已規劃 VM 的完整網域名稱。 |
| $certpassword | 憑證的密碼，必須符合用於已規劃 VM 的密碼。 |
| | |

```PowerShell
   # Certification creation script

    # pfx certification stored path
    $certroopath = "C:\certLocation"

    # location of the resource group
    $location = "westus"

    # Azure virtual machine name that we are going to create
    $vmName = "testvm000000906"

    # Certification name - should match with FQDN of Windows Azure creating VM
    $certname = "$vmName.$location.cloudapp.azure.com"

    # Certification password - should be match with password of Windows Azure creating VM
    $certpassword = "SecretPassword@123"

    $cert=New-SelfSignedCertificate -DnsName "$certname" -CertStoreLocation cert:\LocalMachine\My
    $pwd = ConvertTo-SecureString -String $certpassword -Force -AsPlainText
    $certwithThumb="cert:\localMachine\my\"+$cert.Thumbprint
    $filepath="$certroopath\$certname.pfx"
    Export-PfxCertificate -cert $certwithThumb -FilePath $filepath -Password $pwd
    Remove-Item -Path $certwithThumb

```

> [!TIP]
> 在這些步驟期間保持相同的 Azure PowerShell 主控台工作階段開啟並執行中，以保留各種參數的值。

> [!WARNING]
> 如果您儲存此指令碼，僅能將其儲存在安全的位置，因為其包含安全性資訊 (密碼)。

#### <a name="create-the-azure-key-vault-to-store-the-certificate"></a>建立 Azure Key Vault 來儲存憑證

將以下範本的內容複製至本機電腦上的檔案。 在以下的範例指令碼中，此資源是 `C:\certLocation\keyvault.json`。

```JSON
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "keyVaultName": {
      "type": "string",
      "defaultValue":"isvkv0001",
      "metadata": {
        "description": "Name of the Vault"
      }
    },
    "tenantId": {
      "type": "string",
      "defaultValue":"72f988bf-86f1-41af-91ab-2d7cd011db47",
      "metadata": {
        "description": "Tenant Id of the subscription. Get using Get-AzureSubscription cmdlet or Get Subscription API"
      }
    },
    "objectId": {
      "type": "string",
      "defaultValue":"d55739bf-d5d6-4ce0-be1c-49ade53c4315",
      "metadata": {
        "description": "Object Id of the AD user. Get using Get-AzureADUser or Get-AzureADServicePrincipal cmdlets"
      }
    },
    "keysPermissions": {
      "type": "array",
      "defaultValue": ["all"],
      "metadata": {
        "description": "Permissions to keys in the vault. Valid values are: all, create, import, update, get, list, delete, backup, restore, encrypt, decrypt, wrapkey, unwrapkey, sign, and verify."
      }
    },
    "secretsPermissions": {
      "type": "array",
      "defaultValue": ["all"],
      "metadata": {
        "description": "Permissions to secrets in the vault. Valid values are: all, get, set, list, and delete."
      }
    },
    "skuName": {
      "type": "string",
      "defaultValue": "Standard",
      "allowedValues": [
        "Standard",
        "Premium"
      ],
      "metadata": {
        "description": "SKU for the vault"
      }
    },
    "enableVaultForDeployment": {
      "type": "bool",
      "defaultValue": true,
      "allowedValues": [
        true,
        false
      ],
      "metadata": {
        "description": "Specifies if the vault is enabled for a VM deployment"
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.KeyVault/vaults",
      "name": "[parameters('keyVaultName')]",
      "apiVersion": "2015-06-01",
      "location": "[resourceGroup().location]",
      "properties": {
        "enabledForDeployment": "[parameters('enableVaultForDeployment')]",
        "tenantId": "[parameters('tenantId')]",
        "accessPolicies": [
          {
            "tenantId": "[parameters('tenantId')]",
            "objectId": "[parameters('objectId')]",
            "permissions": {
              "keys": "[parameters('keysPermissions')]",
              "secrets": "[parameters('secretsPermissions')]"
            }
          }
        ],
        "sku": {
          "name": "[parameters('skuName')]",
          "family": "A"
        }
      }
    }
  ]
}

```

編輯並執行下列 Azure PowerShell 指令碼以建立 Azure Key Vault 和相關聯的資源群組。 取代下表中所示參數的值

| **參數** | **說明** |
| --- | --- |
| $postfix | 附加至部署識別碼的隨機數字字串。 |
| $rgName | 要建立的 Azure 資源群組 (RG) 名稱。 |
| $location | 其中一個 Azure 標準地理位置。 |
| $kvTemplateJson | 檔案 (keyvault.json) 的路徑，其中包含金鑰保存庫的 Resource Manager 範本。 |
| $kvname | 新金鑰保存庫的名稱。|
|   |   |

```PowerShell
# Creating Key vault in resource group

    # "Random" number for deployment identifiers
    $postfix = "0101048"

    # Resource group name
    $rgName = "TestRG$postfix"

    # Location of Resource Group
    $location = "westus"

    # Key vault template location
    $kvTemplateJson = "C:\certLocation\keyvault.json"

    # Key vault name
    $kvname = "isvkv$postfix"

    # code snippet to get the Azure user object ID
    try
       {
        $accounts = Get-AzContext
        $accountNum = 0
        $accounts.Account | %{ ++$accountNum; Write-Host $accountNum $_}
        Write-Host "`nPlease select User, e.g. 1:" -ForegroundColor DarkYellow
        [Int] $accountChoice = Read-Host

        While($accountChoice -lt 1 -or $accountChoice -gt $accounts.Length)
        {
            Write-Host "incorrect input" -ForegroundColor Red
            Write-Host "`nPlease select User, e.g. 1:" -ForegroundColor DarkYellow
            [Int] $accountChoice = Read-Host
        }

        $accountSelected = $accounts[$accountChoice-1]
        echo $accountSelected
        $id = $accountSelected.Account

        Write-Host "User $id Selected"
        $myobjectId=(Get-AzADUser -Mail $id).Id
      }
      catch
      {
      Write-Host $_.Exception.Message
      Break
      }

    # code snippet to get Azure User Tenant Id
      # SELECT Subscriptions
        #**************************************
        try
        {
        $subslist=Get-AzSubscription
        for($i=1; $i -le $subslist.Length;$i++)
        {
           Write-Host ($i.ToString() +":"+ $subslist[$i-1].Name)
        }
        Write-Host "`nPlease pick subscription from above, e.g. 1:" -ForegroundColor DarkYellow
        [int] $selectedsub=Read-Host

        While($selectedsub -lt 1 -or $selectedsub -gt $subslist.Length)
        {
            Write-Host "incorrect input" -ForegroundColor Red
            for($i=1; $i -le $subslist.Length;$i++)
             {
              Write-Host ($i.ToString() +":"+ $subslist[$i-1].Name)
             }
            Write-Host "`nPlease pick subscription from above, e.g. 1:" -ForegroundColor DarkYellow
           [int] $selectedsub=Read-Host
        }
        if($selectedsub -ge 1 -and $selectedsub -le $subslist.Length)
        {
        $mysubid=$subslist[$selectedsub-1].Id
        $mysubName=$subslist[$selectedsub-1].Name
        $mytenantId=$subslist[$selectedsub-1].TenantId
        Write-Host "$mysubName selected"
        }
        }
        catch
        {
        Write-Host $_.Exception.Message
        Break
        }

    # Create a resource group
     Write-Host "Creating Resource Group $rgName"
     az group create --name $rgName --location $location
     Write-Host "-----------------------------------"

    # Create key vault and configure access
    New-AzResourceGroupDeployment -Name "kvdeploy$postfix" -ResourceGroupName $rgName -TemplateFile $kvTemplateJson -keyVaultName $kvname -tenantId $mytenantId -objectId $myobjectId

    Set-AzKeyVaultAccessPolicy -VaultName $kvname -ObjectId $myobjectId -PermissionsToKeys Decrypt,Encrypt,UnwrapKey,WrapKey,Verify,Sign,Get,List,Update,Create,Import,Delete,Backup,Restore,Recover,Purge -PermissionsToSecrets Get,List,Set,Delete,Backup,Restore,Recover,Purge

```

#### <a name="store-the-certificates-to-the-key-vault"></a>將憑證儲存到金鑰保存庫

使用下列指令碼，將包含在 .pfx 檔案中的憑證儲存到新的金鑰保存庫：

```PowerShell
 $fileName =$certroopath+"\$certname"+".pfx"

     $fileContentBytes = get-content $fileName -Encoding Byte
     $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

            $jsonObject = @"
    {
    "data": "$filecontentencoded",
    "dataType" :"pfx",
    "password": "$certpassword"
    }
"@
            echo $certpassword
            $jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
            $jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)
            $secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
            $objAzureKeyVaultSecret=Set-AzKeyVaultSecret -VaultName $kvname -Name "ISVSecret$postfix" -SecretValue $secret
            echo $objAzureKeyVaultSecret.Id

```

## <a name="deploy-an-azure-vm-from-your-generalized-vhd-image"></a>從一般化 VHD 映射部署 Azure VM

此節描述如何部署一般化 VHD 映像，以建立新的 Azure VM 資源。 針對此程序，我們將使用提供的 Azure Resource Manager 範本和 Azure PowerShell 指令碼。

### <a name="prepare-an-azure-resource-manager-template"></a>準備 Azure Resource Manager 範本

將下列其中一個 Azure Resource Manager 範本用於 VHD 部署（適用于 Windows 或 Linux），複製到名為 VHDtoImage.js的本機檔案。 下一個指令碼會要求本機電腦上的位置使用此 JSON。

#### <a name="for-windows-based-vms"></a>以 Windows 為基礎的 Vm

```JSON
{
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "userStorageAccountName": {
            "type": "string"
        },
        "userStorageContainerName": {
            "type": "string",
            "defaultValue": "vhds"
        },
        "dnsNameForPublicIP": {
            "type": "string"
        },
        "adminUserName": {
            "defaultValue": "isv",
            "type": "string"
        },
        "adminPassword": {
            "type": "securestring",
            "defaultValue": "Password@123"
        },
        "osType": {
            "type": "string",
            "defaultValue": "windows",
            "allowedValues": [
                "windows",
                "linux"
            ]
        },
        "subscriptionId": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "vmSize": {
            "type": "string"
        },
        "publicIPAddressName": {
            "type": "string"
        },
        "vmName": {
            "type": "string"
        },
        "virtualNetworkName": {
            "type": "string"
        },
        "nicName": {
            "type": "string"
        },
        "vaultName": {
            "type": "string",
            "metadata": {
                "description": "Name of the KeyVault"
            }
        },
        "vaultResourceGroup": {
            "type": "string",
            "metadata": {
                "description": "Resource Group of the KeyVault"
            }
        },
        "certificateUrl": {
            "type": "string",
            "metadata": {
                "description": "Url of the certificate with version in KeyVault e.g. https://testault.vault.azure.net/secrets/testcert/b621es1db241e56a72d037479xab1r7"
            }
        },
        "vhdUrl": {
            "type": "string",
            "metadata": {
                "description": "VHD Url..."
            }
        }
    },
        "variables": {
            "addressPrefix": "10.0.0.0/16",
            "subnet1Name": "Subnet-1",
            "subnet2Name": "Subnet-2",
            "subnet1Prefix": "10.0.0.0/24",
            "subnet2Prefix": "10.0.1.0/24",
            "publicIPAddressType": "Dynamic",
            "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",
            "subnet1Ref": "[concat(variables('vnetID'),'/subnets/',variables('subnet1Name'))]",
            "osDiskVhdName": "[concat('http://',parameters('userStorageAccountName'),'.blob.core.windows.net/',parameters('userStorageContainerName'),'/',parameters('vmName'),'osDisk.vhd')]"
        },
        "resources": [
            {
                "apiVersion": "2015-05-01-preview",
                "type": "Microsoft.Network/publicIPAddresses",
                "name": "[parameters('publicIPAddressName')]",
                "location": "[parameters('location')]",
                "properties": {
                    "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
                    "dnsSettings": {
                        "domainNameLabel": "[parameters('dnsNameForPublicIP')]"
                    }
                }
            },
            {
                "apiVersion": "2015-05-01-preview",
                "type": "Microsoft.Network/virtualNetworks",
                "name": "[parameters('virtualNetworkName')]",
                "location": "[parameters('location')]",
                "properties": {
                    "addressSpace": {
                        "addressPrefixes": [
                            "[variables('addressPrefix')]"
                        ]
                    },
                    "subnets": [
                        {
                            "name": "[variables('subnet1Name')]",
                            "properties": {
                                "addressPrefix": "[variables('subnet1Prefix')]"
                            }
                        },
                        {
                            "name": "[variables('subnet2Name')]",
                            "properties": {
                                "addressPrefix": "[variables('subnet2Prefix')]"
                            }
                        }
                    ]
                }
            },
            {
                "apiVersion": "2015-05-01-preview",
                "type": "Microsoft.Network/networkInterfaces",
                "name": "[parameters('nicName')]",
                "location": "[parameters('location')]",
                "dependsOn": [
                    "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]",
                    "[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]"
                ],
                "properties": {
                    "ipConfigurations": [
                        {
                            "name": "ipconfig1",
                            "properties": {
                                "privateIPAllocationMethod": "Dynamic",
                                "publicIPAddress": {
                                    "id": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]"
                                },
                                "subnet": {
                                    "id": "[variables('subnet1Ref')]"
                                }
                            }
                        }
                    ]
                }
            },
            {
                "apiVersion": "2015-06-15",
                "type": "Microsoft.Compute/virtualMachines",
                "name": "[parameters('vmName')]",
                "location": "[parameters('location')]",
                "dependsOn": [
                    "[concat('Microsoft.Network/networkInterfaces/', parameters('nicName'))]"
                ],
                "properties": {
                    "hardwareProfile": {
                        "vmSize": "[parameters('vmSize')]"
                    },
                    "osProfile": {
                        "computername": "[parameters('vmName')]",
                        "adminUsername": "[parameters('adminUsername')]",
                        "adminPassword": "[parameters('adminPassword')]",
                        "secrets": [
                            {
                                "sourceVault": {
                                    "id": "[resourceId(parameters('vaultResourceGroup'), 'Microsoft.KeyVault/vaults', parameters('vaultName'))]"
                                },
                                "vaultCertificates": [
                                    {
                                        "certificateUrl": "[parameters('certificateUrl')]",
                                        "certificateStore": "My"
                                    }
                                ]
                            }
                        ],
                        "windowsConfiguration": {
                            "provisionVMAgent": "true",
                            "winRM": {
                                "listeners": [
                                    {
                                        "protocol": "http"
                                    },
                                    {
                                        "protocol": "https",
                                        "certificateUrl": "[parameters('certificateUrl')]"
                                    }
                                ]
                            },
                            "enableAutomaticUpdates": "true"
                        }
                    },
                    "storageProfile": {
                        "osDisk": {
                            "name": "[concat(parameters('vmName'),'-osDisk')]",
                            "osType": "[parameters('osType')]",
                            "caching": "ReadWrite",
                            "image": {
                                "uri": "[parameters('vhdUrl')]"
                            },
                            "vhd": {
                                "uri": "[variables('osDiskVhdName')]"
                            },
                            "createOption": "FromImage"
                        }
                    },
                    "networkProfile": {
                        "networkInterfaces": [
                            {
                                "id": "[resourceId('Microsoft.Network/networkInterfaces',parameters('nicName'))]"
                            }
                        ]
                    },
                "diagnosticsProfile": {
                    "bootDiagnostics": {
                        "enabled": true,
                        "storageUri": "[concat('http://', parameters('userStorageAccountName'), '.blob.core.windows.net')]"
                    }
                }
                }
            }
        ]
    }

```

#### <a name="for-linux-based-vms"></a>針對以 Linux 為基礎的 Vm

```JSON
{
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "userStorageAccountName": {
            "type": "string"
        },
        "userStorageContainerName": {
            "type": "string",
            "defaultValue": "vhds"
        },
        "dnsNameForPublicIP": {
            "type": "string"
        },
        "adminUserName": {
            "defaultValue": "isv",
            "type": "string"
        },
        "adminPassword": {
            "type": "securestring",
            "defaultValue": "Password@123"
        },
        "osType": {
            "type": "string",
            "defaultValue": "linux",
            "allowedValues": [
                "windows",
                "linux"
            ]
        },
        "subscriptionId": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "vmSize": {
            "type": "string"
        },
        "publicIPAddressName": {
            "type": "string"
        },
        "vmName": {
            "type": "string"
        },
        "virtualNetworkName": {
            "type": "string"
        },
        "nicName": {
            "type": "string"
        },
        "vaultName": {
            "type": "string",
            "metadata": {
                "description": "Name of the KeyVault"
            }
        },
        "vaultResourceGroup": {
            "type": "string",
            "metadata": {
                "description": "Resource Group of the KeyVault"
            }
        },
        "certificateUrl": {
            "type": "string",
            "metadata": {
                "description": "Url of the certificate with version in KeyVault e.g. https://testault.vault.azure.net/secrets/testcert/b621es1db241e56a72d037479xab1r7"
            }
        },
        "vhdUrl": {
            "type": "string",
            "metadata": {
                "description": "VHD Url..."
            }
        }
    },
        "variables": {
            "addressPrefix": "10.0.0.0/16",
            "subnet1Name": "Subnet-1",
            "subnet2Name": "Subnet-2",
            "subnet1Prefix": "10.0.0.0/24",
            "subnet2Prefix": "10.0.1.0/24",
            "publicIPAddressType": "Dynamic",
            "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",
            "subnet1Ref": "[concat(variables('vnetID'),'/subnets/',variables('subnet1Name'))]",
            "osDiskVhdName": "[concat('http://',parameters('userStorageAccountName'),'.blob.core.windows.net/',parameters('userStorageContainerName'),'/',parameters('vmName'),'osDisk.vhd')]"
        },
        "resources": [
            {
                "apiVersion": "2015-05-01-preview",
                "type": "Microsoft.Network/publicIPAddresses",
                "name": "[parameters('publicIPAddressName')]",
                "location": "[parameters('location')]",
                "properties": {
                    "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
                    "dnsSettings": {
                        "domainNameLabel": "[parameters('dnsNameForPublicIP')]"
                    }
                }
            },
            {
                "apiVersion": "2015-05-01-preview",
                "type": "Microsoft.Network/virtualNetworks",
                "name": "[parameters('virtualNetworkName')]",
                "location": "[parameters('location')]",
                "properties": {
                    "addressSpace": {
                        "addressPrefixes": [
                            "[variables('addressPrefix')]"
                        ]
                    },
                    "subnets": [
                        {
                            "name": "[variables('subnet1Name')]",
                            "properties": {
                                "addressPrefix": "[variables('subnet1Prefix')]"
                            }
                        },
                        {
                            "name": "[variables('subnet2Name')]",
                            "properties": {
                                "addressPrefix": "[variables('subnet2Prefix')]"
                            }
                        }
                    ]
                }
            },
            {
                "apiVersion": "2015-05-01-preview",
                "type": "Microsoft.Network/networkInterfaces",
                "name": "[parameters('nicName')]",
                "location": "[parameters('location')]",
                "dependsOn": [
                    "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]",
                    "[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]"
                ],
                "properties": {
                    "ipConfigurations": [
                        {
                            "name": "ipconfig1",
                            "properties": {
                                "privateIPAllocationMethod": "Dynamic",
                                "publicIPAddress": {
                                    "id": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]"
                                },
                                "subnet": {
                                    "id": "[variables('subnet1Ref')]"
                                }
                            }
                        }
                    ]
                }
            },
            {
                "apiVersion": "2015-06-15",
                "type": "Microsoft.Compute/virtualMachines",
                "name": "[parameters('vmName')]",
                "location": "[parameters('location')]",
                "dependsOn": [
                    "[concat('Microsoft.Network/networkInterfaces/', parameters('nicName'))]"
                ],
                "properties": {
                    "hardwareProfile": {
                        "vmSize": "[parameters('vmSize')]"
                    },
                    "osProfile": {
                        "computername": "[parameters('vmName')]",
                        "adminUsername": "[parameters('adminUsername')]",
                        "adminPassword": "[parameters('adminPassword')]",
                        "secrets": [
                            {
                                "sourceVault": {
                                    "id": "[resourceId(parameters('vaultResourceGroup'), 'Microsoft.KeyVault/vaults', parameters('vaultName'))]"
                                },
                                "vaultCertificates": [
                                    {
                                        "certificateUrl": "[parameters('certificateUrl')]",
                                        "certificateStore": "My"
                                    }
                                ]
                            }
                        ],
                        "windowsConfiguration": {
                            "provisionVMAgent": "true",
                            "winRM": {
                                "listeners": [
                                    {
                                        "protocol": "http"
                                    },
                                    {
                                        "protocol": "https",
                                        "certificateUrl": "[parameters('certificateUrl')]"
                                    }
                                ]
                            },
                            "enableAutomaticUpdates": "true"
                        }
                    },
                    "storageProfile": {
                        "osDisk": {
                            "name": "[concat(parameters('vmName'),'-osDisk')]",
                            "osType": "[parameters('osType')]",
                            "caching": "ReadWrite",
                            "image": {
                                "uri": "[parameters('vhdUrl')]"
                            },
                            "vhd": {
                                "uri": "[variables('osDiskVhdName')]"
                            },
                            "createOption": "FromImage"
                        }
                    },
                    "networkProfile": {
                        "networkInterfaces": [
                            {
                                "id": "[resourceId('Microsoft.Network/networkInterfaces',parameters('nicName'))]"
                            }
                        ]
                    },
                "diagnosticsProfile": {
                    "bootDiagnostics": {
                        "enabled": false,
                        "storageUri": "[concat('http://', parameters('userStorageAccountName'), '.blob.core.windows.net')]"
                    }
                }
                }
            }
        ]
    }

```

複製並編輯下列腳本，以提供這些參數的值：

| **參數** | **說明** |
| --- | --- |
| resourceGroupName | 現有的 Azure 資源群組名稱。 通常會使用與您金鑰保存庫相同的 RG。 |
| TemplateFile | 檔案 VHDtoImage.json 的完整路徑名稱。 |
| userStorageAccountName | 儲存體帳戶的名稱。 |
| sNameForPublicIP | 公用 IP 的 DNS 名稱；必須小寫。 |
| subscriptionId | Azure 訂用帳戶識別碼。 |
| Location | 資源群組的標準 Azure 地理位置。 |
| vmName | 虛擬機器的名稱。 |
| vaultName | 金鑰保存庫的名稱。 |
| vaultResourceGroup | 金鑰保存庫的資源群組。 |
| certificateUrl | 憑證的網址 (URL)，包括儲存於金鑰保存庫的版本，例如：`https://testault.vault.azure.net/secrets/testcert/b621es1db241e56a72d037479xab1r7`。 |
| vhdUrl | 虛擬硬碟的網址。 |
| vmSize | 虛擬機器執行個體的大小。 |
| publicIPAddressName | 公用 IP 位址的名稱。 |
| virtualNetworkName | 虛擬網路的名稱。 |
| nicName | 適用於虛擬網路的網路介面卡名稱。 |
| adminUserName | 系統管理員帳戶的使用者名稱。 |
| adminPassword | 系統管理員密碼。 |
|   |   |

### <a name="deploy-an-azure-vm"></a>部署 Azure VM

複製並編輯下列指令碼，以提供 `$storageaccount` 和 `$vhdUrl` 變數的值。 執行它，從您現有的通用 VHD 建立 Azure VM 資源。

```PowerShell

# storage account of existing generalized VHD

$storageaccount = "testwinrm11815"

# generalized VHD URL
$vhdUrl = "https://testwinrm11815.blob.core.windows.net/vhds/testvm1234562016651857.vhd"

# Full pathname to the file VHDtoImage.json. inserted these highlighted lines
$templateFile = "$certroopath\VHDtoImage.json"

# Size of the virtual machine instance.
$vmSize = "Standard_D2s_v3"

# Name of the public IP address.
$publicIPAddressName = "myPublicIP1"

# Name of the virtual network
$virtualNetworkName = "myVNET1"

# Name of the network interface card for the virtual network
$nicName = "myNIC1"

# Username of the administrator account
$adminUserName = "isv"

# The OS of the virtual machine
$osType = "windows"

echo "New-AzResourceGroupDeployment -Name "dplisvvm$postfix" -ResourceGroupName "$rgName" -TemplateFile $templateFile -userStorageAccountName "$storageaccount" -dnsNameForPublicIP "$vmName" -subscriptionId "$mysubid" -location "$location" -vmName "$vmName" -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl $objAzureKeyVaultSecret.Id  -vhdUrl "$vhdUrl" -vmSize "$vmSize" -publicIPAddressName "$publicIPAddressName" -virtualNetworkName "$virtualNetworkName" -nicName "$nicName" -adminUserName "$adminUserName" -adminPassword $pwd -osType "$osType""

# deploying VM with existing VHD
New-AzResourceGroupDeployment -Name "dplisvvm$postfix" -ResourceGroupName "$rgName" -TemplateFile $templateFile -userStorageAccountName "$storageaccount" -dnsNameForPublicIP "$vmName" -subscriptionId "$mysubid" -location "$location" -vmName "$vmName" -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl $objAzureKeyVaultSecret.Id -vhdUrl "$vhdUrl" -vmSize "$vmSize" -publicIPAddressName "$publicIPAddressName" -virtualNetworkName "$virtualNetworkName" -nicName "$nicName" -adminUserName "$adminUserName" -adminPassword $pwd -osType "$osType"

```

## <a name="run-tests-on-the-deployed-vm"></a>在已部署的 VM 上執行測試

### <a name="download-and-run-the-certification-test-tool"></a>下載並執行認證測試工具

適用于 Azure 認證的認證測試控管是可在本機 Windows 電腦上執行，但會測試以 Azure 為基礎的 Windows 或 Linux VM 的自我測試控管。 其會驗證您的使用者 VM 映像是否可以搭配 Microsoft Azure 使用，以及您 VHD 的準備工作是否有依循指導方針並達到要求。 此工具可確保您的 VM 已準備好根據 Azure Marketplace 需求發佈。」

1. 下載並安裝最新版本的 [Azure 認證的認證測試工具](https://www.microsoft.com/download/details.aspx?id=44299)。
2. 開啟認證工具，然後選取 [啟動新測試]。
3. 從 [測試資訊] 畫面，輸入測試回合的**測試名稱**。
4. 選取您 VM 適用的 [平台]，亦即 Windows Server 或 Linux。 所選的平台會影響其餘的選項。
5. 如果您的 VM 使用此資料庫服務，請選取 [Azure SQL Database 測試] 核取方塊。

### <a name="connect-the-certification-tool-to-a-vm-image"></a>將認證工具連線至 VM 映像

此工具會透過 [Azure PowerShell](https://docs.microsoft.com/powershell/) 連線至以 Windows 為基礎的 VM，並透過 [SSH.Net](https://www.ssh.com/ssh/protocol/) 連線至 Linux VM。 選擇下列兩個選項的其中一個： [Linux] 或 [Windows]。

#### <a name="option-1-connect-the-certification-tool-to-a-linux-vm-image"></a>選項1：將認證工具連接到 Linux VM 映射

1. 選取 [SSH 驗證] 模式：密碼驗證或金鑰檔案驗證。
2. 如果使用以密碼為基礎的驗證，請輸入 [VM DNS 名稱]、[使用者名稱] 和 [密碼]。 您也可以變更預設的 [SSH 連接埠] 號碼。

    ![Azure 認證的測試工具，Linux VM 映像的密碼驗證](media/avm-cert2.png)

3. 如果使用以金鑰檔案為基礎的驗證，請輸入**虛擬機器 DNS 名稱**、**使用者名稱**和**私密金鑰**位置等值。 您也可以加上 [複雜密碼] 或是變更預設的 [SSH 連接埠] 號碼。

#### <a name="option-2-connect-the-certification-tool-to-a-windows-based-vm-image"></a>選項2：將認證工具連線至以 Windows 為基礎的 VM 映射

1. 輸入完整的 [VM DNS 名稱] (例如 MyVMName.Cloudapp.net)。
2. 輸入 [使用者名稱] 和 [密碼] 的值。

    ![Azure 認證的測試工具，Windows 型 VM 映像的密碼驗證](media/avm-cert4.png)

### <a name="run-a-certification-test"></a>執行認證測試

在認證工具中提供適用於您 VM 映像的參數值後，請選取 [測試連線]，以建立與 VM 的有效連線。 驗證連線後，選取 [下一步] 啟動測試。 測試完成時，測試結果會顯示在表格中。 [狀態] 欄會針對每個測試顯示 (通過/失敗/警告)。 如果任何一項測試失敗，您的映像即_未_通過認證。 在此情況下，請檢閱需求與失敗訊息、進行建議的變更，然後再執行測試一次。

自動化測試完成之後，請在 [問卷] 畫面的兩個索引標籤 ([一般評估] 和 [核心自訂]) 上，提供 VM 映像的其他相關資訊，然後選取 [下一步]。

最後一個畫面可讓您提供其他資訊，例如 Linux VM 映像的 SSH 存取資訊，以及尋找例外狀況時任何評量失敗的說明。

最後，選取 [產生報告]，以下載已執行測試案例的測試結果和記錄檔，以及問卷調查的答案。 將結果儲存在與您的 VHD 相同的容器中。

## <a name="next-step"></a>後續步驟

- [常見 SAS URL 問題和修正程式](common-sas-uri-issues.md)
