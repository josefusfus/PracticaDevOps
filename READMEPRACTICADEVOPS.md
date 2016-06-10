Rutas para comprobar servidor :
Aplicación de nodepop :  http://nodepop.jlsaez.info
Web BootStrap : http://www.jlsaez.info  o ip 52.204.11.163

PARA COMPROBAR SERVICIO DE ARCHIVOS ESTATICOS

http://nodepop.jlsaez.info/anuncios/iphone.png
http://nodepop.jlsaez.info/anuncios/bici.jpg


Peticiones con Postman para ver como funciona la app.

Crea un usuario primero.
Hacemos un post con estos datos

http://nodepop.jlsaez.info/apiv1/usuarios/register
           
en BODY x-www-form-urlencoded

CAMPOS

nombre, usuario y clave. 

con esto creamos un usuario

--------------------------

Creamos Token
URL- TIPO POST

http://nodepop.jlsaez.info/apiv1/usuarios/authenticate

en BODY x-www-form-urlencoded

CAMPOS

usuario(El que hayas creado) y clave(La que hayas puesto). 

con esto creamos el token

--------------------------------
Consultamos anuncios
COn postman en GET

http://nodepop.jlsaez.info/apiv1/anuncios?token=(Aqui va el token generado sin los parentesis)

He rehecho el servidor desde 0, he instalado en él GIT,NODE,PM2, MONGO, NGINX he puesto IP elastica y he unido las direcciones a mi domino personal.
El servidor responde con nginx diferenciando a donde va la petición perfectamente, si queremos la web o petición a la app de nodepop.
Solo me ha quedado pendiente hacer funcionar que muestre los archivos estaticos.
Me gustaria saber como hacerlo.

EL unico codigo que he tenido que escribir creo ha sido el de los sites-available,el archivo NODEPOP creo...
Aqui lo pongo:

server {
        listen 80;
        listen [::]:80;
        server_name nodepop.jlsaez.info;
        
        location ~ ^/(css/|img/|js/|sounds/|anuncios/) {
        root /home/node/agbo-master-nodejs-practica/public/images; # ruta base $
        access_log off; # no dejar log de acceso a estáticos (no aportan info)
        expires max; # máximo de expiración (cache)

        #pongo una cabecera personalizada
        add_header X-Owner @josefusfus;

                                                }
        location / {
        #Le paso el trabajo a node actuando como proxy inverso
        proxy_pass http://localhost:3000;
        proxy_redirect off; # no redirigimos
                    }

        }




