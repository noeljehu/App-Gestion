📌 REQUERIMIENTOS FUNCIONALES
RF1 — Autenticación de Usuarios

La aplicación debe permitir registrar y acceder mediante correo y contraseña.

Cada usuario debe tener un rol: admin o vendedor.

El usuario debe poder recuperar su contraseña.

RF2 — Gestión de Vendedores (solo admin)

El administrador debe poder:

Crear vendedores.

Activar o desactivar vendedores.

Ver la lista de todos los vendedores.

RF3 — Gestión de Clientes (por vendedor)

Cada vendedor debe poder:

Registrar clientes nuevos.

Editar los datos de un cliente.

Ver la lista de sus propios clientes.

Ver el total pendiente del cliente.

RF4 — Gestión de Fiados / Apuntes

El vendedor debe poder:

Registrar un nuevo fiado (monto + descripción).

Registrar pagos parciales o totales.

Ver el historial completo de fiados de un cliente.

Actualizar automáticamente la deuda total del cliente.

RF5 — Acceso por Rol

El vendedor solo puede acceder a su propia información.

El administrador puede ver datos globales o agregados.

RF6 — Base de Datos Firestore

Los datos deben almacenarse usando la estructura:
vendedores → clientes → apuntes

La aplicación debe actualizar datos en tiempo real usando Firestore.

RF7 — Sincronización Offline

La aplicación debe funcionar sin conexión a internet.

Al reconectarse, debe sincronizar automáticamente los cambios.

📌 REQUERIMIENTOS NO FUNCIONALES
RNF1 — Seguridad

Las reglas de Firestore deben impedir que un vendedor acceda a información de otro.

Toda comunicación debe estar cifrada mediante HTTPS.

Debe existir un rol admin con permisos especiales.

RNF2 — Uso Controlado del Plan Gratuito Firebase

Las consultas a Firestore deben estar filtradas por vendedor.

Se debe utilizar:

limit() para manejar listas grandes.

Caché local (modo offline).

Paginación para reducir lecturas.

No usar servicios costosos:

Cloud Functions

Firebase Storage pesado

Phone Authentication (SMS)

RNF3 — Rendimiento

La app debe cargar clientes en menos de 300 ms usando caché local.

Las operaciones deben ser ligeras y rápidas.

Las escrituras deben evitar duplicación de datos.

RNF4 — Escalabilidad

La estructura de Firestore debe permitir agregar vendedores sin reestructurar la base de datos.

Las colecciones deben estar organizadas por vendedor para evitar lecturas globales.

RNF5 — Usabilidad

Interfaz simple y fácil de usar.

Botones visibles para registrar fiados y pagos.

Debe mostrar estados claros: cargando, sin internet, sincronizando.

RNF6 — Mantenibilidad

El código debe estar dividido en módulos:

autenticación

clientes

apuntes

servicios Firebase

Debe existir documentación básica del flujo y la base de datos.
