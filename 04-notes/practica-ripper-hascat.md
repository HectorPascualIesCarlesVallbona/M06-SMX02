# NOTES PRÀCTICA RIPPER HASHCAT

hashcat -m 17200 -a 0 hpascual.txt rockyou.txt
hashcat --show -m 17200 -a 0 hpascual.txt rockyou.txt

  11600 | 7-Zip                                                      | Archive
  17220 | PKZIP (Compressed Multi-File)                              | Archive
  17200 | PKZIP (Compressed)                                         | Archive
  17225 | PKZIP (Mixed Multi-File)                                   | Archive
  17230 | PKZIP (Mixed Multi-File Checksum-Only)                     | Archive
  17210 | PKZIP (Uncompressed)                                       | Archive
  20500 | PKZIP Master Key                                           | Archive
  20510 | PKZIP Master Key (6 byte optimization)                     | Archive
  23001 | SecureZIP AES-128                                          | Archive
  23002 | SecureZIP AES-192                                          | Archive
  23003 | SecureZIP AES-256                                          | Archive
  13600 | WinZip

 hashcat -m 17200 -a 0 hpascual.txt rockyou.txt

hashcat (v6.2.6) starting

OpenCL API (OpenCL 3.0 PoCL 6.0+debian  Linux, None+Asserts, RELOC, LLVM 17.0.6, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
============================================================================================================================================

- Device #1: cpu-sandybridge-AMD Ryzen 7 5700U with Radeon Graphics, 1787/3639 MB (512 MB allocatable), 2MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Optimizers applied:

- Not-Iterated
- Single-Hash
- Single-Salt

Watchdog: Temperature abort trigger set to 90c

Host memory required for this attack: 0 MB

Dictionary cache built:

- Filename..: rockyou.txt
- Passwords.: 14344392
- Bytes.....: 139921507
- Keyspace..: 14344385
- Runtime...: 2 secs

$pkzip$1*1*2*0*7e*74*380a77c8*0*4a*8*7e*93d6*2a7e4be3f4f978e2026a5856140c06cbcb2e8542b94923fdc645e8cdbc94ef5ee5b849da69b3a0f89750e6a6d4ed69e680775e49188f006bde071a74b0dbcbd091bfc4f16e024d7f0d70d8d0fce345a243e23a77060c9c899ffeeb571ecdaa4f82613c810ee9eddb74173f2ebbdb6e41f5f0ec5d957669d5df790165dddd*$/pkzip$:P@ssw0rd!

Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 17200 (PKZIP (Compressed))
Hash.Target......: $pkzip$1*1*2*0*7e*74*380a77c8*0*4a*8*7e*93d6*2a7e4b...pkzip$
Time.Started.....: Thu Oct  3 13:03:14 2024 (2 secs)
Time.Estimated...: Thu Oct  3 13:03:16 2024 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:  1489.0 kH/s (0.24ms) @ Accel:256 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 2106368/14344385 (14.68%)
Rejected.........: 0/2106368 (0.00%)
Restore.Point....: 2105856/14344385 (14.68%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#1....: PANDA12 -> Olguin!
Hardware.Mon.#1..: Util: 44%

Started: Thu Oct  3 13:03:10 2024
Stopped: Thu Oct  3 13:03:17 2024

---

─$ hashcat --show -m 17200 -a 0 hpascual.txt rockyou.txt

$pkzip$1*1*2*0*7e*74*380a77c8*0*4a*8*7e*93d6*2a7e4be3f4f978e2026a5856140c06cbcb2e8542b94923fdc645e8cdbc94ef5ee5b849da69b3a0f89750e6a6d4ed69e680775e49188f006bde071a74b0dbcbd091bfc4f16e024d7f0d70d8d0fce345a243e23a77060c9c899ffeeb571ecdaa4f82613c810ee9eddb74173f2ebbdb6e41f5f0ec5d957669d5df790165dddd*$/pkzip$:P@ssw0rd!

---

extracted:
74018d64ad8731a225e703ea3d58b086
original:
388d38c0fbd180c6e9ff1b0dbf4ac6546f2546029f83d4709e3f6d744ca8ef4f
828e950d0867725bbeda5cb879c86485

----

┌──(kali㉿kali)-[~/treball_elmeunom/john/run]
└─$ ./john --show /home/kali/treball_elmeunom/passwords-nomcognom.txt

kali:kali:1000:1000:,,,:/home/kali:/usr/bin/zsh

1 password hash cracked, 0 left

hashcat --show -m 17200 -a 0 fitxer-hash-hector-pascual.txt rockyou.txt

hashcat --show -m 17200 -a 0 fitxer-hash.txt fitxer-diccionari.txt

-----

3. Compilar john des del codi font per suportar yescrypt
Si necessites utilitzar john i vols suport per a yescrypt, pots compilar-lo des del codi font per obtenir una versió amb suport complet. Aquí tens com fer-ho:

Instal·la les eines de compilació:

bash
Copia el codi
sudo apt install build-essential libssl-dev git
Clona el repositori de john:

bash
Copia el codi
git clone <https://github.com/openwall/john> -b bleeding-jumbo john
Compila john:

bash
Copia el codi
cd john/src
./configure && make -s clean && make -sj4
Executa el john compilat:

bash
Copia el codi
./run/john --format=yescrypt /home/kali/treball_elmeunom/passwords-nomcognom.txt
Amb aquestes opcions, hauràs de ser capaç d'abordar aquest problema utilitzant la tècnica o eina més adequada per al format yescrypt. Si necessites més ajuda en algun pas, no dubtis a preguntar!
