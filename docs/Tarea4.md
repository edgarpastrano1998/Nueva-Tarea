---
config:
  layout: elk
  theme: mc

---
erDiagram
    direction TB
    USUARIO {
        int id_usuario PK "Identificador único del usuario"  
        string nombre_usuario  "Nombre o alias del usuario"  
        string correo  "Correo electrónico"  
        date fecha_registro  "Fecha de registro del usuario"  
        string tipo_suscripcion  "Tipo de plan (Gratis, Premium, etc.)"  
    }

    ARTISTA {
        int id_artista PK "Identificador único del artista"  
        string nombre_artista  "Nombre del artista o grupo"  
        string pais_origen  "País de origen del artista"  
        date fecha_nacimiento  "Fecha de nacimiento o formación"  
        string genero_principal  "Género musical principal"  
    }

    CANCION {
        int id_cancion PK "Identificador único de la canción"  
        string titulo  "Nombre o título de la canción"  
        int id_artista FK "Referencia al artista que interpreta la canción"  
        time duracion  "Duración total de la canción"  
        string genero  "Género musical (opcional)"  
    }

    PLAYLIST {
        int id_playlist PK "Identificador único de la lista de reproducción"  
        string nombre_playlist  "Nombre asignado por el usuario"  
        int id_usuario FK "Referencia al usuario creador"  
        date fecha_creacion  "Fecha de creación de la lista"  
        string privacidad  "Pública o privada" 
    }

    PLAYLIST_CANCION {
        int id_playlist FK "Referencia a la lista de reproducción"
        int id_cancion FK "Referencia a la canción"
    }

    %% Relaciones con notación tipo Crow's Foot
    ARTISTA |o--o{ CANCION : "interpreta"
    USUARIO |o--o{ PLAYLIST : "crea"
    PLAYLIST ||--o{ PLAYLIST_CANCION : "contiene"
    CANCION ||--o{ PLAYLIST_CANCION : "aparece_en"

