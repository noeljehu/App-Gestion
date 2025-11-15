# 📱 FiadosApp

![Flutter](https://img.shields.io/badge/Flutter-02569B?style=flat&logo=flutter&logoColor=white)
![Firebase](https://img.shields.io/badge/Firebase-FFCA28?style=flat&logo=firebase&logoColor=black)
![Free Tier](https://img.shields.io/badge/Plan-Free%20Tier-green)

Aplicación móvil para **gestionar vendedores y clientes con fiado**, usando **Firebase Free Tier**.  
Cada vendedor tiene su propia cuenta y solo puede ver sus clientes y apuntes.  
El administrador puede ver todos los vendedores y datos agregados.

---

## ✅ Requerimientos Funcionales

| Código | Funcionalidad | Descripción |
|--------|---------------|------------|
| **RF1** | 🔐 Autenticación | Registro e inicio de sesión con correo y contraseña. Roles: **Administrador** y **Vendedor**. Recuperación de contraseña. |
| **RF2** | 🧾 Gestión de Vendedores | Solo admin: crear, activar/desactivar y ver lista de vendedores. |
| **RF3** | 👥 Gestión de Clientes | Registrar nuevos clientes, editar datos, ver lista propia y total pendiente. |
| **RF4** | 💰 Gestión de Fiados / Apuntes | Registrar fiados, registrar pagos parciales o totales, ver historial completo, actualizar deuda automáticamente. |
| **RF5** | 🔑 Acceso por Rol | Vendedor solo accede a su información; admin puede ver datos globales o agregados. |
| **RF6** | 🗂️ Base de Datos | Estructura `vendedores → clientes → apuntes`. Actualización en tiempo real con Firestore. |
| **RF7** | 🌐 Sincronización Offline | La app funciona sin internet y sincroniza automáticamente al reconectarse. |
| **RF8** | ⚠️ Límite de Uso | La app debe bloquear operaciones si se acercan a los límites gratuitos de Firebase para evitar cobros. |
| **RF9** | 🛠️ Monitoreo | Mostrar en la app alertas de límite de uso, lecturas/escrituras y almacenamiento cercano al máximo permitido en el plan Spark. |

---

## ⚙️ Requerimientos No Funcionales

| Código | Categoría | Descripción |
|--------|-----------|------------|
| **RNF1** | 🔒 Seguridad | Reglas de Firestore que impidan acceso a datos de otros vendedores. HTTPS obligatorio. Rol admin con permisos especiales. |
| **RNF2** | ☁️ Uso Responsable Firebase | Consultas filtradas por vendedor. Uso de `limit()`, paginación y caché local. Evitar Cloud Functions, Storage pesado y Phone Auth (SMS). |
| **RNF3** | 🚀 Rendimiento | Carga de clientes < 300 ms usando caché. Operaciones ligeras y sin duplicar datos. |
| **RNF4** | 📈 Escalabilidad | Base de datos preparada para agregar más vendedores sin reestructurar. Colecciones separadas por vendedor. |
| **RNF5** | 🎨 Usabilidad | Interfaz simple e intuitiva. Botones claros para registrar fiados y pagos. Indicadores de estado: cargando, sin internet, sincronizando. |
| **RNF6** | 🔧 Mantenibilidad | Código modular: autenticación, clientes, apuntes, servicios Firebase. Documentación breve de flujo y estructura de datos. |
| **RNF7** | ⚠️ Control de Costos | Evitar cobros en Firebase Spark mediante: <br> • Limitación de lecturas y escrituras por usuario. <br> • Eliminación de datos innecesarios. <br> • Restricción de subida de archivos grandes (>200KB). <br> • Bloqueo automático de acciones si se supera un porcentaje del límite gratuito. |
| **RNF8** | 🧾 Monitor de Uso | La app debe registrar en logs internos las acciones críticas (creación de clientes, fiados, pagos) para auditar el uso y evitar exceder límites de cuota. |

---

### 🔹 Notas Importantes

- Cada vendedor solo verá su propia información; el admin tendrá acceso a datos agregados.  
- Todas las consultas y escrituras están optimizadas para **evitar cobros en Firebase Free Tier**.  
- Se recomienda revisar periódicamente el **panel de uso de Firebase** para controlar lecturas, escrituras y almacenamiento.  
- Preparado para escalar a un plan pago si se requiere mayor capacidad.

---
