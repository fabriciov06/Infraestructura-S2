# Laboratorio 02

Hoy utilizaremos docker compose para poder desplegar un servicio web y una base de datos

STACK Tecnico

API
- Aplicación JAVA dockerizada
- Build local desde la carpeta ./api (repositorio nmatsui/hello-world-api)
- 3 copias corriendo en paralelo:
  - api01 en puerto 3000
  - api02 en puerto 3001
  - api03 en puerto 3002

BD PostgreSQL
- Imagen oficial postgres:13 con persistencia mediante el volumen administrado pgdata

## COMANDOS
Especificacion de comandos

-Construir las imágenes locales y levantar los 4 contenedores en segundo plano
docker compose up -d --build

-Verificar el estado de los servicios
docker compose ps

-Comprobar el volumen persistente creado
docker volume ls

-Detener y remover los contenedores
docker compose down

## CONFIGURACIONES
-.env
POSTGRES_USER=user
POSTGRES_PASSWORD=password
MESSAGE="Fabricio Vera"

# Actividad
### Tipos de redes en Docker
Docker proporciona diferentes controladores de red según las necesidades de aislamiento y comunicación:

bridge: Controlador por defecto. Crea una red interna virtual dentro del host para que los contenedores puedan comunicarse entre sí utilizando sus nombres de servicio o IPs internas.

host: Elimina el aislamiento de red entre el contenedor y el host anfitrión, permitiendo al contenedor compartir directamente la interfaz y puertos de la máquina.

overlay: Conecta múltiples demonios de Docker entre sí para permitir la comunicación entre contenedores distribuidos en diferentes hosts físicos (empleado en clústeres como Docker Swarm o Kubernetes).

macvlan: Asigna una dirección MAC física al contenedor, permitiendo que aparezca en la red como si fuera un dispositivo físico conectado directamente al router o switch de la red local.

none: Desactiva toda conectividad de red para el contenedor, manteniéndolo en un aislamiento total.

### Tipos de volumen en Docker
Para persistir datos más allá del ciclo de vida de un contenedor, Docker ofrece tres mecanismos:

Named Volumes (Volúmenes administrados): Gestionados íntegramente por Docker dentro del almacenamiento interno del host (/var/lib/docker/volumes/). Son la opción recomendada para bases de datos (como el volumen pgdata usado en este laboratorio) por su portabilidad, aislamiento y rendimiento.

Bind Mounts (Montajes vinculados): Mapean directamente una ruta o carpeta del sistema de archivos local del host dentro del contenedor. Muy utilizados en desarrollo para reflejar cambios de código en tiempo real sin recompilar.

tmpfs Mounts: Almacenan archivos exclusivamente en la memoria RAM del sistema. Al detener el contenedor, los datos se destruyen por completo, siendo óptimos para tokens o datos confidenciales volátiles.

## Evidencias de Ejecución y Despliegue
1. Construcción y descarga de imágenes (docker compose up -d --build)
Se descargó la imagen oficial de PostgreSQL 13 desde Docker Hub y se inició el proceso de compilación local de las tres réplicas del servicio web a partir del Dockerfile ubicado en ./api.
<img width="886" height="239" alt="image" src="https://github.com/user-attachments/assets/d6845704-e92a-451f-8856-cca60d940071" />

2. Creación de red, volumen y arranque de contenedores
Docker Compose provisionó automáticamente la red aislada (infraestructura-s2_default), el volumen administrado de persistencia (infraestructura-s2_pgdata) y arrancó los 4 contenedores en segundo plano sin conflictos.
<img width="634" height="319" alt="image" src="https://github.com/user-attachments/assets/4ab57f74-6543-4af2-930e-b64d92f6aebf" />

3. Verificación de servicios activos (docker compose ps)
Se comprueba que los cuatro contenedores se encuentran en estado Up y con el mapeo correcto de puertos hacia la máquina anfitriona: api01 (3000), api02 (3001), api03 (3002) y la base de datos db (5432).
<img width="1058" height="141" alt="image" src="https://github.com/user-attachments/assets/c2d083e0-9b81-42c7-8d00-73efbae1f5ee" />

4. Verificación de persistencia de datos (docker volume ls)
Se valida mediante la consola que el volumen infraestructura-s2_pgdata existe en el motor de Docker, garantizando que los datos de PostgreSQL persistan aunque los contenedores sean recreados o detenidos.
<img width="886" height="114" alt="image" src="https://github.com/user-attachments/assets/37937c5f-d1fd-4869-b15e-4679e63c1723" />

5. Logs e inicialización de PostgreSQL (Docker Desktop)
Se evidencia a través del visor de logs que el motor de base de datos PostgreSQL 13 arrancó sin errores, inicializó el clúster de datos en el volumen configurado y quedó en estado activo escuchando conexiones en el puerto 5432.
<img width="1591" height="902" alt="image" src="https://github.com/user-attachments/assets/5a9c1f70-e85b-42ec-8408-2c42d8c4f8d0" />
