# Tutorial sobre configuracion de un servidor ssh y administracion de claves criptograficas  
En este Tutorial guiado aprenderá a como administrar de forma basica un servidor ssh y varias claves criptograficas en su computadora

## Administracion basica de claves criptograficas ssh

Este es un paso escencial ya que las claves criptograficas son las herramientas que utiliza este protocolo para realizar la autenticación. Utilizar la misma clave para todos los servidores a los cuales nos conectamos no es lo adecuado, ya que si se compromete la seguridad de la clave privada comprometemos la seguridad de todos los servidores.

Ahora vamos a crear los pares de llaves publicas y privadas para conectarnos a distintos servicios:

Comando para crear el par de claves:

```
ssh-keygen -t ed25519 -C "comentario-descriptivo"
```

las banderas ```-C``` y ```-t``` indican que se quiere agregar un comentario y se quiere indicar el tipo de clave.

### Ejemplo practico:

```
xavier@pop-os:~$ ssh-keygen -t ed25519 -C "proxmox-server" 

Generating public/private ed25519 key pair.
```
En el siguiente paso es importante especificar el archivo en el cual se quiere guardar la nueva clave, ya que no nos interesa sobreescribir las demas claves.

```
Enter file in which to save the key (/home/xavier/.ssh/id_ed25519): /home/xavier/.ssh/proxmox-server
```
Es importante agregar una frase para proteger aun mas la clave. Ya que si logran robar la clave privada, aun es necesario la frase para poder usarla.

```
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 

```
Si todo fue bien, deberia de salir algo asi:

```
The key fingerprint is:
SHA256:oCbx4oS4BM7kgIRZl1Sktaj9KIwTNM6DZ4ok3PQVVmM proxmox-server
The key's randomart image is:
+--[ED25519 256]--+
|.+..o++o.E       |
|=  ..+..o .      |
|++..o o.         |
|#.+=....         |
|*@B.=.  S        |
|=@.+ o           |
|* + . .          |
| . .             |
|                 |
+----[SHA256]-----+

```

Ahora vamos a copiar la clave al servidor de interés:
```
ssh-copy-id path-to-the-public-key user@servidor-de-interes.local
```
Podemos usar las banderas ```-p``` para indicar el puerto.

### Ejemplo practico

```
ssh-copy-id -i /home/xavier/.ssh/proxmox-server.pub root@192.168.1.10
```
Si todo sale bien, nos tiene que salir algo como esto:

```
xavier@pop-os:~$ ssh-copy-id -i .ssh/proxmox-server-1.pub root@192.168.1.10
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: ".ssh/proxmox-server-1.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
root@192.168.1.10's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'root@192.168.1.10'"
and check to make sure that only the key(s) you wanted were added.
```

## Configuracion basica de un servidor ssh

Hay ciertas cosas básicas que podemos configurar para que la computadora que queremos acceder de forma remota se encuentre lo mas segura posible como son las siguientes:

- Cerrar puertos que no esten en uso
- Usar claves fuertes para los usuarios
- Evitar que una gran cantidad de usuarios tengan acceso root
- Habilitar un firewall

Ahora que hacemos si queremos acceder de forma remota a nuestra computadora? Tenemos que implementar ciertas configuraciones para poder realizar labores administrativas de forma efectiva sin que esto sea una vulnerabilidad para el servidor. Por eso es necesario implementar las siguientes buenas practicas:

- Prevenir el acceso como root usando ssh
- Prevenir el acceso utilizando la contraseña de usuario
- Cambiar el puerto predeterminado