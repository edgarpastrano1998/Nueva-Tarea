Actividad 4.1. Simulación: Revisión de Arquitecturas de Software

Métodos HTTP Semánticos:
Los métodos HTTP son instrucciones que se utilizan para indicar qué acción se quiere realizar sobre un recurso dentro de una API o aplicación web.
GET: Se utiliza para consultar o recuperar información. No modifica los datos existentes, solamente los muestra.
POST: Se utiliza para crear o agregar nuevos datos en el servidor.
PUT: Se utiliza para actualizar o reemplazar completamente un recurso que ya existe.
DELETE: Se utiliza para eliminar un recurso o registro almacenado en el servidor.
Por ejemplo, en un sistema de alumnos, GET podría mostrar la lista de estudiantes, POST registrar uno nuevo, PUT actualizar sus datos y DELETE eliminar su registro.


Estructura JSON:
JSON significa JavaScript Object Notation (Notación de Objetos de JavaScript).
Es un formato utilizado para intercambiar información entre aplicaciones, servidores y bases de datos. Los datos se organizan mediante pares de clave y valor, lo que facilita su lectura tanto para las personas como para los programas.
JSON se convirtió en el estándar de la industria porque es más ligero, sencillo y fácil de interpretar que XML. Además, ocupa menos espacio al transmitir datos, requiere menos etiquetas y es compatible prácticamente con cualquier lenguaje de programación moderno.
Por estas razones, la mayoría de las APIs actuales utilizan JSON para enviar y recibir información.



Códigos de Error HTTP:
Los códigos HTTP permiten conocer el resultado de una solicitud realizada a un servidor.
Familia 4xx: Representa errores causados por el cliente (Front-End o usuario).
Familia 5xx: Representa errores causados por el servidor (Back-End).
Diferencias entre algunos códigos comunes
400 Bad Request
 Indica que la solicitud enviada es incorrecta o contiene datos inválidos. El servidor la recibió, pero no puede procesarla debido a un error en la petición.
404 Not Found
 Indica que el recurso solicitado no existe o no pudo encontrarse en la dirección especificada.
503 Service Unavailable
 Indica que el servidor está temporalmente fuera de servicio o saturado y no puede atender solicitudes en ese momento.
La diferencia principal es que el error 400 se debe a una solicitud mal formada, el 404 a que el recurso no existe y el 503 a que el problema se encuentra en el servidor.


Especificación OpenAPI (Swagger):
OpenAPI, conocida popularmente por la herramienta Swagger, es una especificación que permite documentar APIs de manera estandarizada.
Su principal objetivo es describir claramente cómo funciona una API, qué rutas tiene disponibles, qué datos recibe, qué respuestas devuelve y qué errores puede generar.
Puede compararse con un plano de construcción, ya que ayuda a que todos los integrantes del equipo de desarrollo comprendan exactamente cómo debe funcionar la comunicación entre sistemas antes de comenzar a programar.
Además, facilita las pruebas, la documentación y la integración entre diferentes equipos de trabajo.

Contratos de Interfaz (Código):
En programación orientada a objetos, una Interface es una estructura que define qué métodos debe implementar una clase, pero sin especificar cómo deben funcionar internamente.
Se dice que actúa como un "contrato" porque establece reglas que las clases deben cumplir obligatoriamente. De esta manera, diferentes componentes del sistema pueden comunicarse utilizando la misma estructura sin depender de una implementación específica.
Por ejemplo, la lógica de negocio puede trabajar con una interfaz sin importar si los datos provienen de una base de datos MySQL, SQL Server o cualquier otro sistema. Mientras se respete la interfaz, el resto de la aplicación seguirá funcionando correctamente.
Se considera un contrato inmutable porque las reglas definidas por la interfaz deben mantenerse para evitar incompatibilidades entre los distintos componentes del sistema.


Manejo de Excepciones y Resiliencia:
Un Stacktrace es el registro detallado de errores que genera una aplicación cuando ocurre una excepción. Muestra información técnica como el tipo de error, el archivo donde ocurrió, la línea de código involucrada y la secuencia de llamadas que llevaron al problema.
Nunca debe mostrarse directamente al usuario cuando ocurre una falla, especialmente si la base de datos se cae, porque puede revelar información sensible del sistema, como nombres de archivos, rutas internas, tecnologías utilizadas o detalles de la infraestructura.
En lugar de mostrar el Stacktrace completo, la aplicación debe registrar el error internamente para que los desarrolladores puedan analizarlo y presentar al usuario un mensaje sencillo y amigable, por ejemplo: "Ocurrió un problema al procesar la solicitud. Intente nuevamente más tarde."
Esto mejora la seguridad, evita confundir al usuario y facilita el mantenimiento de la aplicación.



Análisis:
La arquitectura de nuestra aplicación de streaming musical se asemeja más al escenario distribuido por HTTP, ya que los contenedores principales se encuentran separados y se comunican mediante solicitudes entre el Frontend y el Backend. En el diagrama C4, la aplicación móvil envía peticiones al Backend API utilizando HTTPS y JSON para realizar operaciones como autenticación, consulta de canciones y administración de listas de reproducción. Este modelo es muy similar al ejemplo del cajero web, donde la interfaz del usuario se comunica con servicios remotos mediante APIs.
Además, el manejo de fallos también sigue una lógica parecida al escenario distribuido. Si la base de datos deja de estar disponible, el Backend API puede devolver mensajes de error controlados al Frontend, evitando que se muestren errores técnicos al usuario. De esta forma, la aplicación continúa ofreciendo algunas funciones, como la reproducción de canciones almacenadas localmente, mientras se implementan mecanismos de resiliencia como manejo de excepciones, reintentos de conexión y uso de caché temporal para mantener la estabilidad del sistema.
