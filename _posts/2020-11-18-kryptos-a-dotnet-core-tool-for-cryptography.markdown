---
layout: post
title:  "kryptos - A .NET core tool for cryptography."
date:   2020-11-18 12:00:00 +0530
categories: [dotnet-core, cryptography]
tags: [dotnet-core, cryptography]
---

<b>Kryptos</b> - A .NET Core tool for cryptography. A nifty CLI tool to achieve tasks like 
- decoding a JWT token or 
- generating a SHA-256 hash of a file or
- generating an UUID
- generating Message Authentication Codes etc..  

<b>.NET Core</b> introduced the concept of <b>.NET Tools</b> which are special NuGet packages that contain a console application. 

Tools hosted on NuGet can be installed easily using the <b> dotnet CLI</b> as follows


``` cmd
dotnet tool install --global kryptos
```

`--global` adds the tool to the PATH environment variable and can easily be invoked by just typing in the command

``` cmd
kryptos --help
```
<b>Kryptos</b> is a <b>.NET Core Tool</b> that helps with cryptographic tasks. It works both on Windows and Linux OS (with dotnet core v3.1 installed)

<b>Kryptos - v0.0.5</b> has the following features
- UUID generation
- Base64, Base64Url encoding and decoding
- MD5 Hash
- SHA-1, SHA-256, SHA-384, SHA-512 Hash
- JWT decoding
- HMAC-SHA1, HMAC-SHA256, HMAC-SHA384, HMAC-SHA512, HMAC-MD5

Here are a few example of using <b>kryptos</b>
1. Generate Base64 encoded string
    ```
    kryptos base64 enc -t "The quick brown fox jumps over the lazy dog."
    ```
2. Decode Base64 encoded string
    ```
    kryptos base64 dec -t "VGhlIHF1aWNrIGJyb3duIGZveCBqdW1wcyBvdmVyIHRoZSBsYXp5IGRvZy4="
    ```
3. Generate SHA-256 hash of file
    ```
    kryptos sha256 hash -i .\ubuntu-20.04-desktop-amd64.iso
    ```
4. Decode a JWT token
    ```
    kryptos jwt dec -t eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IlZpamF5c2hpbnZhIEthcm51cmUiLCJpYXQiOjE1MTYyMzkwMjIsImF1ZCI6Imh0dHBzOi8vZ2l0aHViLmNvbS92aWpheXNoaW52YS9rcnlwdG9zIn0.ufklYra5bLYKM-FWnmxI0Tsw_ILmTIDK0cJ7ZkPfwfE
    ```
<b>Kryptos</b> is under active development and new features are being added. Update to the latest version using the command below.

``` cmd
dotnet tool update --global Kryptos
```
The <b>Kryptos</b> project is available on [GitHub][git-repo]. Feel free to contribute. 

[git-repo]: https://github.com/vijayshinva/kryptos
