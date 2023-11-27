# josemanue-cano-practica-balanceadordecarga
Este repositorio es creado por José Manuel Cano González para el modulo de 2º de ASIR Implementación de Aplicaciones Web




## INTRODUCCIÓN ##

*El objetivo de esta práctica tenemos una infraestructura en la que tenemos un servidor que hace de balanceador este servidor se encargara de distribuir peticiones a los servidores web que están en back-end en la infraestructura que vamos a utilizar para esta práctica tendremos dos servidores web en la parte de back-end y otro servidor llamado datos que contendrá el servidor mysql el objetivo de esta práctica es desplegar una aplicación web en los servidores back-end utilizando apache y motor php enlazado con el servidor de mysql que tendrá instalado mariadb-server y el servidor balanceador que tendrá solamente instalado apache el balanceador será el pivote en esta infraestructura para conectarnos con los demás servidores ya que es el único que tiene acceso a internet con esta infraestructura conseguiremos más seguridad en nuestros servidores.*



## Configuración del servidor balanceador ##

*Lo primero que haremos será hacer una copia del fichero default-ssl.conf que está ubicado en **#/etc/apache2/sites-avaliable** con el comando cp en mi caso le he llamado balanceador.conf el comando seria **#sudo cp default-ssl.conf balanceador.conf** lo siguiente que haremos sera editar el fichero con el comando **#sudo nano balanceador.conf** y añadimos la siguiente información al fichero:*

![1º imagen](/Practica2/Copia%20sitio%20virtual%20balanceador.png)

![2º imagen](/Practica2/fichero%20balanceador.conf.png)


*Directiva SSLProxyEngine: esta directiva activa el uso del motor de protocolo SSL/TLS para proxy esta directiva se pone dentro de la sección <VirtualHost> para activar el uso de proxy con SSL/TLS en un host virtual.*

*Directiva SSLProxyVerify: esta directiva se usa para configurar verificación del certificado del servidor remoto al estar en none le decimos que no requiere el certificado el servidor remoto para nada.*

*Directiva SSLProxyCheckPerrCN: esta directiva hace una comparación del campo CN del certificado del servidor remoto contra el nombre de host de la URL solicitada si ambos no son iguales lanzara el mensaje de estado 502 bad gateway al estar en off no hara esa comparación.*

*Directiva SSLProxyCheckPeerName: esta directiva comprueba el nombre de host de certificados del servidor cunado mod_ssl está actuando como un cliente SSL esta comprobación tendrá éxito si el nombre de host de la petición coincide con unos de los CN del certificado.*

**Estas directivas tuve que añadirlas al fichero ya que el navegador me daba error con la salida SSL Handshake with remote server.**

![3º imagen](/Practica2/error%20server%20apache%20ssl%20handshake2.png)

![4º imagen](/Practica2/error%20server%20apache%20ssl%20handshake.png)

*Una vez echo lo anterior lo que haremos será instalar el paquete snapd con el comando **#sudo apt install snapd** de seguido instalaremos con el comando **#sudo snap install --clasicc certbot** para instalar certbot que es un cliente que se utiliza para solicitar un certificado de let's encrypt e implementarlo en un servidor web una vez instalado crearemos un enlace simbólico para poder usar el comando certbot para ello ponemos el comando **#sudo ln -s /snap/bin/certbot /usr/bin/certbot** una vez echo eso instalaremos el certificado en apache con el comando **#sudo certbot --apache** y contestaremos a las preguntas en la primera pregunta nos dirá si aceptamos los terminos ahí le diremos que si la segunda pregunta que nos hará será si queremos recibir correos para avisarnos de las campañadas montadas por let's encrypt ahí le diremos que no  la tercera pregunta nos preguntara en que host virtual del servidor queremos instalar el certificado en mi caso tuve que seleccionar default-ssl.conf porque no hice la copia antes aunque luego volvi a reinstalar el certificado y seleccionar balanceador.conf pero si la copia y la configuración del sitio virtual esta echo antes elegiremos ese y por ultimo nos preguntara un dominio en mi caso es josema.hopto.org pero para hacer está practica sera necesario que nos registremos en no ip o frenom y creemos un dominio gratuito para está práctica pondremos el dominio esperamos y estara instalado.*


![4º imagen](/Practica2/Captura%20trabajo%202%20certbot.png)

*Lo siguiente que haremos para terminar de configurar el balanceador será habiltar los mods de proxy y ssl y el sitio virtual http y reinciar el servicio apache para que todo funcione correctamente.*

![5º imagen](/Practica2/captrua%20trabajo%202%20habiltar%20modos%20balanceador1.png)

![6º imagen](/Practica2/captrua%20trabajo%202%20habiltar%20modos%20balanceador2.png)

![7º imagen](/Practica2/habiltar%20sitio%20virtual%20y%20ssl%20balanceador.png)


## Configuración del Servidor Apache1 ##

*Lo primero que vamos a hacer en este servidor es instalar git con el comando **#sudo apt install git**  ahora clonaremos el repositorio para desarollar la aplicación web para ello usarmeos el comando **git clone url del repositorio** la clonación la haremos en el directorio **/var/www/html** y le cambiaremos de nombre con el comando mv*


![8º imagen](/Practica2/apache1/captura%20trabajo%20instalar%20git%201.png)

![9º imagen](/Practica2/apache1/clonación%20repositorio.png)

![10º imagen](/Practica2/apache1/cambiar%20nombre%20a%20repositorio%20clonado%20apache1.png)
