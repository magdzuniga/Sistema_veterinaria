
Proyecto : Sistema de registro para veterinaria

Requisitos base del proyecto :

1-minimo 10 microservicios

Microservicio de Clientes y Propietarios: gestiona la información personal de los dueños de las mascotas, sus datos de contacto y métodos de pago.
Microservicio de Mascotas (Pacientes): administra el perfil de cada animal (nombre, especie, raza, edad, peso) y su vinculación con el propietario.
       
Microservicio de Citas (Agendamiento): Se encarga de la disponibilidad de los consultorios, calendarios de los veterinarios y reservas de los clientes.
    
Microservicio de Historial Clínico: Almacena las consultas pasadas, diagnósticos, vacunas aplicadas, cirugías y notas médicas.
    
Microservicio de Facturación y Pagos: Gestiona la creación de recibos, integración con pasarelas de pago y cuentas por cobrar.
    
Microservicio de Inventario y Farmacia: Controla el stock de medicamentos, alimentos, alertas de caducidad y reabastecimiento.
    
Microservicio de Laboratorio: Maneja las órdenes de exámenes (sangre, rayos X) y el almacenamiento de los resultados o imágenes enviadas por laboratorios externos.
    
Microservicio de Hospitalización: Para clínicas grandes, gestiona qué mascotas están internadas, en qué jaula/sala se encuentran, y su monitoreo por hora.
    
Microservicio de Identidad y acceso: Su único objetivo es responder a dos preguntas: ¿Quién eres? (Autenticación) y ¿A qué tienes permiso de acceder? (Autorización).

