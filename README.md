# FiadosApp

App móvil para gestionar vendedores, clientes y fiados.
Desarrollada en Flutter y utilizando Firebase Authentication + Firestore dentro del plan gratuito (Spark).

🚀 Características principales

Registro e inicio de sesión con email y contraseña

Roles: Administrador y Vendedor

Gestión de clientes por vendedor

Registro de fiados y pagos

Actualización automática del total pendiente

Resúmenes diarios para el administrador

Uso optimizado de Firestore para no generar costos

📌 1. Requerimientos Funcionales
Autenticación y roles

RF1 — Registrar usuarios con email y contraseña.

RF2 — Iniciar/cerrar sesión.

RF3 — Asignar roles: admin o vendedor.

RF4 — Cada usuario solo accede a sus datos.

Gestión de vendedores (Admin)

RF5 — Crear, editar y desactivar vendedores.

RF6 — Ver estadísticas resumidas de vendedores.

Gestión de clientes (Vendedor)

RF7 — Registrar clientes.

RF8 — Editar datos del cliente.

RF9 — Ver lista de clientes por vendedor.

RF10 — Ver total pendiente del cliente.

Gestión de fiados

RF11 — Crear un fiado por cliente.

RF12 — Registrar pagos/abonos.

RF13 — Actualizar el total pendiente.

RF14 — Ver historial de fiados.

RF15 — Marcar fiado como pagado.

Reportes

RF16 — Generar resumen diario por vendedor.

RF17 — Solo el admin accede al resumen.

📌 2. Requerimientos No Funcionales
Seguridad

RNF1 — Firestore debe tener reglas por usuario y rol.

RNF2 — Comunicación cifrada (HTTPS).

Rendimiento

RNF3 — Consultas filtradas por vendedor.

RNF4 — Paginación en listas (50 items máx).

RNF5 — Uso de caché local de Firestore.

Confiabilidad

RNF6 — Funciona offline gracias a Firestore cache.

RNF7 — Transacciones atómicas en fiados/pagos.

Escalabilidad

RNF8 — Soporte para miles de documentos.

RNF9 — No usar Cloud Functions ni servicios pagados.

Usabilidad

RNF10 — Interfaz simple e intuitiva.

RNF11 — Búsqueda rápida de clientes.

📂 3. Estructura del Proyecto
/fiadosapp
├─ lib/
│  ├─ main.dart
│  ├─ src/
│  │  ├─ auth/
│  │  ├─ models/
│  │  ├─ services/
│  │  ├─ ui/
│  │  └─ utils/
├─ android/
├─ ios/
├─ docs/
│  └─ architecture.md
├─ scripts/
├─ pubspec.yaml
└─ README.md

🔥 4. Configuración de Firebase
Activar:

Authentication → Email/Password

Firestore Database → Modo producción

Descargar:

google-services.json → android/app/

(Opcional) GoogleService-Info.plist → ios/Runner/

🗄 5. Estructura de Firestore
/usuarios/{uid}
  nombre
  email
  rol
  creadoEn
  activo

/vendedores/{vendedorId}
  nombre
  telefono

/vendedores/{vId}/clientes/{clienteId}
  nombre
  telefono
  totalPendiente
  creadoEn
  actualizadoEn

/vendedores/{vId}/clientes/{clienteId}/fiados/{fiadoId}
  monto
  descripcion
  fecha
  pagado
  creadoEn

/vendedores/{vId}/resumen/{YYYY-MM-DD}
  totalFiadoDia
  totalPagadoDia
  cantidadClientes

🔐 6. Reglas de Seguridad para Firestore

Copia y pega esto en Firestore → Rules:

rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    // Usuarios
    match /usuarios/{uid} {
      allow read, write: if request.auth != null && request.auth.uid == uid;
    }

    // Vendedores
    match /vendedores/{vId} {
      allow read, write: if request.auth != null && request.auth.uid == vId;

      match /clientes/{cId} {
        allow read, write: if request.auth != null && request.auth.uid == vId;

        match /fiados/{fId} {
          allow read, write: if request.auth != null && request.auth.uid == vId;
        }
      }

      match /resumen/{diaId} {
        allow read: if isAdmin();
        allow write: if isAdmin();
      }
    }

    function isAdmin() {
      return request.auth != null 
        && exists(/databases/$(database)/documents/usuarios/$(request.auth.uid))
        && get(/databases/$(database)/documents/usuarios/$(request.auth.uid)).data.rol == "admin";
    }
  }
}

⚙️ 7. Instalación del Proyecto
git clone https://github.com/<TU_USUARIO>/fiadosapp.git
cd fiadosapp
flutter pub get


Coloca google-services.json en:

android/app/


Ejecuta:

flutter run

🧪 8. Comandos útiles
flutter pub get
flutter pub upgrade
flutter clean
flutter run
