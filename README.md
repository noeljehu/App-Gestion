1) Resumen del proyecto

FiadosApp es una app móvil para gestionar vendedores y clientes con fiado.
Roles: admin y vendedor.
Backend: Firebase Authentication + Firestore (Plan Spark).
Frontend: Flutter (Android e iOS opcional).

2) Estructura del repositorio (sugerida)
/fiadosapp
├─ android/
├─ ios/
├─ lib/
│  ├─ main.dart
│  ├─ src/
│  │  ├─ auth/
│  │  ├─ models/
│  │  ├─ services/
│  │  ├─ ui/
│  │  └─ utils/
├─ scripts/
├─ docs/
│  └─ architecture.md
├─ .gitignore
├─ README.md      <- este archivo
└─ pubspec.yaml

3) Requisitos previos (local)

Cuenta Google (para Firebase).

Flutter SDK instalado.

Android Studio o VS Code + Android SDK.

Git y cuenta en GitHub.

(Opcional) Dispositivo Android o emulador.

4) Paso a paso — Preparar Firebase
4.1 Crear proyecto Firebase

Accede a console.firebase.google.com y crea un proyecto (ej. fiadosapp).

Añade app Android:

Package name: com.tuempresa.fiadosapp (ejemplo)

Descarga google-services.json y colócalo en android/app/.

(Opcional) Añade app iOS y descarga GoogleService-Info.plist para ios/.

4.2 Habilitar servicios

Authentication → Métodos → Email/Password (activar).

Firestore Database → Crear en modo producción.

(Opcional) Cloud Messaging para notificaciones (es gratis).

4.3 Configurar SHA-1 (Android)

Genera SHA-1 con:

keytool -list -v -keystore ~/.android/debug.keystore -alias androiddebugkey -storepass android -keypass android


Añádela en la configuración de la app Android en Firebase (para integraciones futuras).

5) Paso a paso — Estructura de Firestore (colecciones)

Usar la siguiente estructura:

/usuarios/{uid}
  - nombre
  - email
  - rol: "admin"|"vendedor"
  - creadoEn (timestamp)
  - activo (boolean)

 /vendedores/{vendedorId}
   - nombre
   - telefono
   - estado

 /vendedores/{vendedorId}/clientes/{clienteId}
   - nombre
   - telefono
   - direccion
   - totalPendiente (number)
   - creadoEn
   - actualizadoEn

 /vendedores/{vendedorId}/clientes/{clienteId}/fiados/{fiadoId}
   - monto
   - descripcion
   - fecha
   - pagado (boolean)
   - creadoEn


Adicional:

 /vendedores/{vendedorId}/resumen/{YYYY-MM-DD}
   - totalFiadoDia
   - totalPagadoDia
   - cantidadClientes


Nota: totalPendiente y resumen evitan lecturas masivas.

6) Reglas de seguridad — Copiar y pegar en Firestore Rules
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


Objetivo: que cada vendedor solo acceda a su propio espacio y que el admin use solo los documentos resumen para las vistas generales.

7) Configuración del proyecto Flutter
7.1 Dependencias (pubspec.yaml)

Agregar:

dependencies:
  flutter:
    sdk: flutter
  firebase_core: ^2.#
  firebase_auth: ^4.#
  cloud_firestore: ^4.#
  provider: ^6.0.#
  # otras: intl, shared_preferences, etc.

7.2 Inicializar Firebase en main.dart
void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp();
  runApp(MyApp());
}

7.3 Colocar google-services.json (Android)

android/app/google-services.json

En android/build.gradle y android/app/build.gradle activar plugin google services según docs FlutterFire.

8) Buenas prácticas en el código (para no consumir cuota)

Filtrar siempre por vendedor_id (o usar path /vendedores/{vId}/...) para lecturas.

Usar limit() en consultas largas.

Preferir snapshots() con streams y listeners (firestore cache) en vez de .get() repetidos.

Paginación para listas largas (startAfterDocument).

Actualizar totalPendiente al escribir un fiado o cuando hay un pago — evita sumar en el cliente.

Limitar tamaño de imágenes (200 KB max) o no usar Storage.

Ejemplo de consulta segura:

FirebaseFirestore.instance
  .collection('vendedores')
  .doc(vendedorId)
  .collection('clientes')
  .limit(50)
  .snapshots();

9) Funcionalidades mínimas (MVP) — Checklist para implementar

 Registro/Login (Email+Password)

 Crear/Desactivar vendedores (sólo admin)

 Listar clientes (por vendedor)

 Crear fiado y actualizar totalPendiente

 Registro de abonos y actualizar totalPendiente

 Historial de fiados por cliente

 Resumen diario (documento resumen)

 Firestore Rules aplicadas

 App configurada para modo offline (Firestore cache)

 Límite de creación por vendedor (anti-spam, app-side)

10) Scripts útiles para el repo / comandos

Inicializar repo:

git init
git add .
git commit -m "Init: estructura FiadosApp"
git branch -M main
git remote add origin git@github.com:TU_USUARIO/fiadosapp.git
git push -u origin main


Flutter run:

flutter pub get
flutter run -d emulator-5554

11) Seeds / creacion de admin (manual)

Crea un usuario admin en Firebase Auth (email+password).
Luego en Firestore usuarios/{uid} crea:

{
  "nombre": "Admin",
  "email": "admin@tuapp.com",
  "rol": "admin",
  "creadoEn": Timestamp.now(),
  "activo": true
}

12) Limitaciones y cómo asegurarte de no pagar

No activar Cloud Functions.

No usar Phone Auth (SMS) ni Storage masivo.

Consultas siempre filtradas por vendedor o por documento específico.

Limitar con .limit() y paginación.

Monitor Firebase → Usage (ver lecturas/escrituras) ocasionalmente.

13) Documentación y commits (recomendado)

docs/architecture.md → diagrama y decisiones.

docs/api.md → endpoints (si creas funciones futuras).

Mensajes de commit: feat:, fix:, chore:, docs:.

14) Plantilla de README en GitHub (resumen para el repo)

Incluye al inicio del repo (README.md) el siguiente bloque:

# FiadosApp
App móvil para gestión de vendedores y clientes con fiado.
Tech: Flutter + Firebase (Auth + Firestore)

## Cómo ejecutar
1. Clona el repo
2. Coloca google-services.json en android/app/
3. `flutter pub get`
4. `flutter run`

## Firebase
- Habilitar Auth (Email/Password)
- Crear colecciones según docs
- Copiar reglas Firestore desde docs/firestore.rules

## Licencia
MIT
