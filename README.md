# Sistema de Gestión de Tunomático

## Objetivo General

Desarrollar la transición completa de modelado arquitectónico de un Sistema de Gestión de Turnos Digitales (Tunomático), aplicando buenas prácticas de diseño orientado a objetos, uso de patrones de diseño y representando la arquitectura física mediante diagramas UML.
Descripción General del Sistema

El Tunomático digital es un sistema de hardware y software diseñado para organizar, gestionar y optimizar la espera de clientes en establecimientos con atención presencial.

### Componentes principales:

•	Tótem o Kiosco de Autoservicio
•	Asignación del Turno (ticket físico o digital)
•	Pantallas de Llamado
•	Consola del Ejecutivo
•	Servidor Central

Ventajas:

•	Reduce la percepción del tiempo de espera.
•	Segmenta clientes preferenciales.
•	Genera métricas y estadísticas.
•	Elimina filas físicas.


1.	Diagrama de Casos de Uso UML– Sistema Tunomático

El análisis funcional permitió identificar con claridad los actores involucrados y las funcionalidades críticas del sistema. Se aplicaron correctamente relaciones <<include>> y <<extend>> para reflejar flujos obligatorios y opcionales en el proceso.

Actores identificados:

•	Cliente: Solicita turno, ingresa RUT, recibe comprobante.
•	Operador: Consulta cola, llama turno, atiende cliente.
•	Administrador: Configura sistema, genera reportes.
•	Sistema de Notificaciones: Actor externo que envía alertas automáticas.

Casos de uso destacados y relaciones aplicadas:

•	Solicitar Turno
o	<<include>> Ingresar RUT
o	<<include>> Registrar Turno en Cola
o	<<extend>> Imprimir Comprobante
•	Llamar Turno
o	<<extend>> Enviar Notificación
•	Generar Reporte
o	<<extend>> Exportar PDF

![Implementación UML](https://github.com/msole1529-mp/turnomatico/blob/main/imagenes/casos_uso_tunomatico.png.png)

Justificación técnica:

•	Cliente: Ingresar RUT (<<include>>), Registrar Turno en Cola, <<extend>> Imprimir Comprobante.
•	Operador: Consultar Cola, Llamar Turno (<<extend>> Enviar Notificación), Atender Turno.
•	Administrador: Configurar Sistema, Generar Reporte (<<extend>> Exportar PDF).
•	Sistema externo: actor “Sistema de Notificaciones” conectado a Enviar Notificación.

2.	Diagrama de Clases UML con Patrones Aplicados

   
Este diagrama representa la estructura lógica del sistema Tunomático, mostrando las clases principales y la aplicación de los patrones de diseño requeridos en la asignatura.
Justificación Arquitectónica y Patrones Aplicados
1.	Singleton – Clase Administrador
o	Garantiza una única instancia que controla la configuración del sistema y supervisa la cola de turnos.
o	La relación con Cola y Operador refleja su rol de coordinación y control centralizado.

2.	Prototype – Clase Turno
o	Permite clonar turnos para simular escenarios o replicar procesos frecuentes.
o	Incluye el método clonar() y el atributo estado, vinculado a la enumeración estado turno

3.	Adapter – Clase NotificadorSMS
o	Adapta la interfaz del servicio externo ServicioSMS al sistema interno de notificaciones.
o	La dependencia hacia Turno muestra que el adaptador notifica directamente sobre el estado de un turno.

4.	Bridge – Interfaz Display y clases Pantalla e Impresora
o	Separa la abstracción de la implementación, permitiendo mostrar o imprimir turnos de forma independiente.
o	La interfaz Display define los métodos generales, mientras que Pantalla e Impresora los refinan según el tipo de salida.

![Implementación UML](https://github.com/msole1529-mp/turnomatico/blob/main/imagenes/clases_tunomatico.png.png)

3. Diagrama de Implementación UML

![Implementación UML](https://github.com/msole1529-mp/turnomatico/blob/main/imagenes/implementacion_tunomatico.png.png)

Descripción de los nodos y componentes
1.	Tótem de Autoservicio
o	Contiene la interfaz táctil y el módulo de control de turnos.
o	Se comunica con el servidor central mediante API REST / HTTPS.
2.	PC Ventanillas (Operadores)
o	Ejecutan la aplicación de operador y el lector de turnos.
o	Utilizan HTTP/WebSocket para recibir actualizaciones en tiempo real.
o	Aplican el patrón Bridge para intercambiar pantalla e impresora.
3.	Servidor Central (Local/Cloud)
o	Aloja los servicios principales: GestorTurnos, API REST, Base de Datos, Cache Redis y Servicio de Notificaciones.
o	Implementa los patrones Singleton y Adapter para garantizar unicidad y conexión con servicios externos.
4.	Smartphone Cliente
o	Recibe notificaciones de turnos mediante Firebase/HTTP.
o	Permite al usuario consultar su estado de atención.
5.	PC Administrador
o	Accede al sistema mediante un Dashboard Web.
o	Supervisa métricas, reportes y configuración general del sistema.
El diagrama refleja una arquitectura distribuida, modular y escalable, donde cada nodo cumple una función específica dentro del flujo de atención. La comunicación entre dispositivos y el servidor central se realiza mediante protocolos estándar, garantizando interoperabilidad, seguridad y eficiencia operativa.

Reflexiones Finales
El modelado arquitectónico del sistema Tunomático permitió comprender la importancia de la coherencia entre los niveles funcional, lógico y físico del diseño. La aplicación de patrones de diseño asegura escalabilidad, reutilización y claridad estructural, mientras que la representación UML facilita la comunicación técnica entre desarrolladores y usuarios.
