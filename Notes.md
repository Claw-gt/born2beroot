### Referencias

- https://42-cursus.gitbook.io/guide/rank-01/born2beroot/install-your-virtual-machine
- https://github.com/pasqualerossi/Born2BeRoot-Guide
- https://github.com/gemartin99/Born2beroot-Tutorial
- https://baigal.medium.com/born2beroot-e6e26dfb50ac

---

No GUI (*Graphical User Interface*) → Desktop Environment

### Passwords

**Root** password: VirtualMachine42

User ***clagarci*** password: MachineUser42

**Encryption** passphrase: Machine42EncriptionKey

IMPORTANTE:  tienen que incluir las restricciones de MAX DAYS y MIN DAYS. No vale solo incluir las reglas en el script

sudo chage -l root

sudo chage -l clagarci

En el subject: “**El hostname de tu máquina virtual debe ser tu login terminado en 42.** Deberás modificar este hostname durante tu evaluación”

hostname → **clagarci42**

***hostnamectl***

Para cambiar de hostname:

***sudo hostnamectl set-hostname new_hostname  ///***

Change /etc/hosts file

```
$ sudo nano /etc/hosts
```

Change old_hostname with new_hostname:

```
127.0.0.1       localhost
127.0.0.1       new_hostname
```

***sudo reboot***

- Word that identifies your system to the network

En el subject: “Además del usuario root, **un usuario con tu login como nombre debe existir**.”

non-administrative user → **clagarci**

# Cambiar permisos sgoinfre

Cambiar permisos con *chmod:*

cambiar permisos con “chmod ugo test.txt” donde “ugo” corresponde a números según los permisos.

El 1er numero (u)→ usuario/owner

El 2o numero (g) → grupo

El 3er numero (o) → others

r - 4 (read)

w - 2 (write)

x - 1 (execute)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0bdd19dc-786b-44f8-b15e-73b0b04eff47/d5c2a977-0568-4057-bf3c-541b481ed335/Untitled.png)

**→ chmod 700 sgoinfre**

# Comandos

sudo (**Su**peruser **Do**) es una herramienta de sistema que permite a los usuarios realizar la ejecución como super-usuario u otro usuario de acuerdo a como se especifique en el archivo **/etc/sudoers. It enhances security by providing a controlled way to perform administrative tasks without logging in as the root user.**

### Ir a root

su

### Ver particiones

lsblk

### Comprobar usuarios en grupo

**getent group *group***

nano /etc/group

### Instalar sudo

apt install sudo

Verificar con:

***dpkg -l (muestra todos los paquetes) OR which sudo OR sudo -v***

### Mostrar argumentos de sudo y redireccionarlo a un archivo

sudo -V > file.txt

### Reiniciar máquina

sudo reboot

### Añadir usuario a un grupo

**sudo adduser *login***

sudo addgroup *grupo* (GID → Group ID)

**sudo adduser *login group //* usermod -a-G *group user*** 

- The *a* (append) flag tells usermod to add a user to a group.
- The *G* flag specifies the name of the secondary group to which you want to add the user.

**sudo visudo to enter /etc/sudoers file**
Lastly find - # User privilege specification, type `your_username  	ALL=(ALL) ALL`

### Actualizar repositorio (etc/apt/sources.list)

sudo apt update

## SSH

### Instalar SSH

sudo apt install openssh-server

### Comprobar estado de SSH y reiniciar

**sudo service ssh status**

sudo service ssh restart

### Config de SSH

nano /etc/ssh/sshd_config

## UFW

### Instalar UFW

sudo apt install ufw

### Habilitar

sudo ufw enable

### Permitir conexiones mediante puerto 42

sudo ufw allow 4242 

### Ver status

**sudo ufw status**

***ss -tunlp***

## Política de contraseñas para sudo

touch /etc/sudoers.d/sudo_config

**“Autenticarte con sudo debe estar limitado a tres intentos en el caso de introducir
una contraseña incorrecta.”**

passwd_tries=3

**“Para cada comando ejecutado con sudo, tanto el input como el output deben quedar
archivados en el directorio /var/log/sudo/.”**

`mkdir /var/log/sudo` 

**“Para cada comando ejecutado con sudo, tanto el input como el output deben quedar
archivados en el directorio /var/log/sudo/.”**

logfile=”/var/log/sudo/sudo_config”

long_input, log_output

iolog_dir=”/var/log/sudo”

**“El modo TTY debe estar activado por razones de seguridad.”**

requiretty

(Es obligatorio el acceso desde una terminal (**TTY**) para poder ejecutar **sudo**)

**“Por seguridad, los directorios utilizables por sudo deben estar restringidos. Por
ejemplo:**

**/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin”**

Busca en dichos directorios el comando. Restringes la búsqueda a ellos. *sudo apt* 

secure_path=”/usr/local/sbin:/usr/local/bin”

## Política de contraseñas fuerte

**nano /etc/login.defs // sudo chage -l *username***

![Screen Shot 2024-04-05 at 4.08.04 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0bdd19dc-786b-44f8-b15e-73b0b04eff47/e0d568eb-85de-46b5-937e-0007abbcfd91/1dfde1f0-db75-4d9b-b28c-fa24e4b3d009.png)

apt install libpam-pwquality

**nano /etc/pam.d/common-password**

 (si no estás en *root* usar **sudo**)

> minlen=10 ➤ La cantidad mínima de caracteres que debe contener la contraseña.
> 

> ucredit=-1 ➤ Como mínimo debe contener una letra mayúscula. Ponemos el - ya que debe contener como mínimo un carácter, si ponemos + nos referimos a como máximo esos caracteres.
> 

> dcredit=-1 ➤ Como mínimo debe contener un dígito.
> 

> lcredit=-1 ➤ Como mínimo debe contener una letra minúscula.
> 

> maxrepeat=3 ➤ No puede tener más de 3 veces seguidas el mismo carácter.
> 

> reject_username ➤ No puede contener el nombre del usuario.
> 

> difok=7 ➤ Debe tener al menos 7 caracteres que no sean parte de la antigua contraseña.
> 

> enforce_for_root ➤ Implementaremos esta política para el usuario root.
> 

Revisar política de contraseñas de un usuario concreto:

**sudo chage -l *login***

| ***sudo nano /etc/pam.d/common-password*** | check the password rules |
| --- | --- |
| ***sudo nano /etc/login.defs*** | check password expiration rules |
| ***sudo chage -l username*** | check password policy for specific user |
| ***sudo ufw status*** | check ufw status |
| ***sudo systemctl status ssh*** | check ssh status |
| ***cat /etc/os-release*** | check the chosen OS |
| ***getent group sudo(user42)*** | check users belonging in specific groups |
| ***sudo addgroup new_groupname*** | create a new group |
| ***sudo adduser new_username*** | add new user |
| ***sudo usermod -aG group name username*** | add a user to the group |
| ***lsblk*** | check partitions |
| ***hostnamectl*** | check host information |
| ***sudo hostnamectl set-hostname new_hostname*** | change the hostname |
| ***sudo reboot*** | to restart the machine |
| ***dpkg -l | grep sudo  // sudo --version*** | check installation status of sudo |
| ***sudo nano /etc/sudoers*** | check sudo rules |
| ***cd /var/log/sudo/*** | check sudo log |
| ***sudo ufw allow 8080*** | add new port rule to ufw |
| ***sudo ufw status numbered*** | check the open ports |
| ***sudo ufw delete number*** | delete the port |
| ***sudo service ssh status*** | check ssh status |
| ***ssh new_user@127.0.0.1 -p 4242*** | login with a new user with ssh |
| ***cd /usr/local/bin && vim monitoring.sh*** | check the monitoring script |
| ***sudo crontab -u root -e (***change 10 value to 1***)*** | change the display of the script |
| ***sudo /etc/init.d/cron stop*** | stop the script from running |
| ***sudo /etc/init.d/cron start*** | launch the script again |
| ***sudo crontab -u root -e*** | check the script running rules and modify the cron |
| ***/usr/sbin/aa-status*** | check AppArmor status |
| ***ss -tunlp*** | Check LISTENING ports |

# Particiones

Disk *partitioning* is the creation of one or more storage regions (called *partitions*), so that each region can be managed separately.

Each OS has a different way to designate partitions. On Linux (and thus Debian or CentOS) they are designated like that: sdXN, with X a letter representing the medium and N the number of the partition on the medium (for example sdb3 for the third partition of disk b).

Partitioning offers many advantages in terms of security.

It is common practice to reserve partitions for services that can generate a lot of volume in order to avoid saturating the system partitions.

Here is a short list of partitions that may exist (and that we use in this project):

| Name |  |
| --- | --- |
| / | Contains the rest of the tree. |
| /boot | Contains data that is used before the kernel begins executing user-mode programs |
| /var | Contains variable files |
| /tmp | Contains temporary files |
| /home | Contains HOME users |

And what's LVM ? You can think of LVM as "dynamic partitions", meaning that you can create/resize/delete LVM "partitions" (they're called "Logical Volumes" in LVM-speak) from the command line while your Linux system is running: no need to reboot the system to make the kernel aware of the newly-created or resized partitions.

---

A partition is a logical division ***of a hard disk that is treated as a separate unit by operating systems (OSes) and file systems***

Las particiones de un disco en sistemas operativos basados en Unix, como Linux, se utilizan para separar física o lógicamente distintas secciones del disco duro con el fin de mejorar el rendimiento, la seguridad y la organización de los datos. Cada partición puede contener un sistema de archivos diferente o servir para distintos propósitos en el manejo del sistema. A continuación, se describen algunos de los usos más comunes de las particiones como **`/home`**, **`/var`**, y otros:

### **`/` (Raíz)**

Esta es la partición raíz del sistema. Contiene todos los archivos y directorios del sistema operativo y los programas instalados, excepto aquellos que se asignan a otras particiones. Es imprescindible para arrancar el sistema.

### **`/home`**

- **Usuarios**: Aloja los directorios personales de los usuarios del sistema. Separar **`/home`** en una partición diferente permite mantener los archivos personales de los usuarios incluso si el sistema operativo necesita ser reinstalado o actualizado. Facilita también hacer copias de seguridad y la gestión de datos de usuario.

### **`/var`**

- **Datos Variables**: Contiene archivos cuyo contenido se espera que crezca de manera dinámica, como logs del sistema (**`/var/log`**), colas de impresión (**`/var/spool`**), y cachés temporales de aplicaciones (**`/var/cache`**). Una partición separada para **`/var`** puede prevenir que un incremento inesperado de datos (como un log que crece rápidamente) llene el disco, lo que podría afectar al funcionamiento del sistema.

### **`/boot`**

- **Arranque del Sistema**: Contiene los archivos necesarios para iniciar el arranque del sistema, incluyendo el kernel de Linux y el gestor de arranque. Tener **`/boot`** en una partición separada asegura que el gestor de arranque pueda encontrar estos archivos incluso si el resto del sistema de archivos no es accesible.

### **`/tmp`**

- **Archivos Temporales**: Usada para almacenar archivos temporales creados por aplicaciones y el sistema. Separar **`/tmp`** en su propia partición puede mejorar la seguridad y evitar que el sistema se bloquee si se llenase completamente el almacenamiento con archivos temporales.

### **`/usr`**

- **Programas de Usuario**: Tradicionalmente contiene la mayoría de los programas y bibliotecas del sistema. En sistemas modernos, **`/usr`** puede ser parte de la partición raíz sin necesidad de separarlo, aunque en sistemas de gran tamaño o en servidores puede ser útil tenerlo en una partición separada para gestión de actualizaciones o para configuraciones de solo lectura.

A conseguir:

![Screen Shot 2024-04-05 at 11.21.18 AM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0bdd19dc-786b-44f8-b15e-73b0b04eff47/86882b59-9bf1-46a7-8630-63b80b2ff691/Screen_Shot_2024-04-05_at_11.21.18_AM.png)

Separadas las particiones /home, swap

LVM → Logic Volume Manager

“After the LVM is configured, no additional changes to the partitioning scheme of disks containing physical volumes are allowed during the installation” 

LVM (Logical Volume Management) is a way to manage large amounts of disk space on Linux systems. It allows you to create, resize, and move logical volumes (partitions) without having to format the entire physical hard drive. This makes it a more flexible and efficient way to manage disk space for growing workloads and changing requirements.
Imagine a large storage room filled with many smaller boxes. LVM lets you group these boxes together to create larger compartments, which you can then rearrange as needed. This makes it easier to use the available space efficiently and adapt to changing storage needs.

SCSI2 (sda) → Harddisk to partition

“/dev/sda is **the first hard drive found**, /dev/sdb is the second hard drive found by the sd-bus driver when the system boots up, or when the system scans for removable devices. **“Hard drive” can be a SCSI, SATA, or a USB device**. SSDs are usually accessed through an NVME port and given /dev/nvme0, /dev/nvme1, etc.” 

“**The term sd stands for SCSI disk**. In most cases **the OS disk is /dev/sda** and in some odd cases it can be /dev/sdb also. This is not controlled by any Azure specific thing. This is a default Linux behavior.”

En el subject: **“Debes crear al menos 2 particiones cifradas usando LVM.”**

Partitioning method:

Guided - use entire disk and set up encrypted LVM

Cantidad de volumen empleada en la partición guiada: 12.4 GB (max)

![Screen Shot 2024-04-01 at 3.03.47 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0bdd19dc-786b-44f8-b15e-73b0b04eff47/3ca0e085-f756-47eb-a22d-178d71fb6c28/Screen_Shot_2024-04-01_at_3.03.47_PM.png)

# Debian Archive Mirror

# Debian Mirrors

> *Debian mirrors are maintained by volunteers. If you are in the position to donate disk space and connectivity, please consider creating a mirror to make Debian more accessible. For more information, please visit [this page](https://www.debian.org/mirror/ftpmirror).* Debian is distributed (aka mirrored) on hundreds of servers worldwide, all offering the same content. This way we can provide better access to our archive.
> 

> It’s a server that mirrors all of the Debian package files. Debian and relatives (Ubuntu, Mint, etc) use a package manager that works with .deb format files. The package managers (apt-get and aptitude are the most well known) download .deb files on demand from the internet. The sites that house all of the packages are called “archive mirrors”. You generally want to pick a fast one close to you so that package installs go quickly.
> 
> 
> 
> *From [https://www.quora.com/What-is-a-Debian-archive-mirror-in-the-Linux-installation-process#:~:text=It's a server that mirrors,on demand from the internet](https://www.quora.com/What-is-a-Debian-archive-mirror-in-the-Linux-installation-process#:~:text=It's%20a%20server%20that%20mirrors,on%20demand%20from%20the%20internet).*
> 

Choose as Debian archive mirror [deb.debian.org](http://deb.debian.org) 

> Is a newer service that uses a content delivery network (CDN) to distribute packages. This means that when you connect to [deb.debian.org](https://deb.debian.org/), **you are automatically directed to a server that is geographically close to you, which can result in faster download speeds**.
> 
> 
> 
> See https://superuser.com/questions/1830232/whats-the-difference-between-security-debian-org-debian-security-and-deb-debian
> 

*deb is the format, as well as filename extension of the software package format for the Debian Linux distribution and its derivatives*

# GRUB boot loader

**GNU GRUB** (*GNU GRand Unified Bootloader*) es un [cargador de arranque](https://es.wikipedia.org/wiki/Cargador_de_arranque) múltiple, desarrollado por el proyecto [GNU](https://es.wikipedia.org/wiki/GNU) que nos permite elegir qué Sistema Operativo arrancar de los instalados. Se usa principalmente en sistemas operativos [GNU/Linux](https://es.wikipedia.org/wiki/GNU/Linux)

Cuando se enciende un equipo, el proceso de arranque se inicia con el firmware, que generalmente es el [BIOS](https://es.wikipedia.org/wiki/BIOS) o el [UEFI](https://es.wikipedia.org/wiki/UEFI), realizando una serie de pruebas de autodiagnóstico y cargando el gestor de arranque. El gestor de arranque, en este caso GRUB, permite seleccionar el sistema operativo a cargar, como GNU/Linux.

Una vez seleccionado el sistema operativo, el gestor de arranque carga el kernel (núcleo) en la memoria y le pasa el control

From: https://es.wikipedia.org/wiki/GNU_GRUB

Se guarda el gestor de arranque (boot loader) en un dispositivo “arrancable”, como el disco duro /dev/sda

# Elegir Sistema Operativo: Debian vs Rocky

`Debian` was one of the first Linux distributions and has been available since 1993. Debian offers a higher degree of control and customization of its configuration. 

***Debian** is a more beginner-friendly operating system. It has a simpler installation process and a more user-friendly interface. This can make it easier for you to learn the basics of using a virtual machine. It’s free and has a lot of packages, which are easy to install and use for people without experience. For your first virtual machine experiment as a beginner, Debian is a favorable choice due to its user-friendly nature and extensive documentation. Debian's straightforward installation process, large package repository, and strong community support make it an ideal platform for learning and experimenting with virtualization, providing a more accessible experience for beginners*
 **Debian tends to be favored by users who value a large number of available packages and a commitment to free software and is easily updated**

Rocky Linux is a good choice for users who want a stable, reliable, and performant Linux distribution that is compatible with RHEL. Debian is a good choice for users who want a more flexible and customizable Linux distribution that offers a wider variety of packages.

things to consider when choosing between Rocky Linux and Debian:

- Your needs and preferences: What features are important to you? What kind of software do you need?
- Your experience level: Are you familiar with Red Hat Enterprise Linux (RHEL)?
- Your budget: Are you willing to pay for commercial support?

***cat /etc/os-release OR hostnamectl***

# Máquina Virtual

Virtual machines are like having multiple computers running on one physical computer. Each virtual machine has its own operating system and software(own CPU, memory, network interface and storage), but they all share the same hardware. This is useful for running multiple operating systems at the same time, or for testing software without affecting your main system.The purpose of virtual machines is to create a virtual environment that is isolated from the physical computer. This allows you to run different operating systems, software, and configurations without affecting the host machine. Virtual machines are also useful for testing software on different environments, or for running old software that is not compatible with newer operating systems. It is commonly used for isolation, resource consolidation, testing, and development, providing flexibility and security benefits.

1. Resource isolation: Virtual machines provide isolation between different virtual environments, preventing software conflicts and security risks.
2. Hardware independence: Virtual machines can run on different hardware platforms, ensuring compatibility and flexibility.
3. Scalability: Virtual machines can be easily scaled up or down to meet changing resource requirements.
4. Cost-effectiveness: Virtual machines can consolidate multiple workloads onto a single physical host, reducing hardware costs.
5. Testing and development: Virtual machines are ideal for testing and developing software without disrupting production environments.
6. Ease of use: Virtual machines are relatively easy to manage and deploy, even for non-technical users

# AppArmor VS SELinux

**APPArmor** is like a security guard for your computer. It watches over the programs (apps) and makes sure they behave and don't go where they shouldn't. It sets up rules, a bit like telling each app where it's allowed to go and what it's allowed to do. This way, even if an app misbehaves, APPArmor helps keep everything under control, adding an extra layer of protection to your computer. It uses pathnames, no labels necessary. 

***/usr/sbin/aa-status***

# apt VS aptitude

In simple terms, **apt and aptitude** are tools that help you manage software on your computer. **Apt** is like a basic tool that does the job efficiently, while aptitude is a bit more advanced, offering additional features for managing software in a slightly fancier way. If you're just getting started, using apt (the basic tool) is usually more than enough. **Aptitude** is a graphical user interface (GUI) front-end for the apt package manager, providing a more user-friendly interface for managing packages on Debian-based distributions. apt is a command-line tool that allows you to install, remove, update, and search for packages on Debian-based systems.While **apt-get** handles all the package installation, up-gradation, system-upgradation, purging package, resolving dependencies etc., Aptitude handles a lot more stuff than apt, including functionalities of **apt-mark** and **apt-cache** i.e. searching for a package in list of installed packages, marking a package to be automatically or manually installed, holding a package making it unavailable for up-gradation and so on.

- Aptitude is a high-level package manager while APT is lower level which can be used by other higher level package managers
- Aptitude is smarter and will automatically remove unused packages or suggest installation of dependent packages
- Apt will only do explicitly what it is told to do in the command line

If you consider only the command-line interfaces of each, they are quite similar as each of them offers you different ways to manage your packages. Therefore, there are a few differences that we can list:

- Apt offers a command-line interface, while aptitude offers a visual interfaceWhen facing a package conflict, `apt` will not fix the issue while `aptitude` will suggest a resolution that can do the jobaptitude can interactively retrieve and displays the Debian changelog of all available official packages

Apt requires the user to have a solid knowledge of Linux systems and package management as you are running everything in the command line. It can be difficult for a novice to handle.

On the other hand, aptitude with its interface is more user-friendly as it offers a layer of abstraction regarding the different sub-commands to use for installation, upgrades, etc.

# SSH

SSH (Secure Shell) version 2 [RFC4250, RFC4251, RFC4252, RFC4253, RFC4254] is a cryptographic network protocol used to encrypt information sent over an insecure network such as the Internet. In essence, it relies on a client-server model to allow a user on one computer to remotely log-in, send commands, and transfer files on another computer, without compromise of data integrity or confidentiality.

Es el nombre de un protocolo y del programa que lo implementa cuya principal función es el acceso remoto a un servidor por medio de un canal seguro en el que toda la información está cifrada.

Instalaremos la herramienta principal de conectividad para el inicio de sesión remoto con el protocolo SSH, esta herramienta es OpenSSH. 

### Instalar

sudo apt install openssh-server

### Ver si está corriendo correctamente:

sudo service ssh status

### Cambiar archivos de configuración

/etc/ssh/sshd_config

![Screen Shot 2024-04-05 at 3.34.21 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0bdd19dc-786b-44f8-b15e-73b0b04eff47/99b0ba15-ffdb-4965-ab19-6e354746ea3d/Screen_Shot_2024-04-05_at_3.34.21_PM.png)

/etc/ssh/ssh_config

![Screen Shot 2024-04-05 at 3.35.56 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0bdd19dc-786b-44f8-b15e-73b0b04eff47/0c34d6e7-bdf2-419f-bdf9-74434c2dd883/Screen_Shot_2024-04-05_at_3.35.56_PM.png)

### Reiniciar para que se actualicen los cambios

sudo service ssh restart

![Screen Shot 2024-04-05 at 3.39.44 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0bdd19dc-786b-44f8-b15e-73b0b04eff47/1da92ce5-80c8-41f6-b8fc-c088d3735178/Screen_Shot_2024-04-05_at_3.39.44_PM.png)

### Conectarse a la máquina virtual recién creada

ssh [clagarci@localhost](mailto:clagarci@localhost) -p 4242

ssh [clagarci@](mailto:clagarci@localhost)127.0.0.1 -p 4242

The term localhost is usually used to refer to the local computer with the loopback address 127.0. 0.1. 

*nano /etc/hosts*

![Screen Shot 2024-04-14 at 12.27.13 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0bdd19dc-786b-44f8-b15e-73b0b04eff47/4a5311b0-7285-426d-97d3-a70d4d2d0265/Screen_Shot_2024-04-14_at_12.27.13_PM.png)

### Desconectarse de localhost

Desde terminal conectada:

*exit*

Desde máquina virtual:

- Ver quién está conectado con *who*

![Screen Shot 2024-04-05 at 4.28.39 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0bdd19dc-786b-44f8-b15e-73b0b04eff47/04b6086b-8272-4ccd-9214-fd4eb946ea69/Screen_Shot_2024-04-05_at_4.28.39_PM.png)

- Identificar el PID de su shell con  *ps -ax \ grep pts/0*
    
    *ps* para ver la info de todos los procesos
    
    ### **Opción ps -a**
    
    Esta opción permite obtener los procesos de todos los usuarios del sistema, pero hay que combinarlo con otras opciones como “x”. En caso contrario tan solo dará una breve información sobre los procesos del usuario que ha lanzado el comando y poco más
    
    ![Screen Shot 2024-04-05 at 4.30.14 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0bdd19dc-786b-44f8-b15e-73b0b04eff47/e75eee88-b3ab-4731-887d-b2c4d27799fd/Screen_Shot_2024-04-05_at_4.30.14_PM.png)
    
- Desconectar con *kill -9 PID*
    
    Sends a SIGKILL signal to a service, shutting it down immediately
    
    ![Screen Shot 2024-04-05 at 4.31.18 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0bdd19dc-786b-44f8-b15e-73b0b04eff47/ee1cf3bf-65e4-4793-90a2-953e7e31e212/Screen_Shot_2024-04-05_at_4.31.18_PM.png)
    

# Firewall UFW

 Es un [firewall](https://es.wikipedia.org/wiki/Cortafuegos_(inform%C3%A1tica)) el cual utiliza la línea de comandos para configurar las [iptables](https://es.wikipedia.org/wiki/Iptables) usando un pequeño número de comandos simples.

UFW, or Uncomplicated Firewall, is a user-friendly command-line tool for managing netfilter, the firewall application in Linux. It simplifies the process of configuring a firewall, allowing users to define rules to control incoming and outgoing network traffic. UFW enhances security by providing an accessible way to manage firewall settings without the need for complex configurations.UFW (Uncomplicated Firewall) is a simple firewall tool for Linux systems that allows you to control incoming and outgoing network traffic. It is designed to be easy to use, even for users with limited technical knowledge.

### Instalar

sudo apt install ufw

### Habilitar

sudo ufw enable

### Permitir conexiones mediante puerto 42

sudo ufw allow 4242 

### Ver status

sudo ufw status

![Screen Shot 2024-04-05 at 3.47.06 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0bdd19dc-786b-44f8-b15e-73b0b04eff47/d6552111-bb02-4869-b52e-74c44ef3512f/Screen_Shot_2024-04-05_at_3.47.06_PM.png)

***sudo ufw allow 8080 // sudo ufw deny 8080 // sudo ufw delete allow 8080***

# Script

![Screen Shot 2024-04-07 at 7.15.09 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0bdd19dc-786b-44f8-b15e-73b0b04eff47/b6a35527-db5a-44bc-a657-1b332b0040ad/Screen_Shot_2024-04-07_at_7.15.09_PM.png)

Es una secuencia de comandos guardada en un fichero que cuando se ejecuta hará la función de cada comando.

### Arquitectura

**“La arquitectura de tu sistema operativo y su versión de kernel”**

**uname -a (donde ‘-a’ = ‘—all’)**

![Screen Shot 2024-04-07 at 6.13.04 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0bdd19dc-786b-44f8-b15e-73b0b04eff47/a9ff19db-3df7-4887-991b-4ca15d570c0e/Screen_Shot_2024-04-07_at_6.13.04_PM.png)

- INFO:
    
    Si utilizamos el comando sin argumentos nos entregara la palabra Linux extraída de la información del Kernel. Nos mostrará el kernel que estamos usando
    
    `$ uname`
    
    ## **¿Cómo obtener la información de la versión kernel de Linux?**
    
    Si deseamos **extraer la información de la versión del kernel** utilizamos el parámetro **-r**.
    
    `$ uname -r
    4.15.0-74-generic`
    
    ## **¿Cómo obtener la fecha de la versión del kernel de Linux?**
    
    Si deseamos **extraer la fecha de cuando la versión del kernel** fue liberada utilizamos el parámetro **-v**.
    
    `$ uname -v
    #84-Ubuntu SMP Thu Dec 19 08:06:28 UTC 2019`
    
    ## **¿Cómo obtener la información de la arquitectura de la distribución Linux instalada?**
    
    Para **conocer la arquitectura de nuestro sistema operativo**, ya sea 36 o 64 bytes, utilizamos el parámetro **-m**.
    
    `$ uname -m
    x86_64`
    
    ## **¿Cómo obtener la información del procesador del sistema en Linux?**
    
    Para **mostrar la información del procesador** del equipo en el cual se ejecuta nuestro sistema operativo Linux, utilizamos el parámetro **-p**.
    
    `$ uname -p
    x86_64`
    
    ## **¿Cómo obtener el nombre de la distribución de Linux instalada?**
    
    Para **obtener la información del nombre de la distribución de Linux** que estamos utilizando, utilizamos el parámetro **-**. Para mostrar el sistema operativo
    
    `$ uname -o
    GNU/Linux`
    
    Para **mostrar el nombre del equipo**, podemos usar el modificador **-n**, que muestra el nombre del ***host* en esta red**:
    
    `$ uname -n`
    
    `clagarci42`
    
    ## **¿Cómo obtener la información detallada del sistema incluyendo versión del kernel y distribución Linux, plataforma, etc?**
    
    Para obtener la información completa en un solo parámetro, usamos el parámetro **-a**.
    
    `$ uname -a
    Linux predator 4.15.0-74-generic #84-Ubuntu SMP Thu Dec 19 08:06:28 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux`
    

### Núcleos físicos

**“El número de núcleos físicos.”**

- INFO
    
    Buscar fichero */proc/cpuinfo* que proporciona info sobre el procesador
    
    ![Screen Shot 2024-04-07 at 6.21.55 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0bdd19dc-786b-44f8-b15e-73b0b04eff47/acc626bc-1f39-4333-ad95-834720663c3c/Screen_Shot_2024-04-07_at_6.21.55_PM.png)
    
    - wc -l cuenta concretamente líneas. Cuando hay más de un núcleo muestra la información por separado, uno por línea. En este caso solo hay uno y empieza a contar desde 0.
    - grep busca una palabra
    - sort -u para incluir solo id únicas. Por si añades otro procesador virtualizado

**grep “physical id” /proc/cpuinfo | sort -u | wc -l**

Las comillas son necesarias porque hay un espacio entre las palabra. Quieres buscar toda la palabra junta, no solo “physical”.

### Núcleos virtuales

**“El número de núcleos físicos.”**

**grep processor /proc/cpuinfo | wc -l**

### Memoria RAM

**“La memoria RAM disponible actualmente en tu servidor y su porcentaje de uso”**

- INFO
    
    *free* muestra información sobre la RAM
    
    ![Screen Shot 2024-04-07 at 6.33.47 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0bdd19dc-786b-44f8-b15e-73b0b04eff47/57de1fc1-8ec3-4a6a-994e-54b2b5d77365/Screen_Shot_2024-04-07_at_6.33.47_PM.png)
    
    —mega para mostrarlo en megabytes
    
    **free —mega**
    
    ---
    
    *awk*  para procesar datos basados en archivos de texto
    
     **Default behavior of Awk:** By default *awk* prints every line of data from the specified file. 
    `awk '{print}' employee.txt`
    
    **Print the lines which match the given pattern.**
    
    `awk '/manager/ {print}' employee.txt`
    
    **Splitting a Line Into Fields :** For each record i.e line, the awk command splits the record delimited by whitespace character by default and stores it in the $n variables. If the line has 4 words, it will be stored in $1, $2, $3 and $4 respectively. Also, $0 represents the whole line.
    
    `awk '{print $1,$4}' employee.txt`
    
    **Built-In Variables In Awk**
    
    Awk’s built-in variables include the field variables—$1, $2, $3, and so on ($0 is the entire line) — that break a line of text into individual words or pieces called fields.
    
    - **NR:** NR command keeps a current count of the number of input records. Remember that records are usually lines. Awk command performs the pattern/action statements once for each record in a file.
    - **NF:** NF command keeps a count of the number of fields within the current input record.
    - **FS:** FS command contains the field separator character which is used to divide fields on the input line. The default is “white space”, meaning space and tab characters. FS can be reassigned to another character (typically in BEGIN) to change the field separator.
    - **RS:** RS command stores the current record separator character. Since, by default, an input line is the input record, the default record separator character is a newline.
    - **OFS:** OFS command stores the output field separator, which separates the fields when Awk prints them. The default is a blank space. Whenever print has several parameters separated with commas, it will print the value of OFS in between each parameter.
    - **ORS:** ORS command stores the output record separator, which separates the output lines when Awk prints them. The default is a newline character. print automatically outputs the contents of ORS at the end of whatever it is given to print.
    
    **free —mega | awk ‘$1 == “Mem:” {print $3}**
    
    - Imprime la información sobre la RAM en MB (*free —mega*)
    - Escoge la línea cuya primera palabra es “Mem” (*$1 == “Mem”)*
    - Imprime la memoria usada, tercera palabra de esa línea, (*print $3)*
    
    ![Screen Shot 2024-04-07 at 6.49.35 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0bdd19dc-786b-44f8-b15e-73b0b04eff47/3b0ca744-a41a-4b3d-808a-cd6972b8edc5/Screen_Shot_2024-04-07_at_6.49.35_PM.png)
    
    Hacemos lo mismo para ver la memoria total (*print $2)*, necesaria para calcular el porcentaje
    
    **printf vs print in awk**
    
    ![Screen Shot 2024-04-07 at 6.53.21 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0bdd19dc-786b-44f8-b15e-73b0b04eff47/aa891f6d-3c8a-44ed-9acd-964b66d0e6e3/Screen_Shot_2024-04-07_at_6.53.21_PM.png)
    
    > Use the print statement to produce output with simple, standardized formatting. You specify only the strings or numbers to print, in a list separated by commas. They are output, separated by single spaces, followed by a newline.
    > 
    
    > For more precise control over the output format than what is provided by print, use printf. With printf you can specify the width to use for each item, as well as various formatting choices for numbers (such as what output base to use, whether to print an exponent, whether to print a sign, and how many digits to print after the decimal point).
    > 
    
    Para imprimir porcentaje de memoria usada ($3) / memoria total ($2)
    
    **free —mega | awk ‘$1 == “Mem:” {printf(”(%.2f%%)\n”, $3/$2 * 100)}’**
    
    ---
    

**free —mega | awk ‘$1 == “Mem:” {print $3/}**

**free —mega | awk ‘$1 == “Mem:” {print $2}**

**free —mega | awk ‘$1 == “Mem:” {printf(”(%.2f%%)\n”, $3/$2 * 100)}’**

### Memoria del disco

**“La memoria disponible actualmente en tu servidor y su utilización como un porcentaje.”**

- INFO
    
    *df* (”disk filesystem”) muestra un resumen completo del uso del espacio del disco
    
    ![Screen Shot 2024-04-07 at 7.01.36 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0bdd19dc-786b-44f8-b15e-73b0b04eff47/af79260f-2bb7-4e61-970b-2cfa9fe14f31/Screen_Shot_2024-04-07_at_7.01.36_PM.png)
    
    De la imagen de arriba:
    
    - udev es un gestor de dispositivos que controla los ficheros de dispositivos en /dev
    - tmpfs es memoria volátill
    
    **df -m** para mostrarlo en MB
    
    Escoger solo de la carpeta /dev (*grep “/dev”), aqui se encuentra el disco duro,* sin contar con /boot (*grep -v “/boot”).*
    
    **df -m | grep "/dev/" | grep -v "/boot"** 
    
    Sumamos la memoria usada del resultado y lo printeamos
    
    **awk '{memory_use += $3} END {print memory_use}’**
    
    if an `END` rule exists, then the input is read, even if there are no other rules in the program
    
    > Aparte de permitirnos definir los patrones que queramos, awk introduce dos patrones especiales BEGIN y END. El primero permite definir acciones que se ejecutarán antes de empezar a procesar el fichero de entrada, el segundo acciones que se ejecutaran al final del proceso
    > 
    
    ---
    
    Mostrar espacio total de la misma manera pero sumando la 2º columna y convirtiéndolo a GB (/1024)
    
    **df -m | grep "/dev/" | grep -v "/boot" | awk '{memory_result += $2} END {printf(”%.0fGb\n”, memory_result/1024)}’** 
    
    ---
    
    Para mostrar un porcentaje de la memoria usada
    
    **df -m | grep "/dev/" | grep -v "/boot" | awk '{use += $3} {total += $2} END {printf("(%d%%)\n", use/total*100)}’**
    

**df -m | grep "/dev/" | grep -v "/boot" | awk '{memory_use += $3} END {print memory_use}’**

**df -m | grep "/dev/" | grep -v "/boot" | awk '{memory_result += $2} END {printf(”%.0fGb\n”, memory_result/1024)}’**

**df -m | grep "/dev/" | grep -v "/boot" | awk '{use += $3} {total += $2} END {printf("(%d%%)\n", use/total*100)}’**

### Porcentaje uso CPU

**“El porcentaje actual de uso de tus núcleos.”**

- INFO
    
    *vmstat* muestra estadísticas del sistema, permitiendo obtener un detalle general de los procesos, uso de memoria, actividad de CPU, estado del sistema, etc. 
    
    ![Screen Shot 2024-04-07 at 7.32.15 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0bdd19dc-786b-44f8-b15e-73b0b04eff47/ca227fbe-e3da-430e-af9c-20c709c09345/Screen_Shot_2024-04-07_at_7.32.15_PM.png)
    
    Quiero que me muestre cada segundo la info y escojo la última línea (*tail -1*)- De esta, imprimo la última palabra (*awk ‘{print $15}’)* que muestra el uso de memoria disponible
    
    **vmstat 1 4 | tail -1 | awk ‘{print $15}’awk ‘{print $15}’**
    
    El resultado 100 no es el definitivo
    
    # **Basic vmstat Output**
    
    The basic output of the **`vmstat`** command displays system information in six sections.
    
    !https://phoenixnap.com/kb/wp-content/uploads/2021/04/vmstat-basic-output.png
    
    1. **procs** – Process Statistics
    
    - **r** – **Active** **process** count.
    - **b** – **Sleeping** **process** count.
    
    2. **memory** – Memory statistics
    
    - **swpd** – Total **virtual memory**. The swap space is initially unoccupied. However, the kernel starts using swap space as the system’s physical memory reaches its limit.
    - **free** – Total **free** **memory**.
    - **buff** – Total **memory** temporarily used as a **data buffer**.
    - **cache** – Total **cache memory**.
    
    3. **swap** – Swap space Statistics
    
    - **si** – The **rate** of **swapping-in** memory from disk.
    - **so** – The **rate** of **swapping-out** memory to disk.
    
    4. **io** – Input/Output Statistics
    
    - **bi** – Blocks **received** from [a block device](https://phoenixnap.com/glossary/block-device) per second.
    - **bo** – Blocks **sent** to a block device per second.
    
    5. **system** – Scheduling statistics
    
    - **in** – The number of **system interrupts**.
    - **cs** – The number of **context switches** per second.
    
    6. **cpu** – CPU Statistics
    
    - **us** – The percentage of **CPU** time spent on **non-kernel processes**.
    - **sy** – The percentage of **CPU** time spent on **kernel processes**.
    - **id** – The percentage of **idle CPU.**
    - **wa** – The percentage of **CPU** time spent waiting for **Input/Output**.
    - **st** – The percentage of **CPU** time **stolen** by a virtual machine.

**cpu_l = vmstat 1 2 | tail -1 | awk ‘{print $15}’**

**cpu_op = expr 100 - $cpu_l**

**cpu_fin = printf(”%.1f%%”, $cpu_op)**

NOTA: Suele ser bajo pues Linux no consume mucha memoria.  

### Último reinicio

**“La fecha y hora del último reinicio.”**

- INFO
    
    *who* muestra quién está loggeado (usuario, tty, fecha y hora, hostname)
    
    man who:
    
    ![Screen Shot 2024-04-07 at 7.42.57 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0bdd19dc-786b-44f8-b15e-73b0b04eff47/67fcb348-598c-4406-a557-e01e211c1044/Screen_Shot_2024-04-07_at_7.42.57_PM.png)
    
    Para filtrar la información usamos *awk*
    

**who -b | awk '$1 == "system" {print $3 " " $4}’**

### LVM

**“Si LVM está activo o no.”**

- INFO
    
    *lsblk* muestra información de todos los dispositivos de bloque (discos duros, SSD, memorias, etc.)
    
    Para imprimir “Yes” o “No” necesitamos comando *if*
    
    -gt 0 (greater than 0)
    
    ```
    if [condition ]
    then
        If statement
    else
        ELSE statement
    fi
    ```
    

**if [ $(lsblk | grep "lvm" | wc -l) -gt 0 ]; then echo yes; else echo no; fi**

### Conexiones TCP

**“El número de conexiones activas.”**

- INFO
    
    *ss (sockets statistics) -ta* para mostrar solo conexiones TCP
    
    -a o —all → List all listening and non-listening connections with:
    
    -t o —tcp → TCP connections
    
    ![Screen Shot 2024-04-07 at 8.07.14 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0bdd19dc-786b-44f8-b15e-73b0b04eff47/df1d8a37-8484-4f2c-a913-bef6d00f5a81/Screen_Shot_2024-04-07_at_8.07.14_PM.png)
    

**ss -ta | grep ESTAB | wc -l**

### Número de usuarios

**“El número de usuarios del servidor.”**

- INFO
    
    El archivo /etc/passwd es un archivo separado por dos puntos que contiene la siguiente información:
    
    - Nombre de usuario
    - Contraseña cifrada
    - Número de ID de usuario (UID)
    - Número de ID de grupo (GID) del usuario
    - Nombre completo del usuario (GECOS)
    - Directorio inicial del usuario
    - Shell de inicio de sesión
    
    sbin/nologin o /usr/bin/false , significa que **no tiene permiso para loguearse en el sistema**
    
    `sync` is one of the user account created by Debian itself.
    
    *getent **passwd** | grep -v nologin | grep -v root | grep -v sync* muestra los usuarios conectados
    

**getent passwd | grep -v nologin | grep -v root | grep -v sync | wc -l**

### Dirección IPv4 del servidor y MAC

**“La dirección IPv4 de tu servidor y su MAC (Media Access Control)”**

- INFO
    
    *hostname -I (--all-ip-addresses)* para obtener dirección del host
    
    `10.0.1.15`. This is part of a range of IP addresses reserved for internal use by private networks.
    
    Debian, then Ubuntu, choose to define 127.0.1.1 for mapping the IP address of your host_name in case that you have no network.
    
    The host_name matches the hostname defined in `/etc/hostname`.
    
    For a system with a permanent IP address, that permanent IP address should be used here instead of 127.0.1.1.
    
    ```
    127.0.1.1 host_name
    ```
    
    *hostname -i (--ip-addresses)*
    
    ![Screen Shot 2024-04-07 at 8.13.52 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0bdd19dc-786b-44f8-b15e-73b0b04eff47/af31617a-050b-413a-b3e9-ab7d7e04d17e/Screen_Shot_2024-04-07_at_8.13.52_PM.png)
    
    *ip link* para mostrar interfaces de red (dirección MAC)
    
    ![Screen Shot 2024-04-07 at 8.14.19 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0bdd19dc-786b-44f8-b15e-73b0b04eff47/7bce1561-5c93-4f3e-97e7-02bf089fb6b7/Screen_Shot_2024-04-07_at_8.14.19_PM.png)
    

**hostname -I**

**ip link | grep "link/ether" | awk '{print $2}’**

### Número de comandos ejecutados con *sudo*

**“El número de comandos ejecutados con sudo.”**

- INFO
    
    *journalctl* es una herramienta que se encarga de recopilar y administrar los registros del sistema
    
    *_COMM=sudo* para filtrar con solo los comando *sudo.* `_COMM` , ya que hace referencia a un script ejecutable.
    
    ![Screen Shot 2024-04-07 at 8.19.14 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0bdd19dc-786b-44f8-b15e-73b0b04eff47/2291bdb0-bf03-4f47-9c2c-4ecfce58fa55/Screen_Shot_2024-04-07_at_8.19.14_PM.png)
    
    `grep COMMAND` pues también muestra cuando inicias o cierras sesión y asi solo aparecerán las líneas de comandos. Por último pondremos `wc -l` para que asi nos salgan enumeradas las líneas
    
    ![Screen Shot 2024-04-07 at 8.19.31 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0bdd19dc-786b-44f8-b15e-73b0b04eff47/0e981b5f-d4c5-48eb-acd6-bdcb113ea8b1/Screen_Shot_2024-04-07_at_8.19.31_PM.png)
    

**journalctl _COMM=sudo | grep COMMAND | wc -l**

### Imprimir por pantalla como en el subject

*wall* → **write to all** envía un mensaje rápido a los terminales de todos los usuarios loggeados

---

```markdown
#!/bin/bash

# ARCH
arch=$(uname -a)

# CPU PHYSICAL
cpuf=$(grep "physical id" /proc/cpuinfo | wc -l)

# CPU VIRTUAL
cpuv=$(grep "processor" /proc/cpuinfo | wc -l)

# RAM
ram_total=$(free --mega | awk '$1 == "Mem:" {print $2}')
ram_use=$(free --mega | awk '$1 == "Mem:" {print $3}')
ram_percent=$(free --mega | awk '$1 == "Mem:" {printf("%.2f"), $3/$2*100}')

# DISK
disk_total=$(df -m | grep "/dev/" | grep -v "/boot" | awk '{disk_t += $2} END {printf ("%.1fGb\n"), disk_t/1024}')
disk_use=$(df -m | grep "/dev/" | grep -v "/boot" | awk '{disk_u += $3} END {print disk_u}')
disk_percent=$(df -m | grep "/dev/" | grep -v "/boot" | awk '{disk_u += $3} {disk_t+= $2} END {printf("%d"), disk_u/disk_t*100}')

# CPU LOAD
cpul=$(vmstat 1 2 | tail -1 | awk '{printf $15}')
cpu_op=$(expr 100 - $cpul)
cpu_fin=$(printf "%.1f" $cpu_op)

# LAST BOOT
lb=$(who -b | awk '$1 == "system" {print $3 " " $4}')

# LVM USE
lvmu=$(if [ $(lsblk | grep "lvm" | wc -l) -gt 0 ]; then echo yes; else echo no; fi)

# TCP CONNECTIONS (también se pueden usar las flags -tnlp)
tcpc=$(ss -ta | grep ESTAB | wc -l) 

# USER LOG
ulog=$(getent passwd | grep -v nologin | grep -v root | grep -v sync **** | wc -l)

# NETWORK
ip=$(hostname -I)
mac=$(ip link | grep "link/ether" | awk '{print $2}')

# SUDO
cmnd=$(journalctl _COMM=sudo | grep COMMAND | wc -l)

wall "	#Architecture: $arch
				#CPU physical: $cpuf
				#vCPU: $cpuv
				#Memory Usage: $ram_use/${ram_total}MB ($ram_percent%)
				#Disk Usage: $disk_use/${disk_total} ($disk_percent%)
				#CPU load: $cpu_fin%
				#Last boot: $lb
				#LVM use: $lvmu
				#Connections TCP: $tcpc ESTABLISHED
				#User log: $ulog
				#Network: IP $ip ($mac)
				#Sudo: $cmnd cmd"
```

# Crontab

Administrador de procesos en segundo plano. Los procesos indicados serán ejecutados en el momento que especifiques en el fichero crontab.

***sudo crontab -u root -e***

-u → user

-e → edit

> **Cron is a program that allows users of Unix systems to automatically execute scripts, commands or software at a pre-specified date and time, or on a pre-defined cycle.**
> 

> In this project you will have to post a message every 10 minutes. You will need cron in order to do it. Here is how it works:
> 

> 1. You create a script (in our case it will be the monitoring script containing all the information) that you want to run using cron.
> 
> 1. you type then `sudo crontab -u root -e` to open crontab and add a rule
> 2. The rule should be written like that:
> 
> `* * * * <path>/<name_of_the_program_to_be_executed>`
> 
> Where `* * * * *` means every minute of every hour of every day of every month and every day of the week. Let's take some examples:
> 
> `0 * * * *` -this means the cron will run always when the minutes are `0` (so hourly)
> `0 1 * * *` - this means the cron will run always at 1 o'clock.
> `* 1 * * *` - this means the cron will run each minute when the hour is 1. So `1:00`, `1:01`, ...`1:59
> */10 * * * *` - means the cron will run each ten minutes.
> 

Funcionamiento de cada parámetro de crontab:

- **m** corresponde al minuto en que se va a ejecutar el script, el valor va de 0 a 59
- **h** la hora exacta, se maneja el formato de 24 horas, los valores van de 0 a 23, siendo 0 las 12:00 de la medianoche.
- **dom** hace referencia al día del mes, por ejemplo se puede especificar 15 si se quiere ejecutar cada dia 15
- **dow** significa el día de la semana, puede ser numérico (0 a 7, donde 0 y 7 son domingo) o las 3 primeras letras del día en inglés: mon, tue, wed, thu, fri, sat, sun.
- **user** define el usuario que va a ejecutar el comando, puede ser root, u otro usuario diferente siempre y cuando tenga permisos de ejecución del script.
- **command** refiere al comando o a la ruta absoluta del script a ejecutar, ejemplo: */home/usuario/scripts/actualizar.sh*, si acaso llama a un script este debe ser ejecutable

# Signature.txt

Cada vez que se encienda o modifique la máquina la firma cambiará

# Evaluation

Ver Sistema Operativo con:

- sudo cat /etc/os-release
- uname -a
- hostnamectl

Comprobar que no haya interfaz gráfica con **ls** **/usr/bin/*session**

If your Linux system has any GUI session, it should display something like below:

```
/usr/bin/dbus-run-session  /usr/bin/gnome-session-custom-session
/usr/bin/gnome-session
```

As you see in the above output, my Ubuntu has **GNOME** Desktop Environment installed.

- If your system has **MATE** installed, it will print `/usr/bin/mate-session`.
- For **LXDE**, it will return `/usr/bin/lxsession`.

If a Linux doesn't has any GUI installed in it, you will see an output like below:

```
/usr/bin/byobu-select-session  /usr/bin/dbus-run-session
```

# Curiosidades

### TTY1

Terminal Emulators. In this case, we're on /dev/tty1, commonly the first TTY, **used for login and the GUI**

### Núcleos Virtuales (vCPU) VS Núcleos Físicos (CPU)

A CPU that has separation betweMaen two areas of the processor is called a virtual core. Virtual cores do some of the computer’s work without interacting with other components. A virtual processor is another name for a virtual CPU (vCPU). It isn’t actually on the processor, but it acts as if it were.

• For every physical core in a CPU, Hyper-Threading technology provides two virtual processing cores.

• Physical cores are located on the chip itself, as the name implies. Logical cores are a method of treating those physical cores that allows them to be used as two cores

### Usar Snapshot

Guarda el estado de la máquina virtual para que puedas modificar en ella sin alterar la firma (**shasum**)
