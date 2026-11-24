# AGENTS.md — Clínica FISE

## 1. Project overview

Clínica FISE es un sistema integral de gestión clínica, diseñado inicialmente para una clínica de fisioterapia, con una arquitectura modular, escalable, segura y mantenible.

El sistema evolucionará progresivamente mediante la incorporación de módulos de autenticación, autorización, gestión de personas, pacientes, profesionales, citas, historias clínicas, tratamientos, facturación y administración.

### Project metadata

- Project: Clínica FISE
- Backend: NestJS
- Package manager: pnpm
- Language: TypeScript
- Database: PostgreSQL
- ORM: TypeORM
- Database modeling: DBML
- Authentication: JWT
- Authorization: RBAC
- Initial deployment: Vercel
- Initial deployment environment: Staging
- Architecture: Modular Monolith
- Timezone for business operations: America/Lima
- Database timestamps: UTC
- Country: Peru

El despliegue inicial en Vercel es temporal y está destinado a la validación funcional del sistema por parte de la clínica.

La arquitectura no debe depender innecesariamente de Vercel ni de un proveedor específico de infraestructura.

## 2. Agent behavior

Todo agente de inteligencia artificial que trabaje sobre este repositorio debe respetar las instrucciones de este documento.

### Before making changes

Antes de implementar cualquier funcionalidad:

1. Inspeccionar el estado actual del repositorio.
2. Revisar las dependencias y versiones instaladas.
3. Identificar los patrones arquitectónicos existentes.
4. Revisar la documentación relacionada con la tarea.
5. Identificar entidades, servicios y componentes reutilizables.
6. Evaluar el impacto sobre otros módulos.
7. Identificar posibles cambios en la base de datos.
8. Considerar los riesgos de seguridad y compatibilidad.
9. Evitar duplicar funcionalidades existentes.

No asumir que una funcionalidad, dependencia, archivo o configuración existe sin verificarlo.

### Implementation principles

- Priorizar soluciones simples, mantenibles y explícitas.
- Aplicar SOLID, DRY, KISS y YAGNI de manera pragmática.
- Evitar sobreingeniería.
- Evitar abstracciones que no aporten valor real.
- Reutilizar componentes existentes cuando sea apropiado.
- Mantener cambios pequeños, coherentes y enfocados.
- No realizar refactorizaciones ajenas a la tarea solicitada.
- No eliminar funcionalidades existentes sin autorización.
- No sobrescribir cambios del desarrollador.
- No introducir dependencias innecesarias.
- No utilizar datos simulados para ocultar errores de implementación.
- No declarar una tarea como completada si existen errores conocidos o verificaciones pendientes.
- No modificar las decisiones arquitectónicas establecidas sin justificación y aprobación.

Si existe una contradicción entre este documento, la implementación actual y una solicitud nueva, identificarla explícitamente antes de realizar cambios incompatibles.

### Decision authority

El agente puede tomar decisiones técnicas menores necesarias para completar una tarea.

Las siguientes decisiones requieren aprobación explícita:

- Cambiar el ORM.
- Cambiar el motor de base de datos.
- Cambiar la arquitectura principal.
- Introducir microservicios.
- Incorporar infraestructura externa obligatoria.
- Modificar el mecanismo de autenticación.
- Modificar el modelo RBAC.
- Cambiar convenciones globales de base de datos.
- Eliminar tablas o columnas con información existente.
- Ejecutar migraciones destructivas.
- Modificar políticas de seguridad.
- Ejecutar operaciones sobre bases de datos de staging o producción.
- Cambiar contratos públicos de la API de manera incompatible.

## 3. Language and naming conventions

### Source code

Utilizar inglés para:

- Variables.
- Constantes.
- Clases.
- Interfaces.
- Métodos.
- Funciones.
- Archivos.
- Directorios.
- Entidades.
- Tablas.
- Columnas.
- Enumeraciones.
- Permisos.
- Identificadores técnicos.
- Mensajes técnicos de logs.

Utilizar español para:

- Documentación funcional.
- Explicaciones dirigidas al desarrollador.
- Mensajes visibles para usuarios finales.
- Reglas de negocio documentadas.

Los mensajes de error destinados a usuarios deben ser comprensibles y no revelar detalles internos del sistema.

### Naming standards

| Element | Convention | Example |
| --- | --- | --- |
| Classes | PascalCase | UserEntity |
| Interfaces | PascalCase | UserRepository |
| Types | PascalCase | AuthenticatedUser |
| Methods | camelCase | findByEmail |
| Variables | camelCase | userId |
| Constants | UPPER_SNAKE_CASE | MAX_LOGIN_ATTEMPTS |
| Enum names | PascalCase | RecordStatus |
| Enum values | UPPER_SNAKE_CASE | ACTIVE |
| Files | kebab-case | user-session.entity.ts |
| Directories | kebab-case | password-recovery |
| Database schemas | snake_case | auth |
| Database tables | plural snake_case | user_sessions |
| Database columns | snake_case | created_at |
| API routes | kebab-case | /auth/reset-password |
| Permission codes | dot notation | auth.users.create |

No utilizar abreviaturas ambiguas ni nombres genéricos cuando exista una alternativa más descriptiva.

## 4. Application architecture

El proyecto utilizará inicialmente una arquitectura Modular Monolith.

Cada módulo representa un dominio funcional y debe mantener responsabilidades claramente delimitadas.

No introducir microservicios hasta que exista una necesidad técnica o de negocio demostrable.

### Project bootstrap

Al inicializar el backend, seguir la [guía de la plantilla NestJS DDD](GUIA_PLANTILLA_NESTJS_DDD.md). Generar la base con Nest CLI y seleccionar pnpm como gestor de paquetes. Usar `pnpm install` para instalar dependencias, `pnpm add` para incorporarlas, `pnpm exec` para ejecutar herramientas locales y `pnpm run` para los scripts. Versionar `pnpm-lock.yaml` junto con `package.json`.

Adoptar de la plantilla la separación por módulos y capas, la composición en los módulos de Nest y la validación de variables de entorno al arrancar. Para Clínica FISE, conservar las rutas de configuración y persistencia que se indican a continuación, utilizar PostgreSQL con TypeORM e incorporar módulos y dependencias según las necesidades del proyecto.

### Proposed directory structure

```text
src/
├── config/
├── database/
│   ├── data-source.ts
│   ├── migrations/
│   └── seeds/
├── shared/
│   ├── constants/
│   ├── decorators/
│   ├── entities/
│   ├── enums/
│   ├── exceptions/
│   ├── filters/
│   ├── guards/
│   ├── interceptors/
│   ├── interfaces/
│   ├── pipes/
│   ├── security/
│   ├── types/
│   └── utils/
├── modules/
│   ├── auth/
│   ├── core/
│   ├── organization/
│   ├── clinical/
│   ├── scheduling/
│   ├── therapy/
│   ├── billing/
│   ├── inventory/
│   └── audit/
├── app.module.ts
└── main.ts
```

Esta estructura es una referencia arquitectónica. No crear directorios vacíos ni módulos sin una necesidad funcional actual.

### Module structure

Los módulos podrán organizarse de la siguiente manera:

```text
module-name/
├── domain/
│   ├── entities/
│   ├── enums/
│   ├── interfaces/
│   └── repositories/
├── application/
│   ├── services/
│   └── use-cases/
├── infrastructure/
│   ├── entities/
│   ├── repositories/
│   └── services/
├── presentation/
│   ├── controllers/
│   └── dtos/
└── module-name.module.ts
```

La separación de capas debe aplicarse de manera pragmática.

No crear interfaces, adaptadores o casos de uso adicionales únicamente para cumplir formalmente con una estructura de carpetas.

### Domain layer

Responsable de:

- Reglas de negocio.
- Modelos de dominio cuando sean necesarios.
- Contratos de repositorios.
- Enumeraciones de negocio.
- Invariantes del dominio.

Debe evitar dependencias directas de infraestructura cuando se utilice un modelo de dominio independiente.

### Application layer

Responsable de:

- Casos de uso.
- Orquestación de operaciones.
- Coordinación de transacciones.
- Aplicación de reglas de negocio.
- Coordinación entre servicios y repositorios.

### Infrastructure layer

Responsable de:

- Entidades TypeORM.
- Implementaciones de repositorios.
- Persistencia.
- Integraciones externas.
- Adaptadores de infraestructura.

### Presentation layer

Responsable de:

- Controllers.
- DTOs.
- Validación de solicitudes.
- Contratos HTTP.
- Transformación de respuestas.

Los controllers deben ser delgados y delegar la lógica de negocio a la capa de aplicación.

### Module boundaries

- Evitar dependencias circulares entre módulos.
- Exportar únicamente los servicios o contratos necesarios.
- No acceder directamente a repositorios internos de otros módulos sin una justificación arquitectónica.
- No convertir `shared` en un contenedor de lógica de negocio.
- Mantener las reglas de negocio dentro de su dominio correspondiente.
- No utilizar `forwardRef` como solución habitual a problemas de diseño.

## 5. TypeScript standards

- Mantener tipado estricto.
- Evitar `any`.
- Utilizar `unknown` cuando el tipo de un valor sea desconocido.
- Validar datos externos antes de utilizarlos.
- Evitar conversiones de tipos inseguras.
- No deshabilitar reglas de TypeScript o ESLint para ocultar problemas.
- Utilizar `async/await` de manera consistente.
- Manejar correctamente las promesas rechazadas.
- Evitar valores mágicos.
- Utilizar constantes y enumeraciones cuando representen conceptos reutilizables.
- Mantener funciones y métodos con responsabilidades claras.
- Utilizar inyección de dependencias de NestJS.
- No instanciar manualmente servicios que deban ser administrados por NestJS.

## 6. Database architecture

### Database engine

PostgreSQL es el motor de base de datos oficial del proyecto.

TypeORM es el ORM establecido inicialmente para gestionar:

- Entidades.
- Relaciones.
- Repositorios.
- Consultas.
- Transacciones.
- Migraciones.
- Evolución del esquema.

No introducir un segundo ORM sin autorización.

### Source of truth

La estructura operativa de la base de datos estará definida por:

1. Entidades TypeORM.
2. Migraciones versionadas.
3. Restricciones explícitas gestionadas mediante migraciones.

DBML será la documentación arquitectónica y visual del modelo de datos.

Las entidades, migraciones y documentación DBML deben mantenerse consistentes.

No utilizar DBML como mecanismo automático de modificación de bases de datos existentes.

### Database schemas

Organizar las tablas por dominio funcional.

| Schema | Responsibility |
| --- | --- |
| auth | Authentication, authorization and sessions |
| core | Persons, catalogs and common data |
| organization | Branches, employees and professionals |
| clinical | Patients and clinical records |
| scheduling | Appointments and schedules |
| therapy | Physiotherapy and treatments |
| billing | Billing, payments and cash management |
| inventory | Equipment and supplies |
| audit | Cross-domain auditing |

No crear todos los schemas anticipadamente. Incorporarlos cuando se implemente el dominio correspondiente.

No crear tablas de negocio en el schema `public` sin justificación.

Las tablas técnicas administradas por herramientas, como el historial de migraciones, pueden permanecer en un schema técnico o en `public`.

## 7. Database conventions

### Primary keys

Todas las tablas de negocio deben tener una clave primaria llamada `id`.

- Tipo PostgreSQL: `uuid`.
- Tipo TypeScript: `string`.
- Generación: UUID v4 como estándar inicial.
- Default de base de datos: `gen_random_uuid()` cuando corresponda.

No utilizar claves primarias enteras autoincrementales para entidades de negocio.

Las tablas intermedias también deben tener su propio UUID.

Las claves compuestas podrán utilizarse como restricciones únicas adicionales, pero no reemplazarán la clave primaria UUID.

Cuando un identificador deba conocerse antes de persistir una entidad, podrá generarse mediante `crypto.randomUUID()`.

Esto es obligatorio para las sesiones cuyo identificador se incorpora al refresh JWT antes de persistirlo.

Las tablas internas administradas por herramientas externas, como la tabla de migraciones de TypeORM, están exceptuadas de esta convención.

### Data types

Utilizar tipos adecuados de PostgreSQL:

| Purpose | PostgreSQL type |
| --- | --- |
| Identifiers | uuid |
| Timestamps | timestamptz |
| Calendar dates | date |
| Boolean values | boolean |
| General record status | smallint |
| Short text | varchar |
| Long text | text |
| Monetary amounts | numeric |
| Structured metadata | jsonb |
| IP addresses | inet |
| Integer counters | integer |

No utilizar `float`, `real` o `double precision` para importes monetarios.

Definir precisión y escala según el dominio financiero correspondiente.

No utilizar `jsonb` como sustituto de un modelo relacional correctamente diseñado.

### Dates and timezones

- Persistir instantes utilizando `timestamptz`.
- Utilizar UTC para el intercambio y tratamiento técnico de instantes.
- Utilizar ISO 8601 en contratos de API.
- Utilizar `America/Lima` para operaciones de negocio que dependan de la hora local.
- Utilizar `date` para fechas que no representan un instante, como fechas de nacimiento.
- No almacenar fechas como cadenas arbitrarias.
- No depender de la zona horaria local del servidor.

## 8. Audit fields

Todas las tablas de negocio, incluyendo tablas intermedias, deben contener los siguientes campos:

| Column | Type | Nullable |
| --- | --- | --- |
| id | uuid | No |
| created_by | uuid | Yes |
| created_at | timestamptz | No |
| modified_by | uuid | Yes |
| modified_at | timestamptz | Yes |
| status | smallint | No |

### General record status

```typescript
export enum RecordStatus {
  INACTIVE = 0,
  ACTIVE = 1,
  DELETED = 2,
}
```

El estado inicial será `ACTIVE`.

La base de datos debe restringir los valores permitidos a `0`, `1` y `2`.

### Audit entity

Centralizar los campos comunes mediante una entidad abstracta reutilizable, por ejemplo:

```text
src/shared/entities/auditable.entity.ts
```

Nombre sugerido:

```typescript
AuditableEntity
```

Todas las entidades de negocio deben heredar esta estructura cuando sea técnicamente apropiado.

La entidad abstracta no debe generar una tabla independiente.

### Audit rules

- `created_at` se establece durante la creación.
- `created_by` identifica al usuario responsable de la creación.
- `modified_at` representa la última modificación.
- `modified_by` identifica al usuario responsable de la modificación.
- `created_at` y `created_by` no deben sobrescribirse durante actualizaciones normales.
- `modified_at` debe permanecer nulo hasta la primera modificación, salvo que se apruebe otra convención.
- Los identificadores de auditoría deben obtenerse del contexto autenticado.
- No aceptar campos de auditoría directamente desde DTOs públicos.
- Los procesos automáticos y migraciones podrán utilizar valores nulos cuando no exista un usuario responsable.
- Las operaciones de escritura deben mantener correctamente los campos de auditoría, incluyendo actualizaciones masivas.

Los campos `created_by` y `modified_by` deben referenciar `auth.users.id`, salvo una excepción explícitamente documentada.

La entidad base debe mantener las columnas y las relaciones de auditoría de manera consistente, evitando dependencias circulares.

No asumir que declarar una columna UUID crea automáticamente una clave foránea.

### Functional status

El campo `status` representa el estado general del registro.

Cuando una entidad tenga un ciclo de vida propio, utilizar un campo funcional independiente.

Examples:

- `session_status`
- `appointment_status`
- `payment_status`
- `treatment_status`

No mezclar estados de negocio con el estado general de auditoría.

## 9. Logical deletion

La eliminación lógica será el comportamiento predeterminado para entidades de negocio.

```text
status = 2
```

No utilizar eliminación física como comportamiento predeterminado.

Las consultas operativas deben excluir registros eliminados, salvo que la funcionalidad requiera explícitamente consultar el historial.

No confundir un registro inactivo con uno eliminado.

No asumir que TypeORM aplicará automáticamente filtros sobre el campo `status`.

La estrategia de filtrado debe implementarse explícitamente y de manera consistente.

La eliminación física estará reservada para:

- Procesos de mantenimiento autorizados.
- Datos temporales.
- Políticas de retención.
- Obligaciones legales aplicables.
- Casos explícitamente aprobados.

No utilizar `@DeleteDateColumn` ni `softDelete()` como mecanismo principal de eliminación lógica mientras el estándar del proyecto esté basado en `status`.

## 10. Database integrity

La base de datos debe proteger las invariantes críticas del sistema.

Utilizar cuando corresponda:

- Primary keys.
- Foreign keys.
- Unique constraints.
- Check constraints.
- Composite indexes.
- Partial indexes.
- Transactions.
- Not-null constraints.

No depender exclusivamente de validaciones de aplicación para garantizar integridad.

### Foreign keys

Utilizar `RESTRICT` como política inicial para eliminación de registros referenciados.

No utilizar `CASCADE` de manera indiscriminada.

Cualquier eliminación en cascada debe estar justificada por el ciclo de vida de las entidades relacionadas.

### Index naming

| Object | Prefix |
| --- | --- |
| Primary key | pk_ |
| Foreign key | fk_ |
| Unique constraint | uq_ |
| Index | idx_ |
| Check constraint | ck_ |

Examples:

```text
pk_users
fk_user_roles_user_id
uq_users_email
idx_user_sessions_family_id
ck_users_status
```

Los nombres deben ser deterministas y respetar los límites de identificadores de PostgreSQL.

Evitar índices redundantes.

Evaluar índices sobre claves foráneas y columnas utilizadas frecuentemente en filtros, relaciones y ordenamientos.

No crear índices indiscriminadamente sobre todas las columnas.

## 11. Database migrations

### Mandatory configuration

TypeORM debe utilizar:

```typescript
synchronize: false,
migrationsRun: false,
```

Estas propiedades deben mantenerse deshabilitadas en todos los entornos persistentes.

No utilizar `synchronize: true` como mecanismo de evolución de la base de datos.

No ejecutar migraciones automáticamente durante el arranque de NestJS.

### Migration location

```text
src/database/migrations/
```

Las migraciones deben estar centralizadas y versionadas en Git.

### Data source

Mantener un DataSource accesible desde la CLI de TypeORM:

```text
src/database/data-source.ts
```

Compartir la configuración de conexión entre NestJS y la CLI para evitar inconsistencias.

Cuando el proveedor de PostgreSQL utilice un pooler, considerar una conexión directa específica para migraciones.

### Required scripts

El proyecto debe proporcionar comandos equivalentes a:

```bash
pnpm run migration:create
pnpm run migration:generate
pnpm run migration:run
pnpm run migration:show
pnpm run migration:revert
pnpm run migration:run:prod
```

Adaptar los scripts al gestor de paquetes, sistema de módulos y estructura de compilación reales del repositorio.

### Migration workflow

Cuando una tarea requiera modificar la estructura de la base de datos:

1. Actualizar las entidades correspondientes.
2. Verificar el estado de las migraciones existentes.
3. Utilizar una base de datos local con todas las migraciones anteriores aplicadas.
4. Generar una nueva migración.
5. Revisar manualmente el SQL generado.
6. Corregir operaciones destructivas o inconsistentes.
7. Incorporar restricciones adicionales cuando sea necesario.
8. Ejecutar la migración en desarrollo.
9. Verificar la estructura resultante.
10. Ejecutar las pruebas relacionadas.
11. Actualizar DBML y documentación.
12. Versionar conjuntamente entidades, migraciones y documentación.

Example:

```bash
pnpm run migration:generate src/database/migrations/CreateAuthTables

pnpm run migration:run

pnpm run migration:show
```

### Migration rules

- Nunca modificar una migración aplicada en un entorno compartido.
- Nunca eliminar migraciones históricas sin autorización.
- No generar migraciones contra bases de datos de producción.
- No ejecutar migraciones contra staging o producción sin autorización explícita.
- No utilizar `dropSchema` en entornos persistentes.
- No utilizar `schema:drop` como solución a inconsistencias.
- No utilizar migraciones falsas para ocultar diferencias estructurales.
- No eliminar tablas o columnas con datos sin una estrategia de migración.
- Revisar especialmente renombramientos de tablas y columnas.
- Mantener operaciones `up` y `down` cuando sean razonablemente reversibles.
- Documentar migraciones irreversibles.
- Evitar modificar datos existentes sin una estrategia verificable.
- Mantener compatibilidad entre la estructura de la base de datos y las versiones de aplicación que puedan coexistir durante un despliegue.

Una migración generada automáticamente es una propuesta de cambio, no una garantía de seguridad.

### Migration verification

Cuando sea posible, verificar:

- Aplicación de todas las migraciones sobre una base de datos vacía.
- Actualización desde la versión anterior.
- Conservación de datos existentes.
- Ausencia de diferencias inesperadas entre entidades y base de datos.
- Integridad de restricciones e índices.
- Reversión de migraciones cuando corresponda.

Las restricciones personalizadas deben reflejarse en los metadatos de TypeORM cuando sea posible, evitando que futuras migraciones generadas intenten eliminarlas.

### Destructive changes

Utilizar una estrategia expand and contract para cambios incompatibles.

1. Incorporar la nueva estructura.
2. Mantener compatibilidad temporal.
3. Migrar los datos existentes.
4. Actualizar los consumidores.
5. Verificar el funcionamiento.
6. Retirar la estructura anterior mediante una migración posterior autorizada.

No utilizar rollback como sustituto de backups.

## 12. Database modeling with DBML

Mantener un archivo maestro:

```text
database/clinica_fise.dbml
```

El archivo debe representar el modelo arquitectónico actualizado.

Utilizar `TablePartial` para centralizar los campos de auditoría.

Example:

```dbml
TablePartial audit_fields {
  created_by uuid
  created_at timestamptz [not null, default: `now()`]
  modified_by uuid
  modified_at timestamptz
  status smallint [not null, default: 1]
}
```

### DBML rules

- Mantener nombres consistentes con PostgreSQL.
- Representar relaciones y cardinalidades correctamente.
- Documentar restricciones únicas.
- Documentar índices relevantes.
- Mantener schemas separados por dominio.
- Documentar decisiones que no puedan representarse directamente en DBML.
- Actualizar el modelo cuando cambien las entidades o migraciones.
- No introducir diferencias estructurales silenciosas.

Las restricciones avanzadas que DBML no pueda representar deben documentarse mediante notas y mantenerse implementadas en las migraciones correspondientes.

## 13. Authentication

El mecanismo de autenticación oficial utilizará JWT con access tokens y refresh tokens.

### Authentication database tables

El módulo `auth` contempla inicialmente:

```text
auth.users
auth.roles
auth.permissions
auth.user_roles
auth.role_permissions
auth.user_sessions
auth.password_reset_tokens
auth.auth_events
```

La estructura detallada debe respetar el DBML aprobado y la documentación de autenticación.

### Users

La tabla `auth.users` representa una identidad digital.

Debe contener credenciales y datos relacionados con autenticación.

No almacenar directamente:

- Nombres.
- Apellidos.
- DNI.
- Dirección.
- Información clínica.
- Información profesional.
- Datos personales que correspondan al dominio `core`.

La relación entre personas y usuarios se incorporará cuando se diseñe el módulo correspondiente.

No asumir que todos los pacientes necesitan una cuenta de usuario.

### Email

- El email será el identificador principal de autenticación.
- Debe ser obligatorio y único.
- Debe normalizarse antes de persistirlo y consultarlo.
- Aplicar una política consistente de normalización mediante trim y lowercase.
- Garantizar unicidad en la base de datos.
- No reutilizar automáticamente el email de una cuenta eliminada sin una política aprobada.

### Passwords

Utilizar Argon2id para el almacenamiento de contraseñas.

- Nunca almacenar contraseñas en texto plano.
- Nunca utilizar SHA-256 o MD5 directamente como hash de contraseña.
- Utilizar una biblioteca mantenida y compatible con el entorno de ejecución.
- Configurar parámetros de hashing seguros y evaluados para la infraestructura.
- No devolver hashes de contraseñas en respuestas HTTP.
- No registrar contraseñas ni hashes en logs.
- Excluir `password_hash` de las consultas predeterminadas de TypeORM.
- Utilizar DTOs explícitos para respuestas públicas.

### Login protection

Contemplar:

- Validación del estado del usuario.
- Control de intentos fallidos.
- Bloqueo temporal.
- Rate limiting.
- Respuestas genéricas ante credenciales inválidas.
- Registro de eventos de seguridad.
- Revocación de sesiones cuando corresponda.

El rate limiting no debe depender exclusivamente de memoria local en entornos con múltiples instancias.

## 14. JWT architecture

### Access token

- Format: JWT.
- Algorithm: HS256 como estándar inicial.
- Recommended lifetime: 15 minutes.
- Persistence: None.
- Transport: Authorization Bearer header.
- Purpose: Authenticate protected requests.

Claims:

```json
{
  "sub": "user-uuid",
  "token_use": "access",
  "iat": 1730000000,
  "exp": 1730000900
}
```

No crear una tabla de access tokens.

No almacenar información clínica, contraseñas ni datos sensibles en el payload JWT.

### Refresh token

- Format: JWT.
- Algorithm: HS256 como estándar inicial.
- Recommended lifetime: Up to 7 days.
- Persistence: HMAC only.
- Purpose: Token renewal and logout.

Claims:

```json
{
  "sub": "user-uuid",
  "sid": "session-uuid",
  "token_use": "refresh",
  "iat": 1730000000,
  "exp": 1730604800
}
```

El claim `sid` corresponde a `auth.user_sessions.id`.

### JWT secrets

Utilizar secretos independientes:

```dotenv
JWT_ACCESS_SECRET=
JWT_REFRESH_SECRET=
JWT_HMAC_SECRET=

JWT_ACCESS_TTL=15m
JWT_REFRESH_TTL=7d
JWT_SESSION_FAMILY_TTL=30d

JWT_ISSUER=
JWT_AUDIENCE=
```

Las duraciones son valores iniciales propuestos y deben ser configurables.

Los secretos deben:

- Generarse criptográficamente.
- Tener suficiente entropía.
- Ser independientes entre sí.
- Obtenerse desde variables de entorno o gestores de secretos.
- Validarse durante el inicio de la aplicación.

Nunca utilizar secretos hardcodeados.

Validar explícitamente el algoritmo, tipo de token, firma, expiración y claims requeridos.

Cuando se configuren issuer y audience, deben exigirse durante la verificación.

No aceptar refresh tokens como access tokens ni viceversa.

## 15. Session management

### Session persistence

Cada refresh token emitido debe tener una fila independiente en:

```text
auth.user_sessions
```

La sesión debe contener:

| Column | Purpose |
| --- | --- |
| id | Refresh JWT sid |
| user_id | Session owner |
| family_id | Refresh token rotation chain |
| refresh_token_hash | HMAC-SHA256 hexadecimal |
| session_status | Functional session state |
| replaced_by_session_id | Successor session |
| ip_address | Client IP address |
| user_agent | Client metadata |
| expires_at | Refresh JWT expiration |
| family_expires_at | Absolute family expiration |
| last_activity_at | Last recorded activity |
| rotated_at | Rotation timestamp |
| revoked_at | Revocation timestamp |
| compromised_at | Compromise timestamp |
| revocation_reason | Invalidation reason |

Además, debe incluir todos los campos de auditoría establecidos.

### Session states

```typescript
export enum SessionStatus {
  ACTIVE = 'ACTIVE',
  ROTATED = 'ROTATED',
  REVOKED = 'REVOKED',
  EXPIRED = 'EXPIRED',
  COMPROMISED = 'COMPROMISED',
}
```

El estado funcional es independiente de `RecordStatus`.

Una sesión rotada puede conservar `status = ACTIVE` como registro histórico válido.

### Refresh token storage

Calcular:

```text
HMAC-SHA256(refreshToken, JWT_HMAC_SECRET)
```

Persistir únicamente la representación hexadecimal de 64 caracteres.

No almacenar el refresh JWT original.

Utilizar comparaciones de tiempo constante para verificar el HMAC.

### Session families

Cada login genera una nueva familia de sesiones.

Cada renovación:

- Genera un nuevo UUID de sesión.
- Conserva el mismo `family_id`.
- Conserva el mismo `family_expires_at`.
- Crea un nuevo refresh JWT.
- Persiste un nuevo HMAC.
- Marca la sesión anterior como `ROTATED`.
- Establece la referencia hacia la sesión sucesora.

Una familia debe tener como máximo una sesión funcionalmente activa.

Esta restricción debe garantizarse mediante un índice único parcial en PostgreSQL:

```sql
CREATE UNIQUE INDEX uq_user_sessions_active_family
ON auth.user_sessions (family_id)
WHERE session_status = 'ACTIVE';
```

### Session expiration

Debe cumplirse:

```text
expires_at > created_at
expires_at <= family_expires_at
```

La expiración persistida debe coincidir con el claim `exp` del refresh JWT.

Una sesión vencida nunca debe aceptarse, incluso si todavía conserva el estado `ACTIVE`.

La transición a `EXPIRED` podrá ejecutarse mediante un proceso posterior.

### Rotation transaction

La rotación debe ejecutarse dentro de una única transacción.

- Verificar la autenticidad del refresh JWT.
- Bloquear la sesión correspondiente mediante un bloqueo de escritura o mecanismo equivalente.
- Serializar las operaciones que afecten a una misma familia.
- Validar propietario, estado general, estado funcional y expiración.
- Verificar el HMAC.
- Marcar la sesión anterior como `ROTATED`.
- Crear la nueva sesión `ACTIVE`.
- Establecer la referencia de reemplazo.
- Confirmar la transacción.
- Entregar los nuevos tokens únicamente después del commit.

Todas las operaciones deben utilizar el mismo contexto transaccional.

No utilizar repositorios externos a la transacción para realizar escrituras que formen parte de la rotación.

El orden de las escrituras debe respetar el índice único parcial.

La sesión sucesora debe pertenecer al mismo usuario y familia que su predecesora.

### Reuse detection

Si se presenta un refresh token auténtico previamente rotado:

- Rechazar la renovación.
- Registrar el evento de seguridad.
- Invalidar la familia comprometida según la política establecida.
- Revocar cualquier sesión activa perteneciente a esa familia.
- Impedir que una rotación concurrente deje una sesión activa después de la invalidación.

Las transiciones de seguridad deben confirmarse en la base de datos antes de devolver el error correspondiente.

No lanzar una excepción dentro de una transacción si ello provoca que se revierta accidentalmente la revocación de seguridad.

El cliente debe coordinar sus solicitudes de refresh para evitar renovaciones simultáneas innecesarias.

### Logout

El logout debe:

- Verificar el refresh JWT.
- Verificar la correspondencia entre usuario, sesión y HMAC.
- Revocar la sesión correspondiente de manera atómica.
- Impedir futuras renovaciones.
- Registrar el evento de seguridad.

Preferir comportamiento idempotente para solicitudes repetidas válidas.

La política de revocación inmediata de access tokens debe definirse explícitamente.

El access token no se considera revocado automáticamente por el solo hecho de revocar el refresh token.

Para acceso clínico sensible, priorizar mecanismos que permitan invalidación inmediata cuando sea necesario.

### Session security

- No almacenar sesiones únicamente en memoria.
- No depender de una instancia específica de NestJS.
- No registrar JWT completos.
- No devolver hashes de refresh tokens.
- No utilizar datos de IP o user agent como prueba exclusiva de identidad.
- No confiar en encabezados de IP reenviados sin una configuración adecuada de proxies.
- No utilizar `last_activity_at` como indicador exacto de actividad si solo se actualiza durante renovaciones.

## 16. Role-Based Access Control

El sistema utilizará RBAC granular.

### Tables

```text
auth.users
auth.roles
auth.permissions
auth.user_roles
auth.role_permissions
```

### Relationships

- A user can have multiple roles.
- A role can belong to multiple users.
- A role can have multiple permissions.
- A permission can belong to multiple roles.

No incorporar un único `role_id` directamente en `auth.users`.

No utilizar roles hardcodeados como mecanismo principal de autorización.

### Permission naming

```text
module.resource.action
```

Examples:

```text
auth.users.create
auth.users.read
auth.users.update
auth.users.delete
auth.roles.assign
auth.roles.update
auth.permissions.assign
auth.sessions.revoke
auth.events.read
clinical.patients.read
clinical.records.read
clinical.records.create
scheduling.appointments.create
scheduling.appointments.cancel
billing.payments.create
billing.payments.refund
```

### Effective permissions

Un permiso será efectivo únicamente cuando:

- El usuario esté activo.
- La asignación del rol esté activa.
- La asignación del rol no haya expirado.
- El rol esté activo.
- La asignación del permiso esté activa.
- El permiso esté activo.

Los permisos efectivos representan la unión de permisos vigentes de los roles asignados.

### Authorization rules

- Deny by default.
- Least privilege.
- Explicit authorization.
- Server-side validation.
- No confiar en permisos enviados por el cliente.
- No utilizar únicamente la visibilidad de elementos del frontend como mecanismo de seguridad.
- No permitir que un usuario eleve sus propios privilegios sin autorización.
- Validar quién puede asignar roles y permisos.
- Registrar cambios de privilegios como eventos de seguridad.
- No otorgar acceso clínico automático a administradores técnicos.
- Considerar cambios de permisos y desactivación de usuarios durante la autorización de peticiones.

RBAC debe complementarse con reglas de acceso a nivel de registro cuando el dominio lo requiera.

Por ejemplo, poseer el permiso `clinical.records.read` no implica necesariamente acceso a todas las historias clínicas.

## 17. Security events and auditing

Utilizar inicialmente:

```text
auth.auth_events
```

Eventos sugeridos:

```text
LOGIN_SUCCESS
LOGIN_FAILED
LOGOUT
TOKEN_REFRESHED
TOKEN_REUSE_DETECTED
PASSWORD_CHANGED
PASSWORD_RESET_REQUESTED
PASSWORD_RESET_COMPLETED
ACCOUNT_LOCKED
ROLE_ASSIGNED
ROLE_REVOKED
PERMISSION_ASSIGNED
PERMISSION_REVOKED
ACCESS_DENIED
SESSION_REVOKED
```

Los eventos de seguridad deben funcionar como registros append-only.

La aplicación operativa no debe modificarlos ni eliminarlos.

Las políticas autorizadas de retención deberán gestionarse separadamente.

No registrar:

- Passwords.
- Password hashes.
- JWT completos.
- Refresh token hashes.
- Secretos.
- Información clínica innecesaria.
- Credenciales de conexión.

La auditoría de autenticación no reemplaza la futura auditoría de acceso a información clínica.

Los campos `created_by`, `created_at`, `modified_by` y `modified_at` tampoco reemplazan un historial completo de cambios cuando exista una necesidad de trazabilidad.

## 18. API design

Utilizar una API REST consistente.

### Route examples

```text
POST   /auth/login
POST   /auth/refresh
POST   /auth/logout
POST   /auth/forgot-password
POST   /auth/reset-password

GET    /users
GET    /users/:id
POST   /users
PATCH  /users/:id
DELETE /users/:id
```

Los ejemplos son orientativos y deben adaptarse a la estrategia de versionamiento de API aprobada.

### HTTP semantics

| Status | Purpose |
| --- | --- |
| 200 | Successful operation |
| 201 | Resource created |
| 204 | Successful operation without response body |
| 400 | Invalid request |
| 401 | Unauthenticated |
| 403 | Forbidden |
| 404 | Resource not found |
| 409 | Conflict |
| 422 | Semantic validation error when applicable |
| 429 | Too many requests |
| 500 | Unexpected internal error |

No devolver HTTP 200 para representar errores.

### DTOs

- Utilizar DTOs explícitos.
- Validar todas las entradas externas.
- Separar DTOs de creación, actualización y respuesta.
- No utilizar entidades TypeORM directamente como contratos públicos.
- Validar UUIDs, emails, fechas y enumeraciones.
- Establecer límites razonables para strings, arrays y paginación.
- Rechazar propiedades inesperadas.
- Evitar mass assignment.

Utilizar mecanismos de validación de NestJS, incluyendo `ValidationPipe` cuando corresponda.

### Responses

- Mantener contratos consistentes.
- No exponer entidades completas indiscriminadamente.
- No exponer secretos ni campos internos innecesarios.
- Utilizar paginación en listados potencialmente grandes.
- Mantener un ordenamiento determinista.
- Validar filtros y campos de ordenamiento permitidos.

No introducir un response wrapper global sin definir previamente el contrato de API.

### API documentation

Utilizar OpenAPI/Swagger cuando se implemente la documentación HTTP.

Documentar:

- DTOs.
- Authentication requirements.
- Permissions.
- Response types.
- Error responses.
- Pagination.
- Query parameters.

Restringir la exposición pública de documentación técnica en entornos sensibles.

## 19. Error handling

- Utilizar excepciones apropiadas de NestJS en los límites HTTP.
- Centralizar el tratamiento de errores cuando sea necesario.
- No devolver stack traces al cliente.
- No exponer SQL interno.
- No exponer credenciales ni configuración.
- No capturar errores para ignorarlos silenciosamente.
- Mantener información técnica suficiente en logs seguros.
- Traducir errores conocidos de persistencia a respuestas apropiadas.
- Diferenciar errores de validación, negocio, autorización e infraestructura.

No utilizar excepciones genéricas cuando exista un error más específico y útil.

## 20. Transactions and concurrency

Utilizar transacciones cuando una operación involucre múltiples escrituras que deban completarse conjuntamente.

Examples:

- Refresh token rotation.
- User creation with role assignments.
- Role and permission changes.
- Session revocation.
- Appointment scheduling.
- Payment registration.
- Clinical record updates requiring consistent history.

### Transaction rules

- Mantener todas las operaciones relacionadas dentro del mismo contexto transaccional.
- Utilizar el EntityManager transaccional o QueryRunner correspondiente.
- Evitar transacciones innecesariamente largas.
- No realizar llamadas externas lentas dentro de transacciones sin justificación.
- Utilizar bloqueos cuando exista riesgo de concurrencia.
- Mantener un orden consistente de adquisición de bloqueos.
- Considerar deadlocks y estrategias de reintento seguras.
- No asumir que una validación previa evita condiciones de carrera.
- Utilizar restricciones de base de datos como última línea de defensa.

## 21. Configuration and environments

Toda configuración sensible debe gestionarse mediante variables de entorno.

Mantener:

```text
.env.example
```

Nunca versionar:

```text
.env
.env.local
.env.production
.env.staging
```

### Suggested variables

```dotenv
NODE_ENV=development
APP_ENV=development
PORT=3000

DATABASE_URL=
DATABASE_MIGRATION_URL=
DATABASE_SSL=false
DATABASE_POOL_SIZE=5

JWT_ACCESS_SECRET=
JWT_REFRESH_SECRET=
JWT_HMAC_SECRET=

JWT_ACCESS_TTL=15m
JWT_REFRESH_TTL=7d
JWT_SESSION_FAMILY_TTL=30d

JWT_ISSUER=
JWT_AUDIENCE=

APP_TIMEZONE=America/Lima
CORS_ORIGINS=
```

Los valores son referencias iniciales y deben ajustarse al entorno real.

No utilizar valores inseguros como fallback para secretos obligatorios.

Validar las variables de entorno antes de iniciar la aplicación.

### Environments

| Environment | Purpose |
| --- | --- |
| Development | Local development |
| Test | Automated testing |
| Staging | Clinic validation |
| Production | Real clinical operations |

Cada entorno debe utilizar una base de datos independiente.

No utilizar `NODE_ENV` como único mecanismo para identificar el entorno de despliegue.

Staging puede ejecutar una compilación de producción sin ser el entorno productivo real.

No utilizar datos clínicos reales en desarrollo, pruebas automatizadas o staging sin autorización y controles adecuados.

## 22. Vercel deployment

Vercel será inicialmente un entorno temporal de validación.

La aplicación debe considerar las características de un entorno serverless.

### Serverless rules

- No depender de estado persistente en memoria.
- No depender de archivos locales persistentes.
- No depender de timers o procesos permanentes.
- No asumir que una instancia atenderá todas las peticiones.
- Evitar conexiones de base de datos ilimitadas.
- Configurar un pool de conexiones adecuado.
- Reutilizar conexiones e inicialización cuando el runtime lo permita.
- Utilizar mecanismos externos para tareas programadas cuando sean necesarios.
- Verificar compatibilidad de dependencias nativas con el runtime de despliegue.

No introducir Redis, colas de mensajes u otros servicios externos sin una necesidad justificada.

### Deployment migrations

No ejecutar migraciones durante:

- Application bootstrap.
- Cold starts.
- HTTP requests.
- Uncontrolled preview deployments.

Las migraciones de staging y producción deben ejecutarse mediante un proceso controlado y serializado.

El despliegue debe garantizar compatibilidad entre la versión de la aplicación y el esquema de base de datos.

Preferir inicialmente un proceso manual controlado y posteriormente automatizarlo mediante CI/CD.

Las credenciales utilizadas para migraciones deben separarse de las credenciales operativas cuando la infraestructura lo permita.

## 23. Clinical data protection

Clínica FISE manejará información personal y potencialmente sensible relacionada con la salud.

La seguridad y privacidad son requisitos transversales.

### Mandatory principles

- Least privilege.
- Data minimization.
- Confidentiality.
- Integrity.
- Availability.
- Traceability.
- Secure transport.
- Controlled access.
- Secure backups.

### Restrictions

- No utilizar información real de pacientes como datos de prueba.
- No copiar bases de datos clínicas a entornos no autorizados.
- No enviar información clínica a servicios externos sin autorización.
- No incluir información personal sensible en logs.
- No exponer historias clínicas mediante endpoints públicos.
- No utilizar identificadores UUID como sustituto de controles de autorización.
- No otorgar acceso clínico por conveniencia administrativa.
- No implementar eliminación irreversible de historias clínicas sin una política aprobada.
- No incluir secretos ni datos de pacientes en documentación, prompts o commits.

Considerar la Ley N.° 29733, Ley de Protección de Datos Personales del Perú, su reglamentación vigente y las obligaciones aplicables al tratamiento de información de salud.

El agente no debe declarar cumplimiento normativo únicamente porque se hayan implementado controles técnicos.

Los requisitos legales, políticas de retención y accesos clínicos deben validarse con los responsables correspondientes.

## 24. Logging

Utilizar logs estructurados cuando sea apropiado.

Los logs deben permitir identificar:

- Operation.
- Timestamp.
- Severity.
- Request identifier.
- Error category.
- Relevant technical context.

No registrar objetos completos de solicitudes o respuestas sin aplicar filtros.

No registrar:

- Passwords.
- Authorization headers.
- Cookies containing credentials.
- JWTs.
- Secrets.
- Database connection strings.
- Clinical records.
- Sensitive personal information.

No utilizar `console.log` indiscriminadamente como mecanismo principal de logging.

Utilizar el sistema de logging de NestJS o la solución aprobada por el proyecto.

## 25. Testing

Las funcionalidades nuevas deben incluir pruebas proporcionales a su riesgo y complejidad.

### Test types

- Unit tests.
- Integration tests.
- End-to-end tests.
- Database migration tests.
- Authorization tests.
- Concurrency tests.

Utilizar PostgreSQL para pruebas de integración que dependan de características específicas del motor.

No sustituir PostgreSQL por SQLite cuando ello impida verificar restricciones, tipos, schemas, transacciones o bloqueos reales.

### Authentication acceptance criteria

Verificar como mínimo:

- Login válido.
- Login inválido.
- Usuario inactivo.
- Usuario eliminado.
- Access token válido.
- Access token expirado.
- Refresh token utilizado como Bearer.
- Refresh token inválido.
- Refresh token expirado.
- Refresh token con claims inconsistentes.
- Rotación exitosa.
- Reutilización de refresh token.
- Dos solicitudes concurrentes de refresh.
- Revocación de familia.
- Logout.
- Comportamiento del access token después de logout.
- Persistencia exclusiva del HMAC.
- Ausencia de secretos en logs y respuestas.

### RBAC acceptance criteria

Verificar:

- Usuario sin roles.
- Usuario con múltiples roles.
- Rol inactivo.
- Permiso inactivo.
- Asignación expirada.
- Permiso concedido.
- Permiso denegado.
- Usuario desactivado.
- Revocación de permisos.
- Intentos de escalamiento de privilegios.
- Acceso a registros no autorizados.

### Before completing a task

Ejecutar las verificaciones disponibles y pertinentes:

```bash
pnpm run lint
pnpm run build
pnpm run test
pnpm run test:e2e
```

Adaptar los comandos a los scripts reales del repositorio.

No afirmar que las pruebas fueron ejecutadas si no se ejecutaron realmente.

Si una verificación no puede ejecutarse, explicar el motivo y el riesgo pendiente.

## 26. Seeds and initial data

Mantener los seeds en:

```text
src/database/seeds/
```

Utilizarlos para:

- Initial roles.
- Initial permissions.
- System catalogs.
- Development fixtures.
- Authorized bootstrap data.

### Seed rules

- Preferir seeds idempotentes.
- Utilizar identificadores naturales estables, como `code`, para evitar duplicados.
- No sobrescribir configuraciones personalizadas sin autorización.
- No incorporar contraseñas predeterminadas inseguras.
- No incluir credenciales reales.
- Separar seeds de sistema de datos ficticios.
- No ejecutar automáticamente datos de desarrollo en staging o producción.

Los cambios de datos indispensables para una nueva versión deben gestionarse mediante un mecanismo versionado y controlado.

La creación del primer administrador debe realizarse mediante un proceso seguro y explícito.

## 27. Documentation

Mantener documentación técnica junto con el código.

### Proposed documentation structure

```text
AGENTS.md
README.md

database/
└── clinica_fise.dbml

docs/
├── ARCHITECTURE.md
├── DATABASE_CONVENTIONS.md
├── AUTHENTICATION.md
├── RBAC.md
├── MIGRATIONS.md
├── DEPLOYMENT.md
├── SECURITY.md
└── adr/
    ├── 001-use-typeorm.md
    ├── 002-use-uuid.md
    ├── 003-refresh-token-rotation.md
    └── 004-versioned-migrations.md
```

No crear documentación vacía únicamente para completar la estructura propuesta.

### Architecture Decision Records

Utilizar ADRs para decisiones arquitectónicas relevantes.

Cada ADR debe incluir:

- Title.
- Status.
- Date.
- Context.
- Decision.
- Alternatives.
- Consequences.

No modificar silenciosamente decisiones previamente aceptadas.

Cuando una decisión sea reemplazada, registrar una nueva decisión y documentar la relación con la anterior.

### Documentation updates

Actualizar documentación cuando cambien:

- Database structure.
- API contracts.
- Authentication flows.
- Authorization rules.
- Environment variables.
- Deployment procedures.
- Architecture decisions.
- Security policies.

## 28. Git conventions

Mantener cambios enfocados y revisables.

Utilizar Conventional Commits cuando corresponda.

Examples:

```text
feat(auth): implement user authentication
feat(auth): add refresh token rotation
feat(rbac): implement permission guard
fix(auth): prevent concurrent session rotation
refactor(database): centralize audit fields
docs(database): update authentication DBML
chore(config): configure TypeORM data source
test(auth): add session concurrency tests
```

### Git rules

- No sobrescribir cambios no relacionados del desarrollador.
- No utilizar comandos destructivos de Git sin autorización.
- No ejecutar force push.
- No incluir secretos.
- No incluir archivos generados innecesarios.
- No crear commits o realizar push sin autorización cuando no formen parte de la tarea.
- No modificar el historial compartido.
- No revertir cambios ajenos para resolver conflictos automáticamente.

## 29. Project roadmap

El proyecto evolucionará por fases.

| Phase | Module |
| --- | --- |
| 0 | Project foundations |
| 1 | Authentication and RBAC |
| 2 | Core and persons |
| 3 | Organization and professionals |
| 4 | Patients |
| 5 | Scheduling and appointments |
| 6 | Clinical records |
| 7 | Physiotherapy and treatments |
| 8 | Billing and payments |
| 9 | Inventory |
| 10 | Advanced auditing and reporting |

Este roadmap es orientativo.

No implementar módulos futuros de manera anticipada sin una solicitud explícita.

Diseñar las funcionalidades actuales de manera que permitan extensiones futuras sin introducir complejidad innecesaria.

## 30. Definition of Done

Una tarea se considera completada cuando, según corresponda:

- Los requisitos solicitados están implementados.
- Se respetan las convenciones del proyecto.
- No se introducen cambios ajenos al alcance.
- El código compila correctamente.
- El tipado es consistente.
- Las entradas externas están validadas.
- Los errores están gestionados.
- Los permisos están correctamente aplicados.
- Las reglas de negocio están protegidas.
- Las operaciones críticas son transaccionales.
- Las migraciones están versionadas y revisadas.
- La documentación DBML está actualizada.
- Las pruebas pertinentes fueron ejecutadas.
- No se introdujeron secretos ni datos sensibles.
- No existen errores conocidos ocultos.
- Los riesgos pendientes están documentados.

No es obligatorio modificar todos los componentes en cada tarea. Aplicar únicamente los criterios relevantes.

## 31. Agent final report

Al finalizar una tarea, el agente debe presentar un resumen que incluya:

### Summary

Descripción breve de la funcionalidad implementada.

### Changes

Archivos y componentes relevantes modificados.

### Database changes

- Entities modified.
- Tables created or modified.
- Migration files.
- Migration execution status.
- Constraints and indexes.
- DBML updates.

Indicar explícitamente cuando no existan cambios en la base de datos.

### Validation

- Build result.
- Lint result.
- Test result.
- Migration verification result.

Diferenciar verificaciones exitosas, fallidas y no ejecutadas.

### Security considerations

Cambios o implicaciones relevantes de seguridad.

### Decisions and assumptions

Decisiones técnicas tomadas durante la implementación y supuestos utilizados.

### Pending work

Limitaciones, bloqueos, riesgos y tareas pendientes.

### Next steps

Sugerencias concretas para continuar el desarrollo.

No ocultar errores, decisiones no autorizadas o verificaciones pendientes.

## 32. Current project priority

La prioridad inicial es establecer los fundamentos técnicos del proyecto antes de implementar funcionalidades clínicas.

Orden recomendado:

1. Inspect the existing repository.
2. Validate NestJS configuration.
3. Configure PostgreSQL and TypeORM.
4. Configure environment validation.
5. Configure the migration DataSource.
6. Configure migration scripts.
7. Establish shared audit fields.
8. Establish database conventions.
9. Create the initial DBML model.
10. Implement the authentication and RBAC module.
11. Implement JWT session management.
12. Implement security auditing.
13. Validate migrations and tests.
14. Prepare the staging deployment.

No comenzar a implementar módulos clínicos hasta que los fundamentos de autenticación, autorización y persistencia estén correctamente establecidos.

## 33. Final principle

Clínica FISE debe evolucionar de manera incremental, segura y mantenible.

La velocidad de implementación no debe comprometer:

- Database integrity.
- Patient privacy.
- Authentication security.
- Authorization correctness.
- Code maintainability.
- Data preservation.
- Migration traceability.

Preferir soluciones explícitas, verificables y compatibles con las decisiones arquitectónicas del proyecto.

Cuando exista incertidumbre relevante, documentarla y solicitar aclaración antes de introducir cambios irreversibles.

---

Project: Clínica FISE

Document: AGENTS.md

Version: 1.0.0

Status: Initial architecture baseline

