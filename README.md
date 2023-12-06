# josemanue-cano-practica-balanceadordecarga
Este repositorio es creado por José Manuel Cano González para el módulo de 2º de ASIR Implementación de Aplicaciones Web.

## Índice ##

- [configuración_balanceador](#configuración-del-servidor-balanceador)
- [configuración_apache1](#configuración-del-servidor-apache1)
- [configuración_apache2](#configuración-del-servidor-apache2)
- [configuración_servidor_mysql](#configuración-del-servidor-mysql)
- [resultado_final](#resultado-final)
- [Instalación Wordpress](#instalación-de-wordpressl)
- [conclusión](#conclusión)


## Introducción ##

*El objetivo de esta práctica tenemos una infraestructura en la que tenemos un servidor que hace de balanceador este servidor se encargara de distribuir peticiones a los servidores web que están en back-end en la infraestructura que vamos a utilizar para esta práctica tendremos dos servidores web en la parte de back-end y otro servidor llamado datos que contendrá el servidor mysql el objetivo de esta práctica es desplegar una aplicación web en los servidores back-end utilizando apache y motor php enlazado con el servidor de mysql que tendrá instalado mariadb-server y el servidor balanceador que tendrá solamente instalado apache el balanceador será el pivote en esta infraestructura para conectarnos con los demás servidores ya que es el único que tiene acceso a internet con esta infraestructura conseguiremos más seguridad en nuestros servidores.*



## Configuración del servidor balanceador ##

*Lo primero que haremos será hacer una copia del fichero default-ssl.conf que está ubicado en **#/etc/apache2/sites-avaliable** con el comando cp en mi caso le he llamado balanceador.conf el comando seria **#sudo cp default-ssl.conf balanceador.conf** lo siguiente que haremos será editar el fichero con el comando **#sudo nano balanceador.conf** y añadimos la siguiente información al fichero:*

![1º imagen](/Practica2/Copia%20sitio%20virtual%20balanceador.png)

![2º imagen](/Practica2/fichero%20balanceador.conf.png)


*Directiva SSLProxyEngine: esta directiva activa el uso del motor de protocolo SSL/TLS para proxy esta directiva se pone dentro de la sección <VirtualHost> para activar el uso de proxy con SSL/TLS en un host virtual.*

*Directiva SSLProxyVerify: esta directiva se usa para configurar verificación del certificado del servidor remoto al estar en none le decimos que no requiere el certificado el servidor remoto para nada.*

*Directiva SSLProxyCheckPerrCN: esta directiva hace una comparación del campo CN del certificado del servidor remoto contra el nombre de host de la URL solicitada si ambos no son iguales lanzara el mensaje de estado 502 bad gateway al estar en off no hará esa comparación.*

*Directiva SSLProxyCheckPeerName: esta directiva comprueba el nombre de host de certificados del servidor cunado mod_ssl está actuando como un cliente SSL esta comprobación tendrá éxito si el nombre de host de la petición coincide con unos de los CN del certificado.*

**Estas directivas tuve que añadirlas al fichero ya que el navegador me daba error con la salida SSL Handshake with remote server.**

![3º imagen](/Practica2/error%20server%20apache%20ssl%20handshake2.png)

![4º imagen](/Practica2/error%20server%20apache%20ssl%20handshake.png)

*Una vez echo lo anterior lo que haremos será instalar el paquete snapd con el comando **#sudo apt install snapd** de seguido instalaremos con el comando **#sudo snap install --clasicc certbot** para instalar certbot que es un cliente que se utiliza para solicitar un certificado de let's encrypt e implementarlo en un servidor web una vez instalado crearemos un enlace simbólico para poder usar el comando certbot para ello ponemos el comando **#sudo ln -s /snap/bin/certbot /usr/bin/certbot** una vez echo eso instalaremos el certificado en apache con el comando **#sudo certbot --apache** y contestaremos a las preguntas en la primera pregunta nos dirá si aceptamos los términos ahí le diremos que si la segunda pregunta que nos hará será si queremos recibir correos para avisarnos de las campañas montadas por let's encrypt ahí le diremos que no  la tercera pregunta nos preguntara en que host virtual del servidor queremos instalar el certificado en mi caso tuve que seleccionar default-ssl.conf porque no hice la copia antes aunque luego volví a reinstalar el certificado y seleccionar balanceador.conf pero si la copia y la configuración del sitio virtual esta echo antes elegiremos ese y por ultimo nos preguntara un dominio en mi caso es josema.hopto.org pero para hacer está practica será necesario que nos registremos en no ip o frenom y creemos un dominio gratuito para esta práctica pondremos el dominio esperamos y estará instalado.*


![4º imagen](/Practica2/Captura%20trabajo%202%20certbot.png)

*Lo siguiente que haremos para terminar de configurar el balanceador será habilitar los mods de proxy y ssl y el sitio virtual https y reiniciar el servicio apache para que todo funcione correctamente.*

![5º imagen](/Practica2/captrua%20trabajo%202%20habiltar%20modos%20balanceador1.png)

![6º imagen](/Practica2/captrua%20trabajo%202%20habiltar%20modos%20balanceador2.png)

![7º imagen](/Practica2/habiltar%20sitio%20virtual%20y%20ssl%20balanceador.png)


## Configuración del Servidor Apache1 ##

*Lo primero que vamos a hacer en este servidor es instalar git con el comando **#sudo apt install git**  ahora clonaremos el repositorio para desarrollar la aplicación web para ello usaremos el comando **git clone url del repositorio** la clonación la haremos en el directorio **/var/www/html** y le cambiaremos de nombre con el comando mv.*


![8º imagen](/Practica2/apache1/captura%20trabajo%20instalar%20git%201.png)

![9º imagen](/Practica2/apache1/clonación%20repositorio.png)

![10º imagen](/Practica2/apache1/cambiar%20nombre%20a%20repositorio%20clonado%20apache1.png)

*Ahora lo que haremos será editar el document root del host virtual del servidor haciendo una copia del fichero default-ssl.conf con el comando **#sudo cp default-ssl.conf apache1** y editando el fichero con el comando **#sudo nano apache1.conf** y en el document root ponemos **/var/www/html/src** habilitaremos el mod ssl con el comando **#sudo enmod ssl** y reiniciaremos el servicio apache.*


![11º imagen](/Practica2/apache1/captura%20apache1%20sitio%20nano.png)

![12º imagen](/Practica2/apache1/captura%20trabajo%203%20apache1%20sitio%20virtual.png)

![13º imagen](/Practica2/apache1/captura3%20trabajo%20habiltar%20ssl%20apache1.png)


## Configuración del Servidor Apache2 ##

*En este servidor repetiremos los mismos pasos que en la configuración del servidor apache1.*

![14º imagen](/Practica2/apache2/captura%20server2%20git%20.png)

![15º imagen](/Practica2/apache2/captura%20trabajo%204%20clonar%20repositorio.png)

![16º imagen](/Practica2/apache2/ssl%20apache2%20modo%20activado.png)

![17º imagen](/Practica2/apache2/sito%20virtual%20apache2.png)


## Configuración del Servidor MYSQL ##

*Lo primero que haremos será configurar el **archivo 50-server.cnf** que esta en la ruta **/etc/mysql/mariadb.conf.d/** y editamos la línea que pone bin-address y poner la ip del servidor mysql guardamos el fichero y reiniciamos el servicio mariadb con el comando **#sudo service mariadb restart**.*

![18º imagen](/Practica2/datos/bin%20address%20server%20mysql.png)

![19º imagen](/Practica2/datos/reinciar%20servicio%20sql.png)


*Ahora haremos el comando scp para pasar el fichero labsuser.pem a la instancia apache1 y desde la maquina apache1 pasaremos el script de la base de daos ubicado en la ruta **/var/www/html/usuarios/db/database.sql** a la instancia datos que es donde está instalado el servidor mysql y cargaremos la base de datos con el comando **#sudo mysql -u root < database.sql**.*


![20º imagen](/Practica2/datos/scp%20maquina%20local%20hacia%20aws%20instancia.png)

![21º imagen](/Practica2/datos/scp%20entre%20dos%20instancias%20aws%20base%20de%20datos.png)

![22º imagen](/Practica2/datos/cargar%20base%20de%20datos%20en%20aws%20datos.png)

*Lo siguiente que haremos será crear los usuarios para que accedan a la base de datos he creado dos porque poner 192.168.1.0* no me accedía, pero creando dos usuarios no me dejaba asique creamos dos usuarios y le damos permisos sobre la base de datos importada llamada **lamp_db** un usuario se llamara user_db y tendra contraseña josema y otro se llamara josema12 y contraseña josema posteriormente accederemos con ellos para ver que se han creado correctamente y pasaremos al resultado final.*

![23º imagen](/Practica2/datos/crear%20usuario%20maquina%201.png)

![24º imagen](/Practica2/datos/crear%20usuario%20maquina2.png)

![25º imagen](/Practica2/datos/permisos%20maquina2.png)

![26º imagen](/Practica2/datos/acceso%20mysql%20apache1.png)

![27º imagen](/Practica2/datos/acceso%20mysql%20apache2.png)


## Resultado Final ##

*El último paso es poner la dirección https://josema.hopto.org y que nos salga la página configurada en el directorio **/var/www/html/usuarios/src**.*

![28º imagen](/Practica2/Resultado%20final.png)

*Ahora insertaremos una fila en la tabla users de la base de datos **lamp_db** y recargaremos la página para ver si se ha agregado una entrada a la web.*

![29º imagen](/Practica2/datos/inserccion%20filas.png)

![30º imagen](/Practica2/datos/resultado%201%20inserccion%20filas.png)

*Por ultimo probaremos a darle al botón add new data y comprobar que la creación ha sido correcta y se refleja recargando la página y se refleja también haciendo una consulta a la tabla **users**.*

![32º imagen](/Practica2/datos/inserccion%20desde%20la%20web.png)

![33º imagen](/Practica2/datos/data%20succesfull.png)

![34º imagen](/Practica2/datos/resultado%20final%20web%202.png)

![35º imagen](/Practica2/datos/resultado%20consulta%20tabla%20users.png)

*Con esto ultimo la página esta implementada y funcionado*


## Instalación de Wordpress ##

Lo primero que haremos será descargar wordpress en los servidores apache

![35º imagen](/josemanue-cano-practica-balanceadordecarga/Practica2/wordpress/descagar%20wordpress%20en%20www%201.png)


## Conclusión ##

*En esta práctica me ha dado quebradero de cabeza ya que por supuesto nada sale a la 1º pero después de todo el finde machacando lo he conseguido una práctica en la que he tenido muchos errores y los he ido solucionando hasta conseguir el resultado final de la práctica he aprendido en esta práctica a buscarme la vida y solucionar errores que me han ido saliendo por ejemplo uno de ellos ha sido cuando la página no me cargaba porque para mostrar la página tenían que acceder los servidores de back_end a la base de datos práctica no muy fácil pero tampoco muy difícil.*

## Práctica realizada por José Manuel Cano González ##