# PRACTICA01 - JHON THE RIPPER I HASCAT

[Enllaç a practica01](https://gitlab.com/pautome/m6-smxpub-2324/-/blob/main/UF1-seguretat-passiva/Reptes/01-crackpass/01.crackpass.md?ref_type=heads)

## xtra notes

┌──(kali㉿kali)-[/media/sf_ripper/test-practica01-C2]
└─$ sudo mount /dev/sdb1 /mnt

┌──(kali㉿kali)-[/media/sf_ripper/test-practica01-C2]
└─$ sudo cp /mnt/etc/passwd /mnt/etc/shadow RUTADETUPRACTICA

┌──(kali㉿kali)-[/media/sf_ripper/newTest]
└─$ unshadow passwd shadow > passwords-hpascual

┌──(kali㉿kali)-[/media/sf_ripper/newTest]
└─$ john --single passwords-hpascual  
