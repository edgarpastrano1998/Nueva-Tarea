# Sección 1: Estrategia y Alcance
## Proyecto: Plataforma para reproducir y descargar música

**Atributo:** Funcionalidad

**Justificación:** Los usuarios necesitan acceder fácilmente a música de distintos géneros y artistas para entretenimiento y personalización de su experiencia.

**Escenario:** Cuando el usuario busque una canción o artista en la plataforma (**Estímulo**), el sistema deberá mostrar un catálogo completo y organizado con resultados relacionados, permitiendo reproducir o descargar la música seleccionada (**Respuesta**), logrando que la búsqueda se complete en menos de 3 segundos y mostrando al menos el 95% de coincidencias relevantes (**Métrica**).

### Requerimientos Funcionales

| ID | Requerimiento Funcional | Descripción |
| :--- | :--- | :--- |
| **RF-01** | Registro y Login | El sistema debe permitir a los usuarios crear una cuenta con correo/contraseña y mantener una sesión segura. |
| **RF-02** | Reproducción de Audio | El sistema debe permitir reproducir, pausar, saltar (next) y regresar (previous) pistas de audio. |
| **RF-03** | Búsqueda de Catálogo | El usuario debe poder buscar canciones, álbumes o artistas mediante palabras clave en una barra de búsqueda. |
| **RF-04** | Creación de Playlists | El usuario debe poder crear listas de reproducción personalizadas y añadir canciones a las mismas. |
| **RF-05** | Streaming en Segundo Plano | La aplicación debe seguir reproduciendo música incluso si la pantalla del dispositivo se apaga o el usuario minimiza la app. |

### Requerimientos No Funcionales (RNF)
Definen cómo debe comportarse el sistema (restricciones y propiedades del sistema).

* **RNF-01 (Disponibilidad):** El servicio de streaming debe tener un tiempo de actividad (uptime) del 99.5% durante el mes.
* **RNF-02 (Seguridad):** Las contraseñas de los usuarios deben encriptarse en la base de datos utilizando algoritmos hash seguros (ej. bcrypt).
* **RNF-03 (Compatibilidad):** La aplicación debe ser compatible con las versiones de Android 10+ e iOS 15+, o ser accesible vía navegadores web modernos (Chrome, Safari, Firefox).
* **RNF-04 (Escalabilidad de Almacenamiento):** El almacenamiento de archivos de audio debe estar desacoplado del servidor web (ej. usando AWS S3) para permitir el crecimiento del catálogo sin degradar el rendimiento.

---

# Sección 2: Vista Estructural (C4)

## Aplicación de Streaming Musical
El siguiente documento presenta la arquitectura de una aplicación de streaming musical utilizando el Modelo C4. Se desarrollan los niveles 1 y 2 para representar el contexto general del sistema y los contenedores principales de la solución.

### Nivel 2 — Diagrama de Contenedores

```mermaid
flowchart LR
    U["Persona<br>Usuario"]
    F["Frontend App Móvil<br>[Flutter]<br>Interfaz gráfica para reproducir música y administrar descargas"]
    B["Backend API<br>[Python / Flask]<br>Procesa autenticación, playlists y gestión de canciones"]
    DB["Base de Datos<br>[MySQL]<br>Almacena usuarios, playlists y registros"]
    ST["Almacenamiento Local<br>[Android/iOS Storage]<br>Guarda canciones descargadas en el dispositivo"]

    U -->|"Interactúa con la aplicación"| F
    F -->|"Envía solicitudes de login y música [HTTPS/JSON]"| B
    B -->|"Consulta y almacena información [SQL]"| DB
    F -->|"Guarda canciones descargadas [File System API]"| ST

   Sección 3: Vista de Fronteras y Contratos

    flowchart LR
    U["Persona<br>Usuario"]
    F["Frontend App Móvil<br>[Flutter]<br>Interfaz gráfica para reproducir música y administrar descargas"]
    B["Backend API<br>[Python / Flask]<br>Procesa autenticación, playlists y gestión de canciones"]
    DB["Base de Datos<br>[MySQL]<br>Almacena usuarios, playlists y registros"]
    ST["Almacenamiento Local<br>[Android/iOS Storage]<br>Guarda canciones descargadas en el dispositivo"]

    U -->|"Interactúa con la aplicación"| F
    F -->|"Envía solicitudes de login y música [HTTPS/JSON]"| B
    B -->|"Consulta y almacena información [SQL]"| DB
    F -->|"Guarda canciones descargadas [File System API]"| ST

    Manejo de Fallos: Caja Rota (Fallo de la BD)
Si la base de datos pierde conexión durante 10 minutos, el Backend API no podrá consultar usuarios ni playlists. El Frontend seguirá funcionando parcialmente, permitiendo al usuario navegar por la aplicación y reproducir canciones descargadas localmente.

El usuario será informado mediante mensajes visuales indicando que el servicio se encuentra temporalmente fuera de línea.

Como estrategia de tolerancia a fallos, el sistema implementará manejo de excepciones, reintentos automáticos de conexión y almacenamiento temporal en caché para evitar el colapso total de la aplicación.

Contenedor,Tecnología,Responsabilidad Principal,Despliegue Local
Frontend App Móvil,Flutter,Mostrar interfaz y reproducir música,Emulador Android/iOS
Backend API,Python / Flask,Procesar lógica de negocio y autenticación,localhost:5000
Base de Datos,MySQL,Almacenar usuarios y playlists,Puerto 3306
Almacenamiento Local,Android/iOS Storage,Guardar canciones descargadas,Memoria del dispositivo

#  Diccionario de Datos - Plataforma para Reproducir y Descargar Música

---

## Proyecto
**Plataforma para reproducir y descargar música**

---

## Descripción general
La plataforma permite a los usuarios buscar música, reproducir canciones, descargar contenido y gestionar playlists desde una aplicación móvil conectada a un backend con base de datos.

---

# Tabla: USUARIO

| Campo | Tipo de Dato | Tamaño | PK | FK | Nulo | Descripción |
|------|-------------|--------|----|----|------|-------------|
| id_usuario | INT | - | Sí | No | No | Identificador único del usuario |
| nombre_usuario | VARCHAR | 50 | No | No | No | Nombre de usuario en la plataforma |
| correo | VARCHAR | 100 | No | No | No | Correo electrónico del usuario |
| contraseña | VARCHAR | 255 | No | No | No | Contraseña cifrada del usuario |
| fecha_registro | DATETIME | - | No | No | No | Fecha de creación de la cuenta |

---

# Tabla: ARTISTA

| Campo | Tipo de Dato | Tamaño | PK | FK | Nulo | Descripción |
|------|-------------|--------|----|----|------|-------------|
| id_artista | INT | - | Sí | No | No | Identificador único del artista |
| nombre_artista | VARCHAR | 100 | No | No | No | Nombre del artista o grupo musical |
| pais_origen | VARCHAR | 50 | No | No | Sí | País de origen del artista |
| genero_principal | VARCHAR | 50 | No | No | Sí | Género musical principal |

---

# Tabla: CANCION

| Campo | Tipo de Dato | Tamaño | PK | FK | Nulo | Descripción |
|------|-------------|--------|----|----|------|-------------|
| id_cancion | INT | - | Sí | No | No | Identificador único de la canción |
| titulo | VARCHAR | 150 | No | No | No | Título de la canción |
| duracion | TIME | - | No | No | No | Duración de la canción |
| genero | VARCHAR | 50 | No | No | Sí | Género musical |
| url_audio | VARCHAR | 255 | No | No | No | Ruta o enlace del archivo de audio |
| id_artista | INT | - | No | Sí | No | Artista que interpreta la canción |

---

# Tabla: PLAYLIST

| Campo | Tipo de Dato | Tamaño | PK | FK | Nulo | Descripción |
|------|-------------|--------|----|----|------|-------------|
| id_playlist | INT | - | Sí | No | No | Identificador único de la playlist |
| nombre_playlist | VARCHAR | 100 | No | No | No | Nombre de la playlist |
| fecha_creacion | DATETIME | - | No | No | No | Fecha de creación |
| id_usuario | INT | - | No | Sí | No | Usuario propietario de la playlist |

---

# Tabla: PLAYLIST_CANCION

| Campo | Tipo de Dato | Tamaño | PK | FK | Nulo | Descripción |
|------|-------------|--------|----|----|------|-------------|
| id_playlist | INT | - | Sí | Sí | No | Relación con playlist |
| id_cancion | INT | - | Sí | Sí | No | Relación con canción |
| fecha_agregado | DATETIME | - | No | No | No | Fecha en que se agregó la canción |

 **Clave primaria compuesta:** (id_playlist, id_cancion)

---

# Tabla: DESCARGA

| Campo | Tipo de Dato | Tamaño | PK | FK | Nulo | Descripción |
|------|-------------|--------|----|----|------|-------------|
| id_descarga | INT | - | Sí | No | No | Identificador único de la descarga |
| fecha_descarga | DATETIME | - | No | No | No | Fecha en que se realizó la descarga |
| id_usuario | INT | - | No | Sí | No | Usuario que realiza la descarga |
| id_cancion | INT | - | No | Sí | No | Canción descargada |

---

# Modelo Relacional

```text
USUARIO(
    id_usuario PK,
    nombre_usuario,
    correo,
    contraseña,
    fecha_registro
)

ARTISTA(
    id_artista PK,
    nombre_artista,
    pais_origen,
    genero_principal
)

CANCION(
    id_cancion PK,
    titulo,
    duracion,
    genero,
    url_audio,
    id_artista FK
)

PLAYLIST(
    id_playlist PK,
    nombre_playlist,
    fecha_creacion,
    id_usuario FK
)

PLAYLIST_CANCION(
    id_playlist PK FK,
    id_cancion PK FK,
    fecha_agregado
)

DESCARGA(
    id_descarga PK,
    fecha_descarga,
    id_usuario FK,
    id_cancion FK
)
````

---

# Relaciones entre entidades

| Entidad origen | Tipo de relación | Entidad destino |
| -------------- | ---------------- | --------------- |
| Usuario        | 1 : N            | Playlist        |
| Usuario        | 1 : N            | Descarga        |
| Artista        | 1 : N            | Canción         |
| Playlist       | N : M            | Canción         |
| Canción        | 1 : N            | Descarga        |

---

# Diagrama Entidad-Relación (MER)

```mermaid
erDiagram

    USUARIO ||--o{ PLAYLIST : crea
    USUARIO ||--o{ DESCARGA : realiza

    ARTISTA ||--o{ CANCION : interpreta

    PLAYLIST ||--o{ PLAYLIST_CANCION : contiene
    CANCION ||--o{ PLAYLIST_CANCION : pertenece

    CANCION ||--o{ DESCARGA : descargada
```

---

# Conclusión

Este modelo de datos permite gestionar una plataforma de música con funcionalidades de búsqueda, reproducción, descarga y organización de playlists. La estructura garantiza integridad, escalabilidad y correcta relación entre usuarios, canciones y artistas.

```
