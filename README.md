### ¿Por qué el frontend no necesita conectarse a PostgreSQL?
Porque solo sirve la interfaz y reenvía las peticiones a la API. Quien habla con la base de datos es el backend, así que el frontend no necesita acceso a ella.
 
### ¿Por qué el backend pertenece a dos redes?
Porque hace de intermediario: recibe las peticiones del frontend por la red `frontend` y consulta a PostgreSQL por la red `datos`.
 
### ¿Qué servicio necesita publicar un puerto en el anfitrión?
El frontend, porque es el único al que accede el navegador del usuario. El backend y PostgreSQL solo se usan dentro de las redes de Docker.
 
### ¿Por qué publicar PostgreSQL hacia fuera no es la solución adecuada para un fallo de comunicación interno?
Porque el fallo se debe a la red interna (el backend no resuelve `db`) y no a los puertos. Además, abrir PostgreSQL al exterior lo expone a accesos no deseados y es un riesgo de seguridad.
 
### ¿Qué ventaja ofrece Docker Compose respecto a ejecutar los tres contenedores manualmente?
Permite definir toda la aplicación en un único archivo y levantarla con un solo comando, sin repetir muchos `docker run`, `docker network create` y `docker volume create`. Así es más fácil de repetir y de mantener.
 
### ¿Por qué hemos dividido la arquitectura en dos redes?
Para aislar los servicios: el frontend no puede llegar a la base de datos y esta solo es accesible desde el backend. Así se reduce la superficie de ataque.
 
### ¿Por qué el backend utiliza `db` como dirección de PostgreSQL?
Porque `db` es el nombre del servicio y el DNS interno de Docker lo traduce a la IP del contenedor. `localhost` apuntaría al propio contenedor del backend, y las IP pueden cambiar al recrear los contenedores.
 
### ¿Qué diferencia existe entre detener un proyecto y eliminar sus volúmenes?
Detener (`stop` o `down`) quita o para los contenedores pero conserva los datos en el volumen. Eliminar los volúmenes (`down -v`) borra la base de datos y los datos se pierden.
 
### ¿Qué consecuencias tendría para la empresa perder el volumen de PostgreSQL?
Perdería todas las incidencias registradas, sin posibilidad de recuperarlas si no hay copia de seguridad, y la aplicación empezaría de cero.