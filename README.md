✅ Requerimientos Funcionales
Código	Funcionalidad	Descripción
RF1	🔐 Autenticación	Registro e inicio de sesión con correo y contraseña. Roles: Administrador y Vendedor. Recuperación de contraseña.
RF2	🧾 Gestión de Vendedores	Solo admin: crear, activar/desactivar y ver lista de vendedores.
RF3	👥 Gestión de Clientes	Registrar nuevos clientes, editar datos, ver lista propia y total pendiente.
RF4	💰 Gestión de Fiados / Apuntes	Registrar fiados, registrar pagos parciales o totales, ver historial completo, actualizar deuda automáticamente.
RF5	🔑 Acceso por Rol	Vendedor solo accede a su información; admin puede ver datos globales o agregados.
RF6	🗂️ Base de Datos	Estructura vendedores → clientes → apuntes. Actualización en tiempo real con Firestore.
RF7	🌐 Sincronización Offline	La app funciona sin internet y sincroniza automáticamente al reconectarse.
⚙️ Requerimientos No Funcionales
Código	Categoría	Descripción
RNF1	🔒 Seguridad	Reglas de Firestore que impidan acceso a datos de otros vendedores. Comunicación cifrada (HTTPS). Rol admin con permisos especiales.
RNF2	☁️ Uso Responsable Firebase	Consultas filtradas por vendedor. Uso de limit(), paginación y caché local. Evitar Cloud Functions, Storage pesado y Phone Auth (SMS).
RNF3	🚀 Rendimiento	Carga de clientes < 300 ms usando caché. Operaciones ligeras, sin duplicar datos.
RNF4	📈 Escalabilidad	Base de datos preparada para agregar más vendedores sin reestructurar. Colecciones separadas por vendedor.
RNF5	🎨 Usabilidad	Interfaz simple e intuitiva. Botones claros para registrar fiados y pagos. Indicadores de estado: cargando, sin internet, sincronizando.
RNF6	🔧 Mantenibilidad	Código modular: autenticación, clientes, apuntes, servicios Firebase. Documentación breve de flujo y estructura de datos.
🔹 Notas

La estructura modular y las reglas de seguridad aseguran que cada vendedor solo vea su información.

Todo está optimizado para evitar cobros en Firebase Spark.

Ideal para escalar en el futuro a planes pagos si se necesita mayor capacidad.
