# josemanue-cano-practica-balanceadordecarga
Este repositorio es creado por José Manuel Cano González para el modulo de 2º de ASIR Implementación de Aplicaciones Web




**INTRODUCCIÓN**

*El objetivo de esta práctica tenemos una infraestructura en la que tenemos un servidor que hace de balanceador este servidor se encargara de distribuir peticiones a los servidores web que están en back-end en la infraestructura que vamos a utilizar para esta práctica tendremos dos servidores web en la parte de back-end y otro servidor llamado datos que contendrá el servidor mysql el objetivo de esta práctica es desplegar una aplicación web en los servidores back-end utilizando apache y motor php enlazado con el servidor de mysql que tendrá instalado mariadb-server y el servidor balanceador que tendrá solamente instalado apache el balanceador será el pivote en esta infraestructura para conectarnos con los demás servidores ya que es el único que tiene acceso a internet con esta infraestructura conseguiremos más seguridad en nuestros servidores.*



## Configuración del servidor balanceador ##

*Lo primero que haremos será hacer una copia del fichero default-ssl.conf que está ubicado en **#/etc/apache2/sites-avaliable** con el comando cp en mi caso le he llamado balanceador.conf el comando seria **#sudo cp default-ssl.conf balanceador.conf** lo siguiente que haremos sera editar el fichero con el comando **#sudo nano balanceador.conf** y añadimos la siguiente información al fichero: *

![1º imagen](/Practica2/Copia%20sitio%20virtual%20balanceador.png)












*Una vez aprovisionado la instancia balanceador lo primero que haremos será instalar el paquete snapd con el comando **#sudo apt install snapd** de seguido instalaremos con el comando **#sudo snap install --clasicc certbot** para instalar certbot que es un cliente que se utiliza para solicitar un certificado de let's encrypt e implementarlo en un servidor web una vez instalado crearemos un enlace simbólico para poder usar el comando certbot para ello ponemos el comando **#sudo ln -s /snap/bin/certbot /usr/bin/certbot** una vez echo eso instalaremos el certificado en apache con el comando **#sudo certbot --apache** y contestaremos a las preguntas en la primera pregunta nos dirá si aceptamos los terminos ahí le diremos que si la segunda pregunta que nos hará será si queremos recibir correos para avisarnos de las campañadas montadas por let's encrypt ahí le diremos que no  la tercera pregunta nos preguntara en que host virtual del servidor lo siguiente nos preguntara un dominio en mi caso es josema.hopto.org pero para hacer está practica sera necesario que nos registremos en no ip o frenom y creemos un dominio gratuito para está práctica seleccionaremos en este