### Bah! Humbug!

![Bahintro](https://user-images.githubusercontent.com/47631344/205985662-3f6c66a1-ef03-4b48-986c-f29db7da9314.PNG)

So all we get is

``
NY6TSMRZGIZDANRQHAYTONRWGY4DQMRZG44DOMBYHA4TCMJSHA4TKOBVGIYTONJSG4YTEMZY
GIZDSMBXHA4DKOBZHAZTKMZSGUYDQNZWGEYTGMBVGAYTSNZTHA4DIMIKMU6TMNJVGM3QUYZ
5GI2TMNBSGQ2TONZXGE2TAMRRGY3TIMJVGA4DCOBYGA3TGNRUGQYTANBTGUYTENRZG44DAOBZ
G44TGMZSGUZTKMRTHE4DAOJXG43TSNZQGIYTINRQGA2DQMJWGQ2Q====
``

Looks like standard Base64 doesn't it?

Nope that gives nothing intelligible try a few variations - multiple
encryption and ROT combinations until realise it's just Base 32 this
time for a change

Cyberchef recipe
<https://gchq.github.io/CyberChef/#recipe=From_Base32('A-Z2-7%3D',true)>

That gives us the output;

```
n=92922060817666882978708891128958521752712382290788589835325087611305019738841

e=65537

c=25642457771502167415081880736441043512697808979332535239809777970214600481645
```

The structure looks familiar.

Googling for "cipher n= e= c=" leads me to dcode.fr
<https://www.dcode.fr/rsa-cipher> and tells me that I'm looking at RSA
cipher.

Tried a few sites and then found a site I've used in previous CTFs
<https://github.com/RsaCtfTool/RsaCtfTool>

Installed that and run it using

```

python3 RsaCtfTool.py -n
92922060817666882978708891128958521752712382290788589835325087611305019738841
-e 65537 \--un

cipher
25642457771502167415081880736441043512697808979332535239809777970214600481645

```

That decrypted using fermat method

Giving me the url

httpsbit.lyomg-a-link

=

<https://bit.ly/omg-a-link>

That takes you to a Mega download site hosting JingleBells.pcap

Analysing the pcap online and with wireshark shows a bunch of zipfiles
called DeckTheHallsnnn.zip so Ineed to extract them from the Pcap file

In Wireshark;

File -\> Export Object -\> HTTP gives grinch.img

Can't mount the img file but can open it using 7zip to see the
DeckTheHalls zip files

There are 1000 of the blighters! Sorting them in the 7zip gui lets you
see that they're all identical file called download.jfif with a picture
of a cat...except for DeckTheHalls_521.zip which is larger and contains
DeckTheHalls.jpg -- this file is encrypted and needs a password.

I used zip2john to create the hash that I could use with johntheripper
or hashcat password crackers.

cttctfchall1 file

Now the catch here is that a brute force will take several days on my
machine so I tried working through a bunch of rules (based on a hint in
the discord about using the right rule). I had assumed that it would
need OneRuleToRuleThemAll.rule from
<https://github.com/NotSoSecure/password_cracking_rules> since both the
challenge and that rule are made by the same awesome individual but it
runed out to be a heck of a lot more simple (although took me approx.
100 attempts, each time aborting when I saw how long it would take!)

In the end (ie: after a day or two of trying various things) I used;

```

hashcat -m 13600 -a 0 -r rules/duplicateletters.rule cttctfchall1
/usr/share/wordlists/rockyou.txt

```

The duplicateletters.rule just contains;

```

:

q

```

(the rule just tries every word in the dictionary supplied -- rockyou of
course -- but doubles each letter)

That eventually gives the password (in less than a minute) of

\_ hhoonndduurraass11\_

the password lets you open the zipfile to access Deckthehalls.jpg

run strings on the file

```

strings Deckthehalls.jpg

```

to give you 

```

Ssh, it\'s a secret!\
Vm0wd2QyUXlWa2hWV0doVVYwZDRWRll3WkZOVU1WcHpXa2M1VjJKSGVIbFhhMXBQWVZVeFYxTnNXbFpOYmtKVVZtMTRTMk15VGtsaVJtUnBVbXR3U1ZkV1pEUlpWMDE0V2toR1UySklRazlWYWtGM1pVWmtWMWRzV214U2JIQjVWR3hhYTFsV1NuUmhSemxWVm14d2VsUlVSbXRXTVdSelYyMTRVMDFFUlRCV1ZFa3hVakZaZVZOcmFGWmlSa3BXVm10V1MxUkdjRmRYYlVacVRWWmFlVmRyV205aFZscHpZMFpvVjFKRldtaFdha1pXWlZaT2NtSkdTbWxXUjNob1ZtMTBWazFXU2tkV1dHaFlZbFZhVlZWcVJtRlRWbkJHVjJ4a1ZXSkdiRFJWTW5SdlZqRkplbUZIYUZwaGEzQk1WV3BHVDFkWFRrZFRiV3hUWWtoQ1dWWXhaRFJpTWtsNVVtdGthbEpYVWxsWmJGWmhWa1pzY2xwR1RteFdiRVkwVmpKME1GWlhTa2RqUkVaV1ZqTlNlbFl3V2xwbGJGWjBZVVprVjFKV2NEWldiWEJIVlRKT2MyTkZaR2hTTW5oWVZtMDFRMWRzV25STlZFSlhUVlV4TkZaWGRHdGhWa3BIWTBaU1dtSllUWGhXTVZwWFkxWkdkVnBHVGs1V2JrSktWa1phVTFFeVJrZFhia3BwVWtad1lWWnNaRk5UUmxsM1YyeHdiR0pHV2pGV01uaDNZVWRGZWxGcmJGZGhhMHBvVmtSS1RtVkhUa1phUjJoVFRXMW9kbGRzWkRSWlYxSnpWMjVPV2sweWFITlpXSEJIVjBaVmVXUkhkR2hpUlhCWVZqSjRWMWRzV2taalJsSlhWbFp3YUZwRlpGTlRSa3B5VGxaa2FWTkZTa3RXYTFwaFlqRlJlVkpyWkZoaWF6VnhWVzB4YjFsV2JIUk9WVTVVVW14d2VGVldhRzlXTURGeVRsVndWazFxUmtoV1ZFWkxWMVpHYzFWc2FHaE5WWEJOVmxod1IxTXlVa2RUYmtwaFVqQmFWRlJYTlc5a2JGcEhWMjA1VWsxRVFqUldNalZMVjBkS1dWVnJPVlppVkVaVVdsWmFVMVl4WkhSa1IyaFRWa1ZKTVZkV1ZtdFNNV3hYVjFod1ZtSlhhR0ZVVlZwM1YwWnJlRmRyWkZkV2EzQjZWbGN4YzFVeVNuSlNhazVYVFZaS1JGbHFSazVsUmxweldrWmthVkpzY0ZCV1YzUnJaV3M1VWxCVU1EMD0=

```

run that through cyberchef base64decode x10 then rot4

cyberchef recipe:
https://gchq.github.io/CyberChef/#recipe=From_Base64(\'A-Za-z0-9%2B/%3D\',true,false)From_Base64(\'A-Za-z0-9%2B/%3D\',true,false)From_Base64(\'A-Za-z0-9%2B/%3D\',true,false)From_Base64(\'A-Za-z0-9%2B/%3D\',true,false)From_Base64(\'A-Za-z0-9%2B/%3D\',true,false)From_Base64(\'A-Za-z0-9%2B/%3D\',true,false)From_Base64(\'A-Za-z0-9%2B/%3D\',true,false)From_Base64(\'A-Za-z0-9%2B/%3D\',true,false)From_Base64(\'A-Za-z0-9%2B/%3D\',true,false)From_Base64(\'A-Za-z0-9%2B/%3D\',true,false)ROT13(true,true,false,4)&input=Vm0wd2QyUXlVWGxXYTJoV1YwZG9WVll3WkRSV1JsbDNXa1pPVlUxV2NIcFhhMk0xVmpKS1NHVkliRmhoTVhCUVdWWlZlRll4VG5OWGJGcE9ZbXRLVlZadE1UUlRNazE1Vkd0c2FWSnRVbkJWYlhSM1UxWmtWMXBFVWxwV01ERTBWMnRvUjFVeVNrbFJhemxXWVd0R00xcFZXbXRXTVdSelYyMTRVMkpJUWpWV1IzaGhZVEZzVjFOdVVtaFNlbXhXVm0xNGQyVnNVbFZTYlhSWFRWZFNlbFl5TVRSVk1ERkZVbFJDVjFaRmEzaFZha1phWlZaT2NtRkdXbWxTYTNCWFZtMTBWMU14VWtkalJtUllZbFZhY1ZSV1dtRmxWbVJ5VjIwNWFGWnNjSHBaTUZwdlZqRktSbGR0YUZkaGExcFhXbFphVDJOdFNrZFRiV3hYVWpOb2IxWnRNVEJXYXpGWFUydGtWMWRIYUZsWmJGWmhWbFpXY1ZKdFJsUldia0pIVmpKNGExWlhTa2RpUkZKV1RXNVNkbFpxUmtwbGJVWklZVVp3YUdFelFrMVdWM0JIVkRGa1dGUnJaRlJpVjNoVVdXdG9RMWRXV1hoYVJGSnBUV3RzTlZWdGRHdGhiRXBZVld4c1dtSkdXbWhXYTFwelkyeHdSMVJ0ZUZkaVJWa3dWbXBLTUUxR1dsaFRhMlJxVWtWYVYxWnFUbE5sYkZsM1YyeHdiR0pHV2pCWlZWcHJWakZLVjJORVdsZGlXRUpJVmxSS1QyTXlUa1phUjJoVFRXNW9XVlp0TURGUk1XUnpWMjVTVGxaRlNsaFVWbFY0VGtaYVdHUkhkR2hXYTNCSVdUQmFVMWR0U2xsVVdHaFhUVlp3V0ZreFdrZGtWbkJIVkdzMVYySnJTa3RXYTFwaFZURkZlVkpyWkZoaWEzQndWV3RhZDFsV1duTmFSazVVVW14c00xWXllSGRpUjBwSFYycEdWMDF1YUROWlZXUkdaV3hHY21KR1pHaGhNSEJ2Vm10U1MxUnRWa2hVYTFwaFVqSm9WRlJYTVc5a2JHUnpXa1JTV2xZeFNucFdNalZQVjJzd2VXRklUbHBYU0VKSVZqQmFWbVZYVWtoa1IyaHBVbGhDV1ZacVNqUldNV1J6VjJ0YWFsSnNTbGhXYkZwM1lVWndSbHBHVGxSU2EzQjVWR3hhYTJGV1RrWlRhM1JYWVRGd2FGbHFSbEpsVmtweVdrWm9hV0Y2Vm5oV1Z6QjRZakZzVjJKSVVrOVdWVFZWVlcxNGQyVkdWbGRoUnpsWFRVUkdlVlJzVm5kV2F6RnhVbXRvVjFaRldreFdNVnBIWXpGV2MyRkhhRTVXV0VKT1ZteG9kMUl4VFhsVmEyUlVZbXR3YUZWcVFtRldSbEpZVGxjNWEySkdjRWhXTWpBMVZXc3hSVkZxVWxkTmFsWk1WakJrUzFkV1ZuSlBWbHBwVmtWYVZWZHNXbUZWTVZsNFdraFNhMUl5YUZSV2ExWktUVlprVjFadGRGTk5WM2hZVmpGb2QxWnRTbGhoUjBaVlZsWndNMVl3V25KbFJtUnlXa1prVjJFelFqWldiR040WXpGVmVWTnVTbE5oYXpWWVZGWmFTMUpHYkhGU2F6VnNVbXh3ZWxkcldtdGhWa3B6WTBaQ1YxWXpVbkphVjNNMVZXeENWVTFFTUQwPQ;

which outputs our flag;

Congrats! **flag{In.security - We\'re in security to prevent
insecurity}**

\*Solved!\*
