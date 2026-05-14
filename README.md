# Sistema_veterinaria
Sistema de gestión para una veterinaria.

En este achivo se detallaran los microservicios y todo lo necesario para que este sistema sea funcional.

# Dependencias necesarias 
spring web

lombok

mysql drive

validation

spring date jpa

flyway magration

openfeign

# 1. Arquitectura y Fundamentos
   
Una API REST y el protocolo HTTP definen el estándar de internet mediante el cual las aplicaciones web se comunican intercambiando datos de forma rápida y sin estado. En cuanto al diseño de sistemas, los microservicios permiten dividir aplicaciones gigantes (monolitos) en pequeños módulos autónomos. El CSR (Client-Side Rendering) delega al navegador la tarea de dibujar la interfaz visual , mientras que el patrón MVC organiza el código interno del backend separando las responsabilidades de forma fluida: el cliente envía la petición, el Controller la recibe y coordina, el Service aplica la lógica de negocio, el Repository interactúa con la base de datos, y el Model estructura los datos.

# 2. Creación y Configuración de Proyecto

Para iniciar un proyecto, Spring Initializr genera la estructura base y Maven gestiona las descargas de código mediante el archivo pom.xml. Aquí se añaden dependencias clave como Spring Web (para crear APIs), Spring Data JPA (para bases de datos), MySQL Driver (la conexión a la BD), Lombok (para generar código repetitivo automáticamente) y Validation (para comprobar datos). Todo el comportamiento central del proyecto, como puertos y credenciales, se configura en el archivo application.properties para poder ejecutar el programa correctamente.

# 3. Persistencia y Bases de Datos

La persistencia es la capacidad de guardar la información de la aplicación de forma permanente en una base de datos (BD), evitando que se pierda como ocurre con la memoria volátil al apagar el sistema. En lugar de escribir sentencias SQL complejas manualmente, se utiliza un ORM regido por el estándar JPA e implementado por la herramienta Hibernate, el cual traduce clases de Java directamente a tablas. Spring Data JPA facilita esto aún más proporcionando interfaces como JpaRepository, que ya traen los métodos de base de datos listos para usar.

# 4. Configuración de Base de Datos

Esta configuración se realiza en el archivo application.properties apuntando a motores como MySQL u Oracle Cloud. Se establecen credenciales como url, username y password. Además, se define el "acento" de base de datos a usar (dialect), se habilita la visibilidad de consultas (show-sql), y se configura la propiedad ddl-auto que le indica a Hibernate si debe create (crear de cero), create-drop (crear y luego borrar), update (solo actualizar), validate (solo verificar) o none (no hacer nada) con las tablas al arrancar el proyecto.

# 5. Modelado de Entidades JPA

El modelado o mapeo es el proceso de convertir una clase Java en una estructura de base de datos relacional. Se utilizan anotaciones como @Entity y @Table para definir las tablas, y @Column para que los atributos se transformen en las columnas de las mismas. La clave primaria única de la tabla se genera utilizando @Id y @GeneratedValue. Finalmente, la relación lógica entre múltiples tablas (como un usuario con varios pedidos) se define usando @OneToMany, @ManyToOne, @OneToOne, @ManyToMany o tablas intermedias con @JoinTable.

# 6. CRUD Completo

Un CRUD representa las operaciones elementales de cualquier sistema para gestionar datos: Crear mediante POST, Leer mediante GET, Actualizar mediante PUT y Borrar mediante DELETE. Con la interfaz JpaRepository, estas acciones se implementan de forma rápida invocando métodos nativos preprogramados como findAll() para listar todo, findById() para buscar, save() para insertar o actualizar, deleteById() para eliminar y existsById() para comprobar si un dato existe. De ser necesario, se pueden crear métodos de búsqueda personalizados.

# 7. Controllers REST

Los controladores son las puertas de entrada al sistema. Marcados con @RestController y @RequestMapping, exponen los endpoints (las URLs de la API) para que un cliente externo pueda acceder. Utilizan los verbos HTTP mapeándolos con @GetMapping, @PostMapping, @PutMapping o @DeleteMapping. Para recibir la información que el cliente envía hacia el servidor, se usa @RequestBody para capturar objetos JSON, y @PathVariable para extraer variables incrustadas directamente en la ruta de la URL.

# 8. ResponseEntity y HTTP

ResponseEntity es la herramienta de Spring utilizada para construir la respuesta completa que el servidor devuelve al cliente, garantizando que lleve la información correcta junto con su estado HTTP asociado. Es vital devolver los códigos precisos: estados de éxito como 200 OK, 201 CREATED o 204 NO CONTENT; y, si algo sale mal o se emiten mensajes de error, aplicar respuestas como 400 BAD REQUEST, 404 NOT FOUND o un 500 INTERNAL SERVER ERROR para problemas graves en el backend.

# 9. Validaciones

Las validaciones son filtros de seguridad primaria que evitan que datos incorrectos o basura lleguen a la base de datos. Activándolas con la anotación @Valid, Spring revisa las reglas declaradas en los atributos de la entidad mediante etiquetas como @NotNull (para evitar datos nulos), @NotBlank (para evitar espacios en blanco), @Size (para limitar caracteres) o @Email (para forzar formato de correo electrónico). Si alguna de estas reglas no se cumple, el marco de trabajo lanza automáticamente la excepción MethodArgumentNotValidException que deberá ser capturada.

# 10. Manejo de Errores

El sistema debe estar preparado para no colapsar ante fallos previsibles como búsquedas de recursos inexistentes o fallos de validación, utilizando bloques defensivos try/catch. Para evitar repetir este código en múltiples lugares, Spring permite centralizar el manejo de todas las excepciones del proyecto de forma elegante y limpia empleando las anotaciones @ControllerAdvice y @ExceptionHandler, garantizando que la API siempre retorne mensajes de error ordenados.

# 11. Logs

El logging consiste en llevar una bitácora detallada y estructurada de lo que sucede mientras la aplicación se ejecuta. Utilizando librerías estándar de Java como SLF4J y Logback, los desarrolladores pueden registrar eventos rutinarios importantes o trazas de errores críticos. Esto es un pilar fundamental en ambientes de producción para comprender la historia de los fallos y ayudar en la trazabilidad y reparación del sistema.

# 12. Seguridad y JWT

Para asegurar que solo usuarios autorizados utilicen la aplicación, Spring Security gestiona las reglas mediante SecurityFilterChain y AuthenticationManager. Como las APIs REST modernas deben operar sin estado (Stateless), la sesión no se guarda en el servidor. En su lugar, al hacer un login exitoso, se genera un token de acceso llamado JWT (JSON Web Token); el cliente debe enviar este token como un encabezado (Bearer Token) en cada petición posterior para que el JwtAuthenticationFilter verifique su validez antes de entregar la información.

# 13. Migraciones y Scripts SQL

Para gestionar bases de datos en entornos maduros y profesionales está terminantemente prohibido utilizar herramientas de creación automática (ddl-auto). Se aplican sistemas de migraciones mediante herramientas como Flyway o Liquibase, los cuales ejecutan de forma estricta y ordenada scripts en SQL. Las buenas prácticas obligan a tener scripts numerados y versionados donde, ante la necesidad de modificar una tabla, jamás se alteran los scripts que ya se hayan ejecutado previamente, sino que se crea un script de actualización nuevo.

# 14. Comunicación entre Microservicios

Un principio y regla de oro indispensable de esta arquitectura es que bajo ningún motivo un microservicio puede conectarse o acceder de forma directa a la base de datos que pertenece a otro. Cuando las piezas separadas del sistema requieren intercambiar información, efectúan comunicación REST usando utilidades de red como WebClient o Feign Client. Durante ese intercambio, los datos se empaquetan y viajan en estructuras simples llamadas DTOs (Data Transfer Objects).


# 15. Git y GitHub

El uso de sistemas de control de versiones es crucial para almacenar el historial de cambios del código y fomentar el trabajo colaborativo sin pisar el avance de otros miembros. Esto se domina mediante comandos de terminal para preparar el entorno (git init), empaquetar archivos (git add), guardar el estado de progreso conformando un "commit" (git commit), y finalmente sincronizar ese estado empujando los datos a repositorios remotos en la nube, como GitHub (git push).

# 16. Postman y Pruebas

Antes de conectar una interfaz gráfica (el frontend), las APIs se verifican empleando clientes como Postman para emitir solicitudes GET, POST, PUT y DELETE hacia los controladores del proyecto. Este proceso requiere probar exhaustivamente que los códigos devueltos por el protocolo HTTP coincidan con el éxito o fallo de la acción, auditar que las respuestas sean esquemas de JSON limpios, asegurarse de que las validaciones y los mensajes de error funcionen correctamente y, por último, confirmar que la persistencia en la base de datos sea real y sólida.
