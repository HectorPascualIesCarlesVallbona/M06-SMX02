# PASOS

- ZIP => SO

---

## 1er: ZIP

### john

```bash
zip2john fitxer.zip > fitxer-hash-el-meu-nom-cognom.txt
```

- fitxer-hash-el-meu-nom-cognom => fitxer final on desarà el HASH del pass del ZIP
- esborrar tot l'anterior a `$pkzip` al `fitxer-hash-el-meu-nom-cognom.txt`

### hash

```bash
  hashcat -m CODI -a 0 fitxer-hash.txt fitxer-diccionari.txt
  ```

- `fitxer-hash.txt` => l'extret a
- `CODI`=>

  ```bash
  hashcat -h | grep -i zip
  ```

- veure resultat procès => ```bash hashcat --show -m CODI -a 0 fitxer-hash.txt fitxer-diccionari.txt```

### final

- descomprimim *.zip amb el pass obtingut

```bash
file confidencial.gif
```

=> confidencial.txt

- descarrega imatge disc dur i afegir a `controlador SATA`

## 2on: SO

```bash
fdisk -l
```

### muntem disc a crackejar i passem tot a carpeta temporal per treballar

- obtenim llistat dispositius i disc durs connectats a la nostra màquina
- muntem i copiem disc dur a una carpeta on podrem operar

```bash
sudo mount /dev/sdb1 /mnt
```

- agafem els passwords del SO i el diccionari i ho copiem a la nostra carpeta de pràctica

```bash
sudo cp /mnt/etc/passwd /mnt/etc/shadow RUTADETUPRACTICA
```

### unshadow

- extreiem els noms d'usuaris amb els seus hash del passwords

```bash
unshadow passwd shadow > passwords-nomcognom
```

---

### john

```bash
john --single passwords-nomcognom
```

```bash
john --wordlist=500-worst-passwords.txt --rules passwords-nomcognom
```

```bash
john --incremental:alpha passwords-nomcognom
```
