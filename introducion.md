## INTRODUCCIÓN

![LOGO](https://www.ryadel.com/wp-content/uploads/2017/12/vsftpd-linux-very-secure-ftp-server-logo.png)

**vsFTPd** (del inglés very secure FTP daemon), es un servidor de FTP popular entre sistemas Unix y Linux. *Está licenciado bajo la licencia GPL y soporta IPv6 y SSL*. Además, soporta FTP explicito y implícito. vsFTPd se puede usar en las distribuciones de GNU/Linux como **Ubuntu, CentOS** o **Debian**.

[WEB OFICIAL](https://security.appspot.com/vsftpd.html)

### Características
- Soporte para direcciones IP virtuales.
- Poderosa configuración para cada usuario.
- Se ejecuta de manera independiente o a través de inetd.
- Soporte para usuarios virtuales.
- Soporte para IPv6.
- Límites para direcciones IP.
- Soporte para el control de ancho de banda.
- Soporte de encriptación a través de SSL.

### Seguridad
vsftpd fue diseñado e implementado enfocandose en la seguridad. Al no sobreusar el usuario root, soluciona problemas presentes en la mayoría de las instalaciones de [wu-ftpd](https://en.wikipedia.org/wiki/WU-FTPD), [proftpd](http://www.proftpd.org) incluyendo [freebsd](https://www.freebsd.org).

- Usa chroot.
- Emplea técnicas de codificación segura para resolver los buffers overflows.
- Su desarrollado es investigador de vulnerabilidades en otras aplicaciones.

---
[ATRAS](https://github.com/estebancr1993/vsftpd)
