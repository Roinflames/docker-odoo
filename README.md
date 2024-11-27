# docker-odoo
POC

Levantar Odoo con Docker es una manera eficiente y rápida de configurar un entorno de desarrollo o producción. A continuación, te guío paso a paso:

1. Requisitos previos
Docker y Docker Compose instalados:

Instala Docker siguiendo las instrucciones oficiales.
Instala Docker Compose siguiendo las instrucciones oficiales.
Verifica las instalaciones:

docker --version
docker-compose --version

2. Crear el archivo docker-compose.yml
Crea un directorio para tu proyecto Odoo y dentro de este un archivo llamado docker-compose.yml:

mkdir odoo-docker
cd odoo-docker
nano docker-compose.yml

Dentro del archivo docker-compose.yml, agrega lo siguiente:

version: '3.8'

services:
  odoo:
    image: odoo:17.0
    container_name: odoo17
    depends_on:
      - db
    ports:
      - "8069:8069"
    volumes:
      - odoo-web:/var/lib/odoo
      - ./addons:/mnt/extra-addons
      - ./config:/etc/odoo
    environment:
      - HOST=db
      - USER=odoo
      - PASSWORD=odoo

  db:
    image: postgres:14
    container_name: odoo-db
    environment:
      POSTGRES_USER: odoo
      POSTGRES_PASSWORD: odoo
      POSTGRES_DB: odoo
    volumes:
      - odoo-db:/var/lib/postgresql/data

volumes:
  odoo-web:
  odoo-db:
3. Crear directorios para datos persistentes
Asegúrate de crear los directorios locales que serán montados en los contenedores:

mkdir addons config
addons/: Aquí puedes colocar módulos personalizados de Odoo.
config/: Puedes agregar un archivo de configuración de Odoo (opcional).

4. Iniciar los contenedores
Ejecuta el siguiente comando para levantar los contenedores:

docker-compose up -d

Esto hará que Docker descargue las imágenes necesarias y configure los servicios.

5. Verificar que Odoo está corriendo
Abre tu navegador y visita: http://localhost:8069
Deberías ver la pantalla de configuración inicial de Odoo, donde puedes crear una base de datos.

6. Administración de los contenedores
Para ver los contenedores en ejecución:

docker ps

Para detener los contenedores:

docker-compose down

Para reiniciar los contenedores:

docker-compose restart

7. Personalización adicional (opcional)
a) Archivos de configuración personalizados
Si necesitas un archivo de configuración específico para Odoo, crea un archivo en config/odoo.conf:

[options]
addons_path = /mnt/extra-addons
db_host = db
db_port = 5432
db_user = odoo
db_password = odoo

Reinicia los contenedores para que los cambios surtan efecto.

b) Agregar módulos personalizados
Coloca tus módulos personalizados en el directorio addons/. Estos serán detectados automáticamente por Odoo.

8. Resolver problemas comunes
El contenedor no se inicia correctamente: Usa este comando para inspeccionar los logs del contenedor:

docker logs odoo17

Problemas de conexión con la base de datos: Verifica que los servicios estén corriendo:

docker ps

Errores en los módulos personalizados: Asegúrate de que los módulos en addons/ sean compatibles con la versión de Odoo que estás usando.

Con estos pasos, tendrás Odoo 17 funcionando con Docker. Si necesitas más ayuda, no dudes en pedírmelo. 🚀