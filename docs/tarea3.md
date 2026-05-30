# Tarea 3 - Modelo C4

Nivel 1 — Diagrama de Contexto
## Aplicación de Streaming Musical

El siguiente documento presenta la arquitectura de una aplicación de streaming musical utilizando el Modelo C4. Se desarrollan los niveles 1 y 2 para representar el contexto general del sistema y los contenedores principales de la solución.
```mermaid
flowchart LR

    U["Persona<br>Usuario de la aplicación"]

    S["Sistema de Streaming Musical<br>Permite escuchar música, descargar canciones y administrar listas de reproducción"]

    EXT["Sistema Externo<br>Servicio de autenticación y gestión de cuentas"]

    U -->|"Escucha música y administra playlists"| S

    S -->|"Valida credenciales de usuario"| EXT
```

Nivel 2 — Diagrama de Contenedores
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
```
Caja rota (fallo de la BD)
Si la base de datos pierde conexión durante 10 minutos, el Backend API no podrá consultar usuarios ni playlists. El Frontend seguirá funcionando parcialmente, permitiendo al usuario navegar por la aplicación y reproducir canciones descargadas localmente.

El usuario será informado mediante mensajes visuales indicando que el servicio se encuentra temporalmente fuera de línea.

Como estrategia de tolerancia a fallos, el sistema implementará manejo de excepciones, reintentos automáticos de conexión y almacenamiento temporal en caché para evitar el colapso total de la aplicación.

Diccionario de contenedores
| Contenedor | Tecnología | Responsabilidad Principal | Despliegue Local |
|---|---|---|---|
| Frontend App Móvil | Flutter | Mostrar interfaz y reproducir música | Emulador Android/iOS |
| Backend API | Python / Flask | Procesar lógica de negocio y autenticación | localhost:5000 |
| Base de Datos | MySQL | Almacenar usuarios y playlists | Puerto 3306 |
| Almacenamiento Local | Android/iOS Storage | Guardar canciones descargadas | Memoria del dispositivo |
