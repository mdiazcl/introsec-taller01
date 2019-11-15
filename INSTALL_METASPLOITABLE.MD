# Metasploitable
Metasploitable es una máquina virtual que está diseñada para recibir ataques. Tiene programas vulnerables y muchas -malas configuraciones- a propósito para que puedas probara tus habilidades. Si bien está en internet la documentación de cómo atacarla será utilizada para enseñar algunas técnicas en el taller.

Para este taller usaremos la máquina Metasploitable versión 2 (ya que la 3 ocupa otra tecnológia y complicaría de sobremanera el taller).

## Paso 1: Descargar lo necesario.
Como siempre, necesitas descargar la máquina para utilizarla. Te dejo los enlaces
* **[Descargar Metasploitable v2](http://downloads.metasploit.com/data/metasploitable/metasploitable-linux-2.0.0.zip)**: Es un archivo en formato Zip que pesa aproximadamente 825mb.

## Paso 2: Instalación de la máquina
La máquina viene preparada para Virtualbox, así que no es tan complicado. Descomprime el zip en una carpeta y te aparecerán 5 archivos, que representan la máquina.

![](images/files_metasploitable.PNG)

Cada uno de estos archivos representa distitnas partes de la máquina. Principalmente el archivo de gran tamaño (con la cajita azul) representa el disco duro (con la extensión VMDK).

## Paso 3: Crear la máquina virtual
Necesitamos crear la máquina virtual de metasploitable! Pero esta es más facil que Kali por que el disco duro ya viene creado (el VMDK del paso anterior). Asi que tan solo es necesario crear el fierro alrededor de ese disco.

Para ello, crearemos una nueva máquina virtual

![](images/abrir_vbox.PNG)

Hacer click en Nueva (Botón celeste) y elegir un nombre para la máquina. En Tipo seleciona ``Linux`` y Versión ``Ubuntu (64-bit)``.

![](images/new_vm_metasploitable.PNG)

Elige cuanta RAM le quieres asignar a la máquina. Como mínimo elije 1536 MB, pero si quieres ponerle más dale!

![](images/ram_metasploitable.PNG)

En la seleccion del disco duro, utilizaremos el que descargamos, para ello es necesario elegir la opción "Usar un archivo de disco duro virtual existente" y darle al ícono de la carpeta.

![](images/select_disk_metasploitable.png)

En la parte superior, presiona el botón **Agregar** y seleciona el VMDK que descomprimimos.

![](images/disk_metasploitable.PNG)

Luego termina de crear la máquina.

## Paso 4: Configurar la máquina virtual
Al igual que en en la máquina virtual de Kali, vamos a especificar que la tarjeta de red que utilizará será la de uso interno. Para ello selecionar la máquina recién creada y presiona el botón configuración.

![](images/config_metasploitable.png)

Ir a la **Opcion Red > (tab) Adaptador 1** y seleccionamos la opción ``Red Interna`` y dejaremos el nombre como ``intnet``.

![](images/eth0_metasploitable.PNG)

Guardar la configuración y ejecutar la máquina Virtual con el botón Iniciar.

La máquina comenzará a bootear todo lo necesario. Esperemos hasta que aparezca el prompt de login.

![](images/prompt_metasploitable.PNG)

En este punto tenemos que logear en la máquina. Usa las siguientes credenciales por defecto para esta máquina:
* **Usuario:** msfadmin
* **Password:** msfadmin

Como esta sera nuestra máquina objetivo, necesitamos asignarle una dirección IP dentro de nuestra LAN. Para ello le pondremos una dirección IP estática y así usarla como objetivo posterior. Esta dirección debes ANOTARLA.

Para establecer una dirección IP estática sigue los siguientes pasos. Usaré nano por la simplicidad, pero siéntete libre de usar VIM.

```bash
sudo nano /etc/network/interfaces
```

![](images/ifconfig_metasploitable.PNG)

Dentro de este archivo, necesitamos editar desde la línea ``iface eth0 inet dhcp`` hacia abajo, reemplazandolo por lo siguiente:

```bash
iface eth0 inet static
address 10.0.0.30
netmask 255.255.255.0
network 10.0.0.0
broadcst 10.0.0.255
dns-nameservers 8.8.8.8
```

Presionar ``CTRL + X`` y guardar los cambios. Ahora que establecimos la dirección estática, necesitamos reiniciar el adaptador de red.
```bash
sudo /etc/init.d/networking restart
```

![](images/ifconfig_ready_metasploitable.PNG)

Con esto la máquina queda lista para el laboratorio!