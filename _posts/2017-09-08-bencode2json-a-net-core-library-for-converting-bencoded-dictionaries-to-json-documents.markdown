---
layout: post
title:  "Bencode2Json : A .NET Core Library for converting Bencoded Dictionaries to Json Documents."
date:   2017-09-08 12:00:00 +0530
categories: [bencode, json, dotnet-core]
tags: [bencode, json, dotnet-core]
---

A BitTorrent metadata file [(.torrent)][torrent] is a Bencoded Dictionary. The file is basically a dictionary of data like the *Tracker URL* and *File Pieces* which a BitTorrent client can parse. [Bencode][bencode] is a simple 'cross-platform safe' encoding that the [(.torrent)][torrent] files use so that they can be parsed across multiple OS platforms.

[Bencode2Json][bencode2json] is a .NET Core library that can be used to convert Bencoded Dictionaries to JSON Documents. You can then query the JSON directly or use any JSON frameworks like JSON.NET and convert it into strongly typed objects.

```C#
using Bencode2Json;
...
...
// BencodedData takes any Stream
var bencodedData = new BencodedData(dataStream);
var json = bencodedData.ConvertToJson();
```

[Bencode2Json][bencode2json] is packaged as a [NuGet][nuget] package and is available in the [Nuget Gallery][nuget]

You can install it from the Package Manager Console as follows

```
PM> Install-Package Bencode2Json
```

The source code is available on [GitHub][bencode2json] and is open for contribution. Feel free to send in your code contributions via a *Pull Request* or raise an *Issue* for suggestions.

A detailed example of parsing a .torrent file is available in the Examples folder.


[![Build status](https://ci.appveyor.com/api/projects/status/fcije8tvireboq8d/branch/master?svg=true)](https://ci.appveyor.com/project/vijayshinva/bencode2json/branch/master)
[![CodeFactor](https://www.codefactor.io/repository/github/vijayshinva/bencode2json/badge)](https://www.codefactor.io/repository/github/vijayshinva/bencode2json)
[![NuGet version](https://badge.fury.io/nu/bencode2json.svg)](https://badge.fury.io/nu/bencode2json)

[bencode]: https://en.wikipedia.org/wiki/Bencode
[bencode2json]: https://github.com/vijayshinva/Bencode2Json
[torrent]: https://en.wikipedia.org/wiki/Torrent_file
[nuget]: https://www.nuget.org/packages/Bencode2Json/
