Actividad 4.1 Arquitectura de sofware.
Métodos HTTP Semánticos
En la arquitectura REST, los verbos HTTP no son simples comandos, sino que definen la semántica (el significado) de la acción que se quiere realizar sobre un recurso (como un usuario, un producto, etc.).

GET: Se utiliza exclusivamente para consultar o leer información. No debe modificar el estado del servidor ni de la base de datos (es una operación segura e idempotente).

POST: Se usa para crear un nuevo recurso. Cuando envías datos de un formulario de registro, por ejemplo, usas POST para que el servidor genere ese nuevo registro en la base de datos.

PUT: Se emplea para actualizar o reemplazar por completo un recurso existente. Si el recurso no existe, en algunos diseños puede crearlo. Envías todo el objeto modificado para sustituir al anterior.

DELETE: Como su nombre lo indica, se utiliza para eliminar un recurso específico del servidor.

Estructura JSON
JSON significa JavaScript Object Notation (Notación de Objetos de JavaScript). Aunque nació de la sintaxis de JavaScript, hoy es completamente independiente del lenguaje.

Códigos de Error HTTP
Los códigos de estado HTTP están organizados por familias numéricas para identificar rápidamente el origen del problema.

Errores del Cliente: Indican que la solicitud enviada por el cliente tiene un problema.

Errores del Servidor: Indican que la solicitud del cliente era correcta, pero el servidor falló internamente al intentar procesarla.

Diferencias:
400 Bad Request: El servidor no entiende la petición porque el cliente envió datos inválidos, incompletos o con un formato incorrecto (por ejemplo, un JSON mal estructurado). Es culpa del Front.

404 Not Found: El servidor está en línea, pero el recurso específico que el cliente está solicitando (la URL o el ID del elemento) no existe.

503 Service Unavailable: El servidor no puede atender la petición en ese momento. Generalmente ocurre porque está sobrecargado de tráfico o en mantenimiento técnico. Es un problema del Back.

Especificación OpenAPI
OpenAPI es un estándar para diseñar y documentar REST APIs. Swagger es el conjunto de herramientas interactivas que implementa dicha especificación para renderizarla de forma visual.
Su objetivo principal al entregar planos de software es actuar como una fuente única de verdad. Permite que tanto los desarrolladores de Backend como los de Frontend (y QA) sepan exactamente qué endpoints existen.

Contratos de Interfaz.
Una Interface es una estructura abstracta que define qué debe hacer una clase, pero no cómo lo hace.

Se dice que actúa como un contrato inmutable porque obliga a cualquier clase que la implemente a cumplir estrictamente con los métodos pactados.

Manejo de Excepciones y Resiliencia
Un Stacktrace es el rastro o "huella" de ejecución que deja el código en la consola cuando ocurre una excepción. Muestra la línea exacta del código donde falló, el archivo, la función y toda la cadena de llamadas previas que provocaron el colapso.

¿Por qué NUNCA debe llegar cruda al usuario?
Seguridad Crítica: Un stacktrace expone las entrañas del sistema. Si la base de datos se cae, la consola podría mostrar nombres de tablas, variables de entorno, la IP del servidor, el motor de base de datos usado (PostgreSQL, SQL Server, etc.) e incluso credenciales. Un atacante usaría esta información para vulnerar el sistema.

Experiencia de Usuario: Para un usuario común, ver una pantalla llena de líneas de código técnico incomprensible genera desconfianza, miedo y la sensación de que la aplicación está rota o es insegura.
