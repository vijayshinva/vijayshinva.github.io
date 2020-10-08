---
layout: post
title:  "Azure ARM : InvalidParameter certificateUrl - It should be in the format"
date:   2018-05-05 12:00:00 +0530
categories: [azure]
tags: [azure]
---

**Azure ARM templates** can refer to certificates from an **Azure Key Vault**. Earlier this required us to upload the certificate into the Key Vault as a JSON object and refer to the secret as described in [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-push-certificate-windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-push-certificate-windows) .

Secrets have an identifier URL with the format **https://\<vaultEndpoint\>/secrets/\<secretName\>/\<secretVersion\>**. For example  https://vk-test-keyvault.vault.azure.net/secrets/yourdomain-cloudapp-net/595b30046acb48e024a4fc2dbd1b7561

Azure has now introduced the concept of **Certificates** in Key Vault. You can upload PFX files directly to the Key Vault without the pain of converting them to a JSON object. 

Certificates have an identifier URL with the format **https://\<vaultEndpoint\>/certificates/\<secretName\>/\<secretVersion\>**. For example https://vk-test-keyvault.vault.azure.net/certificates/yourdomain-cloudapp-net/a86437e4906343c3a9ff48b4af7ffdbf

But you cannot refer to those certificate identifier URLs in ARM templates directly. For example, if you refer to the certificate identifier URL as follows

{% highlight json %}
"osProfile": {
    "computerNamePrefix": "[variables('namingInfix')]",
    "adminUsername": "[parameters('adminUsername')]",
    "adminPassword": "[parameters('adminPassword')]",
    "secrets": [
        {
        "sourceVault": {
            "id": "[resourceId('Test', 'Microsoft.KeyVault/vaults', 'vk-test-keyvault')]"
        },
        "vaultCertificates": [
            {
            "certificateUrl": "https://vk-test-keyvault.vault.azure.net/certificates/yourdomain-cloudapp-net/a86437e4906343c3a9ff48b4af7ffdbf"
            }
        ]
        }
    ]
},
{% endhighlight %}

You will get the following error.

{% highlight azure %}
15:53:32 - New-AzureRmResourceGroupDeployment : 03:53:32 PM - Resource Microsoft.Compute/virtualMachineScaleSets 'testvijay' 
15:53:32 - failed with message '{
15:53:32 -   "error": {
15:53:32 -     "code": "InvalidParameter",
15:53:32 -     "target": "certificateUrl",
15:53:32 -     "message": 
15:53:32 - "https://vk-test-keyvault.vault.azure.net/certificates/yourdomain-cloudapp-net/a86437e4906343c3a9ff48b4af7ffdbf is not 
15:53:32 - a valid versioned Key Vault Secret URL. It should be in the format 
15:53:32 - https://<vaultEndpoint>/secrets/<secretName>/<secretVersion>."
15:53:32 -   }
15:53:32 - }'
{% endhighlight %}

These certificates cannot be refered in ARM templates with their identifier URL. For every certficate that is uploaded, Key Vault also assigns a **SecretId**. You can list that by using the powershell cmdlet **Get-AzureKeyVaultCertificate**

{% highlight poweshell %}
PS C:\Windows\system32> Get-AzureKeyVaultCertificate -VaultName vk-test-keyvault -Name yourdomain-cloudapp-net

Name          : yourdomain-cloudapp-net
VaultName     : vk-test-keyvault
Certificate   : [Subject]
                  CN=yourdomain.cloudapp.net

                [Issuer]
                  CN=yourdomain.cloudapp.net

                [Serial Number]
                  26B84076D704B5854A28CA63491DB658

                [Not Before]
                  05-05-2018 03:04:13 PM

                [Not After]
                  05-05-2019 03:24:13 PM

                [Thumbprint]
                  BCF5F9E59C14B82C884C3AD2194395ED69E4ECE9

Id            : https://vk-test-keyvault.vault.azure.net:443/certificates/yourdomain-cloudapp-net/a86437e4906343c3a9ff48b4af7ffdbf
KeyId         : https://vk-test-keyvault.vault.azure.net:443/keys/yourdomain-cloudapp-net/a86437e4906343c3a9ff48b4af7ffdbf
SecretId      : https://vk-test-keyvault.vault.azure.net:443/secrets/yourdomain-cloudapp-net/a86437e4906343c3a9ff48b4af7ffdbf
Thumbprint    : BCF5F9E79C14B82C784C3AD2171295ED42E4ECE9
Tags          :
Enabled       : True
Created       : 05-05-2018 09:45:56 AM
Updated       : 05-05-2018 09:45:56 AM
RecoveryLevel : Purgeable
{% endhighlight %}

You can refer to the certificate using the **SecretId** URL.

{% highlight json %}
"osProfile": {
    "computerNamePrefix": "[variables('namingInfix')]",
    "adminUsername": "[parameters('adminUsername')]",
    "adminPassword": "[parameters('adminPassword')]",
    "secrets": [
        {
        "sourceVault": {
            "id": "[resourceId('Test', 'Microsoft.KeyVault/vaults', 'vk-test-keyvault')]"
        },
        "vaultCertificates": [
            {
            "certificateUrl": "https://vk-test-keyvault.vault.azure.net:443/secrets/yourdomain-cloudapp-net/a86437e4906343c3a9ff48b4af7ffdbf"
            }
        ]
        }
    ]
},
{% endhighlight %}