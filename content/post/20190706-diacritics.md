---
title: "Remove Diacritics"
date: 2019-07-06T09:45:53+01:00
draft: false
css: []
tags: ["powershell", "tips"]
---

If you ever faced the terror of finding a diacritic that messed up some of your
regex, or some awesome script you've written, then we are on the same boat.
Although I love my language, portuguese, messing up with scripts is never a good
way to make friends, so I often need to remove those annoying characters from
sentences and every time I need to do so I go surf the web for the same answer.
Well, here is my list of ways to remove diacritics in every language I needed.

Please, keep in mind that I've only needed a way to remove characters used in
portuguese, so all of this tools work for portuguese and were not tested for any
other language.

Powershell
----------

When automating the creation of folders in powershell, I really needed some kind
of tool to remove diacritics. This is
[where](https://lazywinadmin.com/2015/05/powershell-remove-diacritics-accents.html)
I got this tip from.

```powershell
function Remove-Diacritics {
    PARAM ([string]$String)

    $bytes = [Text.Encoding]::GetEncoding("Cyrillic").GetBytes($String)
    [Text.Encoding]::ASCII.GetString($bytes)
}
```

C#
--

This solution was very simple, it only needed some tunning of the powershell
solution.

```csharp
function Remove-Diacritics
{
    PARAM ([string]$String)
    [Text.Encoding]::ASCII.GetString([Text.Encoding]::GetEncoding("Cyrillic").GetBytes($String))
}
```
