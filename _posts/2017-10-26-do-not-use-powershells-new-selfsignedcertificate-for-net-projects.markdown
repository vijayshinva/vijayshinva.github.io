---
layout: post
title:  "Do not use Powershell's New-SelfSignedCertificate for .NET projects"
date:   2017-10-26 12:00:00 +0530
categories: [ssl, .net]
tags: [ssl, .net]
---

**Powershell** introduced a new cmdlet **New-SelfSignedCertificate**, which can be used to create self-signed SSL certificates for testing and development purposes. 

The PS command below creates a new self-signed SSL certificate

``` ps
New-SelfSignedCertificate -DnsName "example.cloudapp.net" -CertStoreLocation "cert:\LocalMachine\My"
```

![New-SelfSignedCertificate](/img/posts/post-2017-10-26-New-SelfSingedCertficate.png)

This self-signed SSL certificate can then be exported out as a **.pfx** file, using the PS command below

``` ps
$pass = Convert-ToSecureString -String "password" -Force -AsPlainText
Export-PfxCertificate -Cert Cert:\LocalMachine\My\0DA8A50F8BEF89A1DE0BE59EBEADCF32813EBBFA -FilePath example.cloudapp.net.pfx -Password $pass
```

![Export-PfxCertificate](/img/posts/post-2017-10-26-Export-PfxCertificate.png)

But self-signed SSL certificates generated using **New-SelfSignedCertificate** cannot be used for **.NET** projects. If used, the code will fail with a **System.Security.Cryptography.CryptographicException** exception.

You will get exceptions like the ones below

```
Inner Exception 1:
ArgumentException: It is likely that certificate 'CN=example.cloudapp.net' may not have a private key that is capable of key exchange or the process may not have access rights for the private key. Please see inner exception for detail.

Inner Exception 2:
CryptographicException: Invalid provider type specified.
```

with a call stack similar to 

```
at System.Security.Cryptography.Utils.CreateProvHandle(CspParameters parameters, Boolean randomKeyContainer)
at System.Security.Cryptography.Utils.GetKeyPairHelper(CspAlgorithmType keyType, CspParameters parameters, Boolean randomKeyContainer, Int32 dwKeySize, SafeProvHandle& safeProvHandle, SafeKeyHandle& safeKeyHandle)
at System.Security.Cryptography.RSACryptoServiceProvider.GetKeyPair()\r\n   at System.Security.Cryptography.RSACryptoServiceProvider..ctor(Int32 dwKeySize, CspParameters parameters, Boolean useDefaultKeySize)
at System.Security.Cryptography.X509Certificates.X509Certificate2.get_PrivateKey()\r\n   at System.ServiceModel.Security.SecurityUtils.GetKeyContainerInfo(X509Certificate2 certificate)
at System.ServiceModel.Security.SecurityUtils.CanKeyDoKeyExchange(X509Certificate2 certificate)
at System.ServiceModel.Security.SecurityUtils.EnsureCertificateCanDoKeyExchange(X509Certificate2 certificate)"
``` 

The reason for this is a bit complicated and discussed [here]( https://blogs.msdn.microsoft.com/winsdk/2014/11/18/accessing-a-cng-private-key-from-an-x509certificate2-class/)

In simple terms **New-SelfSignedCertificate** creates and stores the certificate data in a certificate store (CNG Store) which .NET Security classes do not understand.

So instead of using **New-SelfSignedCertificate** for generating your self-signed SSL certificates, for **.NET** projects, use the **makecert** tool which is available from the **Developer Command Prompt for VS**

You can generate a new self-signed SSL certificate with the command below

```
makecert -r -pe -n "CN=example.cloudapp.net" -sky exchange -sv example.cloudapp.net.pvk example.cloudapp.net.cer
```

![makecert](/img/posts/post-2017-10-26-makecert.png)

You can then export out this self-signed SSL certificate as a **.pfx** file using the **pvk2pfx** command.

```
pvk2pfx -pvk example.cloudapp.net.pvk -spc example.cloudapp.net.cer -pfx example.cloudapp.net.pfx
```

![pvk2pfx](/img/posts/post-2017-10-26-pvk2pfx.png)