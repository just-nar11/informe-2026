# informe-2026
## 1. Introducción 
En el área de la administración de sistemas informáticos, garantizar la seguridad de los accesos remotos es un aspecto clave para proteger la información y el correcto funcionamiento de la red. Habitualmente, la gestión remota de servidores en Linux se realiza mediante el protocolo SSH (Secure Shell), el cual permite conectar un equipo con el servidor de forma cifrada. Sin embargo, utilizar la configuración predeterminada de este servicio —como mantener el puerto 22 que viene por defecto— suele dejar al sistema expuesto a escaneos automáticos e intentos de acceso no autorizados.   El objetivo de este informe es documentar el proceso de instalación, revisión y fortalecimiento del servicio SSH (sshd) en un entorno de servidor Linux. A lo largo del documento se detallan los cambios aplicados en el archivo de configuración principal (/etc/ssh/sshd_config), enfocándose principalmente en el cambio del puerto de conexión estándar por uno personalizado (el puerto 222), como una medida básica para reducir posibles riesgos.   Además, se explica el procedimiento para ajustar las reglas de seguridad internas del sistema mediante SELinux (Security-Enhanced Linux). Para ello, se utilizó la herramienta semanage (con el comando semanage port -a -t ssh_port_t -p tcp 222), lo que permitió autorizar el uso del nuevo puerto y evitar que el sistema bloquee el tráfico legítimo.   Por último, se comprobó el correcto funcionamiento del servicio a través de comandos como systemctl para revisar el estado del servidor y analizar los registros de acceso. Finalmente, se realizó la prueba de conexión remota desde el cliente MobaXterm Personal Edition, confirmando que la autenticación del usuario (chrisspatoby2008), el ingreso al sistema y el manejo de archivos funcionen sin inconvenientes.   Con este trabajo se busca mostrar de forma práctica los pasos necesarios para configurar accesos remotos seguros, sirviendo como una referencia sencilla para mantener un servidor mejor protegido.

## 2. Objetivo general: 
Documentar de forma práctica el proceso de instalación, revisión, fortalecimiento y verificación del servicio SSH (sshd) en un entorno de servidor Linux para garantizar accesos remotos seguros
## Objetivos específicos
 Modificar el archivo de configuración principal (/etc/ssh/sshd_config) cambiando el puerto predeterminado(22) por un puerto personalizado (222) para mitigar escaneos automatizados.


Verificar el estado del servicio mediante comandos del sistema como systemctl y analizar los registros de acceso.


Comprobar la conexión remota exitosa desde el cliente MobaXterm Personal Edition, validando la autenticación del usuario (chrisspatoby2008), el ingreso al sistema y la manipulación de archivos.


## Desarrollo
La administración remota de servidores basados en Linux se realiza mediante el comando SSH, el cual cifra de extremo a extremo las credenciales y los comandos ejecutados en la red. Sin embargo, la configuración estándar opera sobre el puerto TCP 22, convirtiéndolo en el blanco principal de bots, escaneos de puertos lo que hace que la seguridad del puerto sea deficiente.


Para reducir esta inseguridad, se procede a modificar el archivo de configuración del demonio SSH:
* Ubicación del archivo: /etc/ssh/sshd_config
* Cambio aplicado: Se localiza la directiva del puerto y se sustituye el valor predeterminado por un puerto no estándar
Port 222


En distribuciones de Linux CentOS, el servicio SSH solo tiene permitida por defecto la escucha en los puertos autorizados por su etiqueta de seguridad (ssh_port_t). Si se cambia el puerto en el archivo de configuración sin actualizar la política de SELinux, el servicio fallará al iniciar.


Una vez modificados la configuración y los parámetros de SELinux, es indispensable aplicar los cambios en el sistema CentOS:
• Reinicio del servicio:
comando: systemctl restart sshd


y para comprobar el estado: systemctl status sshd

Para validar la funcionalidad del servidor, se efectúa una prueba práctica desde un equipo cliente utilizando el MobaXterm:
	• Tipo de sesión: SSH
	• Dirección IP del servidor: (Especificar la IP asignada a la máquina CentOS)
	• Puerto: 222
	• Usuario: chrisspatoby2008


## Resultados:
Ingreso: 
  MobaXterm se ejecuta de forma segura la contraseña del usuario chrisspatoby2008. Tras ser validada por CentOS, 
  se abre la consola de comandos remota.

  
Manejo de MobaXterm: 
  La interfaz gráfica de MobaXterm activa automáticamente, utilizando el mismo puerto 222. Esto permite confirmar que la subida,
  bajada y edición visual de archivos dentro del servidor
  CentOS funcionan de manera fluida y segura.


## Conclusión:
De acuerdo con las pruebas y evidencias operativas registradas en el sistema, se concluye que:

Reconfiguración Exitosa del Servicio: Se realizó de forma satisfactoria la modificación del archivo de configuración del demonio de OpenSSH (/etc/ssh/sshd_config) en el servidor (IP 10.21.165.123).   


Adaptación de Políticas de Seguridad (SELinux): Mediante la ejecución de semanage port, se actualizaron de manera correcta las reglas de SELinux para permitir el tráfico SSH a través del puerto no estándar 222/TCP, garantizando la integración de los cambios sin comprometer las políticas de seguridad del sistema operativo.   


Operatividad y Validación de Conexión: Tras reiniciar y auditar el estado del servicio (systemctl status sshd), se verificó que el demonio de SSH se encuentra en estado activo y escuchando peticiones en la dirección 0.0.0.0:222. Asimismo, los registros de auditoría (logs) confirmaron la autenticación e inicio de sesión exitoso del usuario chrisspatoby2008 mediante la nueva vía de acceso configurada.


En síntesis, la implementación del cambio de puerto para el servicio SSH se ejecutó de manera efectiva, cumpliendo con los estándares de administración de sistemas y manteniendo la disponibilidad del acceso remoto.
