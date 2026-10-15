<!--
SYNC IMPACT REPORT
- Cambio de versión: 1.0.0 → 1.1.0 (MINOR: nueva regla de dominio)
- Principios modificados: "3. Reglas de Dominio › Bloques de Tiempo" — se fija la duración
  de cada reserva en 60 minutos exactos.
- Secciones añadidas: ninguna.
- Secciones eliminadas: ninguna.
- Plantillas dependientes: no modificadas (leen la constitución en tiempo de ejecución).
- TODOs diferidos:
  - Horario operativo y si los bloques inician solo en hora en punto: deben fijarse en spec.md.
-->

# Constitución del Proyecto: Sistema de Reservas de Pádel

## 1. Naturaleza del Proyecto

Aplicación para la reserva de canchas de pádel. Su propósito es permitir a los usuarios
autenticarse y gestionar reservas de tiempo en espacios específicos.

## 2. Stack Tecnológico (Reglas de Implementación)

* **Frontend / UI:** Angular. Los estilos MUST escribirse en SCSS.

* **Backend:** Node.js con Express.

* **Base de Datos:** SQLite local (archivo `padel.db`). MUST NOT usarse ORMs pesados; se usa
  `better-sqlite3` o sentencias SQL puras.

* **Lenguaje:** TypeScript en todo el stack (frontend y backend).

## 3. Reglas de Dominio y Lógica de Negocio

Estas reglas son inmutables. El agente de IA MUST respetarlas estrictamente:

* **Catálogo Cerrado:** El sistema SOLO maneja 5 canchas fijas: Cancha Laureles,
  Cancha El Poblado, Cancha Belén, Cancha Robledo y Cancha Envigado. MUST NOT existir
  funcionalidad para crear, editar o eliminar canchas.

* **Bloques de Tiempo:** Las reservas operan en formato de 24 horas (HH:mm, 00:00–23:59).
  Cada reserva dura exactamente 60 minutos; MUST NOT aceptarse otras duraciones.
  El horario operativo se define en `spec.md`.

* **Prevención de Colisiones (Double-Booking) — NO NEGOCIABLE:** Bajo ninguna circunstancia
  se escribe una reserva en la base de datos sin validar primero, en el backend, que la cancha
  seleccionada esté libre en ese horario. Ante colisión, la escritura MUST rechazarse con 409.

* **Autenticación:** Todo flujo de reserva MUST exigir un usuario con sesión activa; las
  peticiones sin sesión válida se rechazan con 401.

## 4. Estructura y Estilo de Código

* **Estructura Plana:** Evitar la sobreingeniería. MUST NOT implementarse "Clean Architecture"
  ni patrones complejos. Estructura raíz: `/frontend`, `/backend` y `/db`.

* **Estilo Angular:** Usar standalone components y signals como opción por defecto. Organizar
  el frontend con una distribución de carpetas estilo `core` (p. ej. `core/` para servicios,
  guards e interceptores singleton; `shared/` para componentes reutilizables; `features/` o
  `pages/` para las vistas).

* **Nomenclatura:** camelCase para funciones y variables; PascalCase para Interfaces y Tipos.

## 5. Manejo de Errores y Validaciones

* **UI:** MUST NOT exponerse errores crudos ni stack traces al usuario final. Todo error
  técnico se traduce a un mensaje amigable (ej.: "La cancha ya fue reservada en este horario").

* **Backend:** Retornar siempre códigos de estado HTTP semánticos: 400 petición inválida,
  401 no autenticado, 409 conflicto de reserva.

## 6. Comportamiento del Agente de IA (Reglas SDD)

* **Cero Código Sombra (Shadow Code):** Construir estrictamente lo documentado en `spec.md`.
  MUST NOT añadirse características "por si acaso" (no pasarelas de pago, no perfiles
  complejos, etc.).

* **Fuente de la Verdad:** Si una instrucción del usuario contradice esta constitución o se
  detecta una falla lógica, el agente MUST detenerse, advertir el problema y solicitar
  actualizar `spec.md` antes de tocar el código fuente.

## 7. Gobernanza

* Esta constitución prevalece sobre cualquier otra práctica, plan o instrucción puntual.
* **Enmiendas:** Toda modificación se realiza mediante `/speckit-constitution`, documentando
  el cambio en el Sync Impact Report y actualizando la versión.
* **Versionado (SemVer):** MAJOR para eliminación o redefinición incompatible de reglas;
  MINOR para reglas o secciones nuevas; PATCH para aclaraciones y redacción.
* **Cumplimiento:** Cada `plan.md` MUST incluir una verificación contra esta constitución
  (Constitution Check), y cada revisión de código MUST validar las reglas de las secciones 3 y 5.

**Version**: 1.1.0 | **Ratified**: 2026-10-14 | **Last Amended**: 2026-10-14
