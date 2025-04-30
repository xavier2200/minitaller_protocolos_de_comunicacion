# Tutorial sobre configuracion de un servidor ssh y administracion de claves criptograficas  
En este Tutorial guiado aprenderá a como administrar de forma basica un servidor ssh y varias claves criptograficas en su computadora

## Configuracion del contenedor docker
Primero creamos el contenedor ejectutando el siguiente comando dentro de la carpeta donde se encuentra el archivo ```compose.yaml```

```
docker compose up --build
```

Probamos conectarnos al contenedor usando el siguiente comando:

```
ssh root@localhost -p 2222
```
**Nota:** la contraseña es ```root```

Luego cerramos la conexion colocando ```exit```

## Administracion basica de claves criptograficas ssh

Este es un paso escencial ya que las claves criptograficas son las herramientas que utiliza este protocolo para realizar la autenticación. Utilizar la misma clave para todos los servidores a los cuales nos conectamos no es lo adecuado, ya que si se compromete la seguridad de la clave privada comprometemos la seguridad de todos los servidores.

Ahora vamos a crear los pares de llaves publicas y privadas para conectarnos a distintos servidores:

Primero abrimos una nueva terminal **por facilidad es necesario estar en el directorio Home** caso contrario ejecutal ```cd```

Comando para crear el par de claves:

```
ssh-keygen -t ed25519 -C "comentario-descriptivo"
```

las banderas ```-C``` y ```-t``` indican que se quiere agregar un comentario y se quiere indicar el tipo de clave.

### Ejemplo practico:

```
xavier@pop-os:~$ ssh-keygen -t ed25519 -C "minitaller-ssh"
Generating public/private ed25519 key pair.
```
En el siguiente paso es importante especificar el archivo en el cual se quiere guardar la nueva clave, ya que no nos interesa sobreescribir las demas claves.

```
Enter file in which to save the key (/home/xavier/.ssh/id_ed25519): /home/xavier/.ssh/prueba-minitaller
```
Es importante agregar una frase para proteger la clave. Ya que si logran robar la clave privada, aun es necesario la frase para poder desencriptarla.

```
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 

```
Si todo fue bien, deberia de salir algo asi:

```
The key fingerprint is:
SHA256:9HXKucUVPf4dFjKgmFstJWOi0rg7f5PoRU2uBwv2tNg minitaller-ssh
The key's randomart image is:
+--[ED25519 256]--+
|       . + o.  ..|
|    o . = *  o oo|
|   o o o.= .. = +|
|    o  .*..o = = |
|   . o =So. + + +|
|    o B =    o  o|
|   o ..E..  .    |
|    o..+.        |
|    .o. .        |
+----[SHA256]-----+


```

Ahora vamos a copiar la clave al servidor de interés:
```
ssh-copy-id -i path-to-the-public-key root@localhost -p 22222
```
Podemos usar las banderas ```-p``` para indicar el puerto.

### Ejemplo practico

```
xavier@pop-os:~$ ssh-copy-id -p 2222 -i .ssh/prueba-minitaller.pub root@localhost
```
Si todo sale bien, nos tiene que salir algo como esto:

```
xavier@pop-os:~$ ssh-copy-id -p 2222 -i .ssh/prueba-minitaller.pub root@localhost
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: ".ssh/prueba-minitaller.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
root@localhost's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh -p '2222' 'root@localhost'"
and check to make sure that only the key(s) you wanted were added.
```
Ahora si probamos entrar usando ssh no nos deberia pedir contraseña

```
xavier@pop-os:~$ ssh root@localhost -p 2222
Welcome to Ubuntu 22.04.5 LTS (GNU/Linux 6.12.10-76061203-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

This system has been minimized by removing packages and content that are
not required on a system that users do not log into.

To restore this content, you can run the 'unminimize' command.
Last login: Wed Apr 30 00:53:51 2025 from 172.19.0.1
root@1ca64d5cff24:~# 

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