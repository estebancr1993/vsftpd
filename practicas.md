## CASOS PRÁCTICOS

**1.-Version vsftpd Instalada.**

```vsftpd -v```

![2](https://github.com/estebancr1993/vsftpd/blob/main/imagenes/2.JPG)

**2.-Usuarios creados en instalación.**

El usuario creado es **ftp**, comprobamos que esta:

```getent passwd | grep ftp```

![3](https://github.com/estebancr1993/vsftpd/blob/main/imagenes/3.JPG)

**3.-Servicio Asociado.**

Visualizar en el estado que se encuentra: ```systemctl status vsftpd```

Habilitar el servicio: ```systemctl enable vsftpd```

Deshabilitar el servicio: ```systemctl disable vsftpd```

Iniciar el servicio: ```systemctl start vsftpd```

Parar el servicio: ```systemctl stop vsftpd```

Reiniciar el servicio: ```systemctl restart vsftpd```

Recargar el servicio: ```systemctl reload vsftpd```

![4](https://github.com/estebancr1993/vsftpd/blob/main/imagenes/4.JPG)

**4.-Ficheros de configuracion.**

Toda la configuración de **vsftpd** es manejada por su archivo de configuración: ```/etc/vsftpd/vsftpd.conf```

Las directivas más importantes son:

- **write_enable=YES** –> Esta directiva nos permite poder escribir (copiar archivos y carpetas) al servidor FTP.
- **local_umask=022** –> Esta directiva nos permite habilitar los permisos nuevos cuando copiemos datos al servidro FTP, por defecto el umask es 077 pero podremos modificarlo por el valor que nosotros queramos, 022 es el umask más utilizado en otros servidores FTP.
- **ftpd_banner** –> Esta directiva permite poner un banner de inicio de sesión.
- **chroot_list_enable=YES** –> Nos permite habilitar el chroot de los diferentes usuarios del sistema, para que solamente un usuario entre en su carpeta /home/usuario y en ninguna más, es una medida de seguridad, pero hay que usarla con mucho cuidado ya que si un usuario tiene permisos en directorios superiores tendrá acceso al resto.
- **chroot_list_enable=YES** –> Nos permite crear una lista con los usuarios en chroot, todos los que aparezcan aquí podrán conectarte.
- **chroot_list_file=/etc/vsftpd.chroot_list** –> Es la lista de usuarios con sus rutas predeterminadas.

Archivo sin comentarios:

![5](https://github.com/estebancr1993/vsftpd/blob/main/imagenes/5.JPG)

Existen otros ficheros interesantes:

- **/etc/vsftpd.ftpusers** —> Una lista de los usuarios que no tienen permitido conectarse a vsftpd. Por defecto esta lista incluye a los usuarios _root, bin_ y _daemon_, entre otros.

- **/etc/vsftpd.user_list** —> Este archivo se puede configurar para negar o permitir el acceso a los usuarios listados, dependiendo de si la directiva *userlist_deny* está configurada a YES (por defecto) o a NO en /etc/vsftpd/vsftpd.conf. Si se utiliza /etc/vsftpd.user_list para permitir acceso a los usuarios, los nombres de usuarios listados no deben aparecer en /etc/vsftpd.ftpusers.

- El directorio **/var/ftp/** —> El directorio que contiene los archivos servidos por vsftpd. También contiene el directorio **/var/ftp/pub/** para los usuarios anónimos. Ambos directorios están disponibles para la lectura de todos, pero sólo el superusuario o root puede escribir en el.

**5.-Acceso al servidor ftp: usuarios del sistema (Enjaulados)**

> **NOTA: Antes de modificar cualquier archivo de configuración es conveniente hacer una copia.**
 
- Descomentamos la linea 28 del archivo /etc/vsftpd.conf 

![6](https://github.com/estebancr1993/vsftpd/blob/main/imagenes/6.JPG)

- **Enjaular a los usuarios del sistema.**

Agregue estas dos opciones para restringir a los usuarios de FTP a sus directorios de Inicio.

```chroot_local_user=YES```

```allow_writeable_chroot=YES```

![7](https://github.com/estebancr1993/vsftpd/blob/main/imagenes/7.JPG)

> chroot_local_user = YES significa que los usuarios locales serán ubicados en una jaula chroot, su directorio de inicio después de iniciar sesión con la configuración predeterminada.

> Y también de forma predeterminada, vsftpd no permite que se pueda escribir en el directorio chroot jail por razones de seguridad; sin embargo, podemos usar la opción allow_writeable_chroot = YES para anular esta configuración.

Reiniciar servicio

**COMPROBACIÓN**

Utilizaremos Filezilla para las conexiones, [DESCARGAR](https://filezilla-project.org/download.php)

![8](https://github.com/estebancr1993/vsftpd/blob/main/imagenes/8.JPG)

**6.-Acceso al servidor FTP: anónimo tiene solo permisos de lectura en su directorio trabajo.**

- Activar el usuario **anónimo (anonymous)** en el archivo /etc/vsftpd.conf linea 25.

```anonymous_enable=YES```

![9](https://github.com/estebancr1993/vsftpd/blob/main/imagenes/9.JPG)

Reiniciar servicio.

- Crear archivo prueba.txt en el servidor.

```touch /srv/ftp/prueba.txt```


**COMPROBACIÓN**
 
![10](https://github.com/estebancr1993/vsftpd/blob/main/imagenes/10.JPG)

**7.-Acceso al servidor FTP: Anonimo tiene permisos de escritura en sugerencias.**

- Creamos la carpeta ```sugerencias``` le cambiamos el dueño y los permisos.

```console
mkdir /srv/ftp/sugerencias
chown ftp:nogroup -R /srv/ftp 
chmod u-w /srv/ftp
```

- Configurar el archivo **vsftpd.conf** añadiendo:

```write_enable=YES```

```anon_upload_enable=YES```

```anon_mkdir_write_enable=YES```

```anon_other_write_enable=YES```

![11](https://github.com/estebancr1993/vsftpd/blob/main/imagenes/11.JPG)

Reiniciar servicio.

**COMPROBACIÓN**

![12](https://github.com/estebancr1993/vsftpd/blob/main/imagenes/12.JPG)

**8.-Creación usuarios virtuales.**

- Instalar los siguientes paquetes:

```console
apt install apache2-utils
apt install libpam-pwdfile
```

- Añadir al archivo de configuración:

```console
# Configuración específica de usuarios virtuales
user_config_dir=/etc/vsftpd/usersConf
guest_enable=YES
virtual_use_local_privs=YES
pam_service_name=vsftpd
```

Reiniciar servicio

- Creación usuarios virtuales.

```console
mkdir /etc/vsftpd
htpasswd -c -d /etc/vsftpd/ftpd.passwd virtual1
htpasswd -d /etc/vsftpd/ftpd.passwd virtual2
```

> NOTA: El comando htpasswd nos solicitará que definamos la contraseña y que la confirmemos, y ojo que en el segundo usuario hemos omitido la opción -c, solo es necesario incluirla con el primer usuario, si la incluimos en el segundo o sucesivos truncaremos el fichero y por lo tanto únicamente dejaremos el último usuario que hemos creado.
 
- Nos dirigimos al archivo ```/etc/pam.d/vsftpd``` y añadimos:

```console
auth required pam_pwdfile.so pwdfile /etc/vsftpd/ftpd.passwd
account required pam_permit.so
```

- Directorio de usuarios virtuales.

> Para realizar esta configuración crearemos un archivo por cada usuario al que estemos concediendo acceso, y para mantenerlos organizados los almacenaremos dentro de un directorio que crearemos a tal efecto, al que ya hemos hecho referencia en /etc/vsftpd.conf mediante la directiva user_config_dir. **El nombre de cada archivo debe ser idéntico al nombre de usuario al que hace referencia:**

```console
mkdir /etc/vsftpd/usersConf
echo "local_root=/srv/ftp/virtual1" > /etc/vsftpd/usersConf/virtual1
echo "local_root=/srv/ftp/virtual2" > /etc/vsftpd/usersConf/virtual2
```

- Los permisos.

```console
chown -R vsftpd:nogroup /srv
```

**COMPROBACIÓN**

![13](https://github.com/estebancr1993/vsftpd/blob/main/imagenes/13.JPG)

**9.-Acceso seguro al servidor FTP.**

- Instalación del paquete para generar certificado:

```apt install openssl```

- Creación del certificado.

```console
cd /etc/ssl/private/
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout vsftpd.pem -out vsftpd.pem
```

- Ponemos la directiva de ssl en YES:

```ssl_enable=YES```

**COMPROBACIÓN**

![14](https://github.com/estebancr1993/vsftpd/blob/main/imagenes/14.JPG)

---
[ATRAS](https://github.com/estebancr1993/vsftpd)
