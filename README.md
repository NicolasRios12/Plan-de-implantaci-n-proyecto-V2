# Plan-de-implementacion-proyecto-V2
Aquí tienes una guía completa y estructurada para tu proyecto **Dog Club** en Flutter + Firebase. Todo está diseñado como prototipo/MVP (no orientado a producción) y adaptado a tus tablas relacionales.

---

## 🎨 Paleta de colores centrada en azules + blanco
Diseñada bajo principios Material 3, con contraste WCAG AA y enfoque en legibilidad.

| Token / Uso             | Hex        | Descripción de aplicación                              |
|-------------------------|------------|--------------------------------------------------------|
| `primary`               | `#1565C0`  | AppBar, botones principales, iconos activos            |
| `primaryLight`          | `#42A5F5`  | Estados hover/pressed, chips seleccionados, bordes     |
| `primaryDark`           | `#0D47A1`  | Textos sobre fondos claros, sombras, estados inactivos |
| `surface`               | `#FFFFFF`  | Tarjetas, diálogos, fondos de formularios              |
| `background`            | `#F5F9FF`  | Fondo general de la app (azul muy suave + blanco)      |
| `textPrimary`           | `#0A192F`  | Títulos, cuerpo principal, etiquetas de formularios    |
| `textSecondary`         | `#546E7A`  | Subtítulos, metadatos, placeholders                    |
| `error`                 | `#D32F2F`  | Validaciones, incidencias de severidad `alta`          |
| `success`               | `#2E7D32`  | Estados `confirmada`/`completada`, pagos `pagado`      |
| `divider`               | `#E0E7EF`  | Líneas separadoras, bordes sutiles de tarjetas         |

**Implementación en `theme.dart` (Material 3):**
```dart
ThemeData(
  colorScheme: ColorScheme.fromSeed(
    seedColor: const Color(0xFF1565C0),
    primary: const Color(0xFF1565C0),
    secondary: const Color(0xFF42A5F5),
    surface: Colors.white,
    background: const Color(0xFFF5F9FF),
    onSurface: const Color(0xFF0A192F),
  ),
  appBarTheme: const AppBarTheme(
    backgroundColor: Color(0xFF1565C0),
    foregroundColor: Colors.white,
    elevation: 0,
  ),
  cardTheme: CardTheme(color: Colors.white, elevation: 2),
  inputDecorationTheme: InputDecorationTheme(
    fillColor: Colors.white,
    filled: true,
    border: OutlineInputBorder(borderRadius: BorderRadius.circular(12)),
  ),
)
```

---

## 📦 Dependencias para `pubspec.yaml`
*(Versiones con `^` para compatibilidad con 2024-2026. Verifica siempre `pub.dev` antes de instalar)*

```yaml
dependencies:
  flutter:
    sdk: flutter

  # 🔥 Firebase
  firebase_core: ^3.6.0
  firebase_auth: ^5.3.1
  cloud_firestore: ^5.4.4
  firebase_storage: ^12.3.2

  # 🧩 State Management & Routing
  provider: ^6.1.2
  go_router: ^14.2.0

  # 🖼️ UI & Assets
  cached_network_image: ^3.3.1
  flutter_svg: ^2.0.10+1
  lottie: ^3.1.2
  shimmer: ^3.0.0

  # 📝 Forms & Validation
  flutter_form_builder: ^9.4.1
  form_builder_validators: ^10.0.1

  # 📅 Utils & Tools
  intl: ^0.19.0
  shared_preferences: ^2.3.2
  uuid: ^4.4.2
  image_picker: ^1.1.2
  firebase_ui_firestore: ^1.6.2

dev_dependencies:
  flutter_test:
    sdk: flutter
  flutter_lints: ^4.0.0
```

---

## 🌳 Estructura del proyecto (árbol)
Arquitectura por capas + módulos por entidad. Lista para escalar a feature-first si el proyecto crece.

```
dog_club/
├── android/ ios/ web/ linux/ windows/ macos/
├── assets/
│   ├── icons/
│   ├── images/
│   └── lottie/
├── lib/
│   ├── main.dart
│   ├── core/
│   │   ├── constants/
│   │   │   ├── app_colors.dart
│   │   │   ├── app_strings.dart
│   │   │   └── app_routes.dart
│   │   ├── theme/
│   │   │   └── app_theme.dart
│   │   ├── utils/
│   │   │   ├── formatters.dart
│   │   │   └── validators.dart
│   │   └── config/
│   │       └── firebase_options.dart
│   ├── data/
│   │   ├── models/
│   │   │   ├── cliente_model.dart
│   │   │   ├── mascota_model.dart
│   │   │   ├── servicio_model.dart
│   │   │   ├── empleado_model.dart
│   │   │   ├── reservacion_model.dart
│   │   │   ├── pago_model.dart
│   │   │   └── incidencia_model.dart
│   │   ├── repositories/
│   │   │   ├── auth_repository.dart
│   │   │   ├── cliente_repository.dart
│   │   │   ├── mascota_repository.dart
│   │   │   ├── servicio_repository.dart
│   │   │   └── reservacion_repository.dart
│   │   └── services/
│   │       └── firebase_service.dart
│   ├── domain/
│   │   └── entities/ (opcional si separas DTOs de lógica pura)
│   ├── presentation/
│   │   ├── screens/
│   │   │   ├── auth/
│   │   │   │   ├── login_screen.dart
│   │   │   │   └── register_screen.dart
│   │   │   ├── home/
│   │   │   │   └── home_screen.dart
│   │   │   ├── pets/
│   │   │   │   ├── pet_list_screen.dart
│   │   │   │   └── pet_detail_screen.dart
│   │   │   ├── services/
│   │   │   │   ├── service_catalog_screen.dart
│   │   │   │   └── service_booking_screen.dart
│   │   │   ├── bookings/
│   │   │   │   └── my_bookings_screen.dart
│   │   │   ├── payments/
│   │   │   │   └── payment_history_screen.dart
│   │   │   └── profile/
│   │   │       └── profile_screen.dart
│   │   ├── widgets/
│   │   │   ├── app_button.dart
│   │   │   ├── custom_appbar.dart
│   │   │   ├── loading_skeleton.dart
│   │   │   ├── pet_card.dart
│   │   │   └── status_badge.dart
│   │   └── providers/
│   │       ├── auth_provider.dart
│   │       ├── pet_provider.dart
│   │       └── booking_provider.dart
│   └── routes/
│       └── app_router.dart
├── pubspec.yaml
├── analysis_options.yaml
└── README.md
```

---

## 📱 Diseño UI/UX (Descripción amplia)

### 1. Arquitectura de navegación
- **BottomNavigationBar** con 4 ítems: `Inicio`, `Mascotas`, `Reservar`, `Perfil`.
- **FAB flotante** (`+ Reservar rápido`) en `Inicio` y `Mascotas`.
- **GoRouter** maneja deep links, autenticación guardada y rutas protegidas.
- Transiciones suaves (`SlideTransition` / `FadeTransition`) entre pantallas.

### 2. Flujo de usuario principal (UX)
1. **Onboarding/Auth**: 
   - Splash → Login/Registro con email o Google.
   - Si es nuevo, wizard de 3 pasos: datos cliente → registro primera mascota → preferencias de servicio.
2. **Home**: 
   - Banner superior con saludo + foto mascota activa.
   - Grid de servicios destacados (`guarderia`, `paseo`, `cuidado`).
   - Sección "Próxima reservación" con countdown y estado.
   - Lista horizontal de mascotas registradas (scrollable).
3. **Catálogo & Reserva**:
   - Filtros por categoría, precio, duración.
   - Tarjetas con imagen, descripción, precio base y badge `activo`.
   - Al seleccionar: formulario con selección de mascota, fecha/hora (`intl` + `DateTimePicker`), asignación opcional de empleado, campo `notas`.
   - Resumen antes de confirmar → mock de pago.
4. **Mis Reservaciones**:
   - Lista vertical con `ListView.separated`.
   - Cada ítem muestra: mascota, servicio, fechas, estado con color (`pending`=azul claro, `confirmada`=verde, `cancelada`=rojo suave).
   - Swipe para `Cancelar` o `Ver detalles`.
5. **Perfil & Mascotas**:
   - CRUD de mascotas (foto con `image_picker`, subida a Firebase Storage).
   - Historial de pagos e incidencias vinculadas.
   - Configuración de notificaciones y cierre de sesión.

### 3. Sistema visual (UI)
- **Tipografía**: `Inter` o `Roboto` (importada vía `google_fonts`). Títulos `20-24sp`, cuerpo `16sp`, metadatos `14sp`.
- **Tarjetas**: `borderRadius: 16`, sombra suave `elevation: 2`, padding interno `16`. Fondo blanco sobre `background` azul muy claro para crear profundidad sin saturar.
- **Botones**: 
  - Primarios: `primary` (`#1565C0`) con texto blanco, `height: 48`, `borderRadius: 12`.
  - Secundarios: borde `primary`, fondo transparente, texto `primary`.
  - Estados: `pressed` → `primaryDark`, `disabled` → `textSecondary` con opacidad `0.5`.
- **Formularios**: `FormBuilder` con validaciones en tiempo real. Campos con iconos a la izquierda, labels arriba, helper/error debajo. Teclado numérico para `edad`, `precio`, `teléfono`.
- **Badges de estado**: Pill shape, fondo semitransparente del color de estado, texto en mayúsculas `12sp`.
- **Loading/Empty States**: 
  - `Shimmer` para listas mientras carga Firestore.
  - Ilustraciones Lottie para "Sin mascotas", "Sin reservaciones".
  - Snackbars para éxito/error (3s, dismissible, con acción `DESHACER`).

### 4. Accesibilidad & UX Patterns
- Contraste mínimo `4.5:1` entre texto y fondo.
- Target táctil ≥ `44x44px`.
- Semántica: `Semantics(label: "...")` en botones e íconos.
- Soporte a modo claro/oscuro preparado (aunque la paleta base es azul/blanco).
- Feedback háptico ligero en confirmaciones de reserva/pago (solo en desarrollo).
- Manejo de errores de red: retry button, offline indicator (banner azul claro en top).

### 5. Adaptación de tu esquema relacional a Firestore (NoSQL)
| Tabla SQL          | Colección Firestore       | Notas de adaptación                                     |
|--------------------|---------------------------|---------------------------------------------------------|
| CLIENTES           | `users/{uid}`             | `uid` de Auth como PK. Campos planos.                   |
| MASCOTAS           | `users/{uid}/pets/{petId}`| Subcolección para aislamiento y consultas rápidas.      |
| SERVICIOS          | `services/{id}`           | Colección global. Filtrado por `categoria` y `activo`.  |
| EMPLEADOS          | `employees/{id}`          | Global. Asignación por `reservacion`.                   |
| RESERVACIONES      | `bookings/{id}`           | Contiene `petId`, `serviceId`, `employeeId`, `estado`.  |
| PAGOS              | `bookings/{id}/payments/` | Subcolección. Múltiples pagos por reserva si aplica.    |
| INCIDENCIAS        | `bookings/{id}/incidents/`| Vinculada por `reservationId`.                          |

*(Firestore no usa INT autoincremental. Se usa `DocumentReference.id` o `UUID`. Las FK se reemplazan por IDs de documento o `DocumentReference`.)*

---

## ⚠️ Notas importantes (Entorno NO PRODUCTIVO)
1. **Seguridad**: Las reglas de Firestore estarán abiertas (`allow read, write: if true;`) solo para prototipado. En producción requieren validación de `request.auth != null` y ownership checks.
2. **Pagos**: Se usa flujo simulado (mock UI + estado `pendiente/pagado`). Sin integración real con Stripe/MercadoPago.
3. **Notificaciones**: Firebase Cloud Messaging no configurado. Se simula con `showDialog` o `Snackbar`.
4. **Imágenes**: `firebase_storage` con reglas públicas. En producción se requiere compresión, CDN y políticas de retención.
5. **Testing**: No se incluyen unit/widget tests. Recomendable agregarlos antes de cualquier lanzamiento.
6. **Antigravity**: Asumo que es tu IDE/local wrapper. Todo el código es 100% compatible con Flutter estándar + cualquier editor (VS Code, Android Studio, Cursor, etc.).

---
## Prompt
Actua como un creador de software, diseñador de aplicaciones moviles multiplataforma.
Proyecto de Guarderia canina llamado "Dog Club", el objetivo de esta app es que los dueños de los perros puedan buscar cuidadores, paseadores o guarderias para sus mascotas. La paleta de colores sera centrada en azules, como diseñador de aplicaciones moviles seras el encargado de generar una paleta de colores centrada en azules para appbar, botones y el blanco como apoyo para distiguir a apreciar otras cosas.
El entorno de trabajo que usare sera la aplicacion antigravity vinculada con una base de datos en firebase console, me proporcionaras la lista de dependencias que necesitare para el pubspec.yaml, como ayuda para la generacion de este proyecto te dejare las tablas que planeo usar para este proyecto con sus campos, tipo de dato y descripcion:

CLIENTES
Dueños de las mascotas registradas
Atributo	Tipo	Restricción	Descripción
id_cliente	INT	PK NN	Identificador único autoincremental
nombre	VARCHAR(80)	NN	Nombre(s) del cliente
apellido	VARCHAR(80)	NN	Apellido(s) del cliente
telefono	VARCHAR(20)	NN	Teléfono de contacto principal
email	VARCHAR(120)	UQ	Correo electrónico
direccion	VARCHAR(200)	—	Domicilio del cliente
fecha_registro	DATE	NN	Fecha de alta en el sistema
MASCOTAS
Perros registrados en Dog Club
Atributo	Tipo	Restricción	Descripción
id_mascota	INT	PK NN	Identificador único autoincremental
id_cliente	INT	FK NN	Referencia a CLIENTES
nombre	VARCHAR(60)	NN	Nombre de la mascota
raza	VARCHAR(80)	—	Raza del perro
tamanio	ENUM	NN	Pequeño / mediano / grande
edad	TINYINT	—	Edad en años
notas_medicas	TEXT	—	Alergias, condiciones especiales
foto_url	VARCHAR(255)	—	URL de la foto del perro
SERVICIOS
Catálogo de servicios ofrecidos
Atributo	Tipo	Restricción	Descripción
id_servicio	INT	PK NN	Identificador único autoincremental
nombre	VARCHAR(100)	NN	Nombre del servicio
descripcion	TEXT	—	Detalle del servicio ofrecido
categoria	ENUM	NN	guarderia / cuidado / paseo
precio_base	DECIMAL(8,2)	NN	Precio estándar del servicio
duracion_min	SMALLINT	—	Duración estimada en minutos
activo	BOOLEAN	NN	Disponible para reservar (borrado lógico)
EMPLEADOS
Personal que atiende los servicios
Atributo	Tipo	Restricción	Descripción
id_empleado	INT	PK NN	Identificador único autoincremental
nombre	VARCHAR(80)	NN	Nombre(s) del empleado
apellido	VARCHAR(80)	NN	Apellido(s) del empleado
rol	ENUM	NN	cuidador / paseador / groomer / admin
telefono	VARCHAR(20)	—	Teléfono de contacto
activo	BOOLEAN	NN	Empleado vigente (borrado lógico)
RESERVACIONES
Tabla central del negocio
Atributo	Tipo	Restricción	Descripción
id_reservacion	INT	PK NN	Identificador único autoincremental
id_mascota	INT	FK NN	Referencia a MASCOTAS
id_servicio	INT	FK NN	Referencia a SERVICIOS
id_empleado	INT	FK	Referencia a EMPLEADOS (asignable)
fecha_inicio	DATETIME	NN	Inicio del servicio
fecha_fin	DATETIME	—	Fin estimado / real del servicio
estado	ENUM	NN	pendiente / confirmada / en_curso / completada / cancelada
precio_final	DECIMAL(8,2)	—	Precio real cobrado (permite descuentos)
notas	TEXT	—	Instrucciones especiales del cliente
PAGOS
Registro de cobros por reservación
Atributo	Tipo	Restricción	Descripción
id_pago	INT	PK NN	Identificador único autoincremental
id_reservacion	INT	FK NN	Referencia a RESERVACIONES
monto	DECIMAL(8,2)	NN	Cantidad pagada
metodo	ENUM	NN	efectivo / tarjeta / transferencia
fecha_pago	DATETIME	NN	Fecha y hora del pago
estado	ENUM	NN	pendiente / pagado / reembolsado
INCIDENCIAS
Eventos durante la prestación del servicio
Atributo	Tipo	Restricción	Descripción
id_incidencia	INT	PK NN	Identificador único autoincremental
id_reservacion	INT	FK NN	Referencia a RESERVACIONES
descripcion	TEXT	NN	Descripción del evento ocurrido
severidad	ENUM	NN	baja / media / alta
fecha_hora	DATETIME	NN	Fecha y hora del incidente
PK Llave primaria
FK Llave foránea
NN NOT NULL
UQ UNIQUE.
Tambien me vas a generar un arbol de la estructura del proyecto con todos los archivos para saber como quedara estructurado mi proyecto.
tambien dame el Diseño UI y UX, describelo ampliamente
No hacer para produccion


