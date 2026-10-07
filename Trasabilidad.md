# Matriz de trazabilidad — v0.2

> Base: 6 entrevistas de descubrimiento, inventario de funciones y pseudocódigo escrito hasta hoy.
---

## 0. Convenciones

**Capas:** `[Dom]` dominio · `[App]` aplicación · `[Dat]` acceso a datos · `[Inf]` infraestructura · `[UI]` presentación

**Pseudocódigo:** ✅ escrito · ⏳ pendiente

**Prioridad:**
- **P0** imprescindible: sin esto la app no resuelve lo que dijeron las entrevistas
- **P1** importante: la app funciona sin ello, pero queda coja
- **P2** incluida por decisión del usuario, con evidencia débil en las entrevistas (hábitos)
- **P3** después: va al backlog o se hace antes de publicar en tiendas

---

## 1. Evidencia de las entrevistas

| Patrón | Quiénes | De 6 |
|---|---|---|
| Registrar es fricción o se les olvida | Margarita, Sebastian, Valentina, Isabella, Mariana | 5 |
| Pendientes y tareas | Margarita, Sebastian, Laura, Valentina | 4 |
| Todo en un solo lugar | Margarita, Sebastian, Laura (Valentina lo implica) | 3 a 4 |
| Recordatorios o notificaciones | Margarita, Sebastian, Laura | 3 |
| Gastos como dolor explícito | Isabella, Mariana | 2 |
| Hábitos | Aparecen en la redacción de la HU de Margarita y Sebastian, no en su frase ni su dolor. Isabella y Mariana los llevan mentalmente, sin herramienta y sin dolor reportado | 0 como dolor |
| Exportar CSV | Nadie | 0 |

**Lecturas:**
- El umbral que fijamos fue 6 de 10 (60%). Sobre 6 respuestas equivale a 4 de 6. Pasan: registrar sin fricción (5) y tareas (4). **Recordatorios (3, es decir 50%) queda bajo el umbral**, pero se marca P0 por la gravedad de la queja de Laura (las notificaciones fallaban). Confirmar en la próxima ronda de entrevistas.
- Margarita pierde el control cuando se cae la app de su banco. Refuerza la decisión de captura manual (ADR-004) y apunta a un candidato de backlog: guardar el gasto aunque no haya internet.
- Muestra pequeña y probablemente homogénea (estudiantes universitarios). Sirve para decidir el MVP, no para concluir que el mercado es amplio.

### Correcciones pendientes a la tabla de entrevistas

- Isabella en hábitos: resuelto. Ella (y Mariana) llevan sus hábitos mentalmente, sin registro, así que no cuenta como mención ni como demanda.
- Mariana en tareas: pendiente de verificar; su fila habla solo de gastos.
- Valentina: el criterio "menos de 5 minutos" está confirmado en minutos. Se mantiene como criterio de HU-02 junto al de 5 s por tarea, que es más estricto y lo cubre.
- Numeración: en el documento, hábitos = HU-07 y HU-08; exportar = HU-09 (en la matriz de contraste aparecía HU-06 repetida).

---

## 2. Historias de usuario finales propuestas

| HU | Nombre | Prioridad | Evidencia | Criterio de aceptación medible |
|---|---|---|---|---|
| HU-01 | Registro e inicio de sesión | P0 | Habilitadora (aísla los datos) | Registro con email y contraseña de 8+ caracteres; la sesión persiste al cerrar y abrir; un usuario B ve 0 filas del usuario A |
| HU-02 | Crear tarea | P0 | Margarita, Sebastian, Laura, Valentina | Crear una tarea en menos de 5 s (10 intentos con cronómetro); 10 tareas en menos de 5 min (criterio de Valentina, confirmado en minutos); fecha por defecto = hoy |
| HU-03 | Ver tareas de hoy | P0 | Las mismas 4 | La vista lista las de hoy y las vencidas sin completar; excluye las completadas |
| HU-04 | Completar tarea | P0 | Las mismas 4 | Se completa con 1 toque; guarda la hora de cierre; completar dos veces no falla |
| HU-05 | Registrar gasto | P0 | Isabella, Mariana (más Margarita y Sebastian en su HU) | Gasto guardado en menos de 10 s desde que se abre la app; solo monto y fecha obligatorios; 15.000 pesos se guardan como 1.500.000 centavos; si falla el guardado, el gasto sigue en pantalla para reintentar |
| HU-06 | Resumen mensual | P1 | Mariana | Total del mes y total por categoría correctos; un mes vacío muestra cero; un gasto de otro mes no cuenta |
| HU-07 | Marcar hábito (incluye crear hábito) | P2 | Margarita, Sebastian (solo en la redacción de la HU); Isabella y Mariana los llevan mentalmente, sin dolor | Marcar en 2 toques; marcar dos veces el mismo día no duplica ni falla |
| HU-08 | Racha (la vista semanal pasa al backlog) | P2 | Ninguna directa | La racha coincide con los 5 casos del pseudocódigo |
| HU-09 | Exportar datos a CSV | P3 | Nadie | El CSV incluye tareas y gastos y escapa comas y comillas |
| HU-10 | Desmarcar hábito | P2 | Ninguna directa (corrección de error) | Se borra solo el registro de hoy; la racha se recalcula |
| HU-11 | Editar y eliminar gasto | P1 | Ninguna directa (corrección de error) | Tras editar o borrar, el total del mes se actualiza |
| HU-12 | Gestionar categorías | P1 | Ninguna directa | Crear, renombrar y eliminar; al eliminar una categoría, sus gastos quedan sin categoría |
| HU-13 | Ajustes de perfil (zona horaria) | P1 | Ninguna directa (técnica) | Al cambiar la zona horaria, "hoy" se recalcula |
| HU-14 | Eliminar cuenta | P3 | Requisito de tiendas | Borra todos los datos del usuario y cierra la sesión |
| **HU-15** | Configurar recordatorios | **P0** | Margarita, Sebastian, Laura | Activar, elegir hora y tipo en 3 pasos o menos; la configuración sobrevive al reinicio de la app |
| **HU-16** | Recibir recordatorios de forma confiable | **P0** | Laura (fiabilidad) y las otras dos | Llega a la hora configurada con la app cerrada 7 de 7 días seguidos en 1 Android y 1 iPhone; también tras reiniciar el teléfono; si el permiso está denegado, la app lo avisa y lleva a los ajustes |
| **HU-17** | Atajo para registrar un gasto | **P0** | Isabella (más Mariana por velocidad) | Del ícono de la app al gasto guardado en menos de 10 s con la app cerrada (10 intentos) |
| **HU-18** | Ver mi día en un solo lugar | **P0** | Margarita, Sebastian, Laura (Valentina lo implica) | Una sola pantalla muestra las tareas de hoy, el total gastado hoy y los hábitos de hoy (si existen) sin navegar; una sección vacía no rompe la pantalla |
| **HU-19** | Corregir una tarea (editar, eliminar, reabrir) | P1 | Ninguna directa (corrección de error) | Los tres cambios se reflejan de inmediato en la vista Hoy |

**Nota:** las HU sin evidencia directa son "de soporte": se justifican por necesidad técnica o porque el usuario debe poder corregir errores. Si hay que recortar, se recortan primero.

---

## 3. Matriz de trazabilidad

### 3.1 Cuenta

| HU | Pantalla | Funciones | Tablas | Pruebas clave |
|---|---|---|---|---|
| **HU-01** | Login / Registro | `[Dom]` validarCredenciales ✅ · `[App]` registrarUsuario ✅, iniciarSesion ⏳, cerrarSesion ⏳ · `[Inf]` crearCuentaAuth ✅, obtenerSesionActual ⏳ · `[Dat]` trigger al_crear_usuario ✅ · `[UI]` pantalla Registro ✅, pantalla Login ⏳ | auth.users, profiles | 5 casos de validación; 5 de registro (incluye que con datos inválidos no se llama al servicio); integración: se crea el perfil; RLS con dos cuentas; manual: la sesión persiste |
| **HU-13** | Ajustes | `[App]` actualizarPerfil ⏳ · `[Dat]` obtenerPerfil ⏳ · `[UI]` pantalla Ajustes ⏳ | profiles | Cambio de zona recalcula "hoy" |
| **HU-14** | Ajustes | `[App]` eliminarCuenta ⏳ · `[Inf]` función de borrado en servidor (Edge Function) ⏳ · `[UI]` confirmación doble ⏳ | auth.users, todas las tablas (cascade) | Tras borrar, no queda ninguna fila del usuario |

*Nota HU-14:* borrar un usuario de autenticación requiere privilegios de administrador; **no puede hacerse desde la app con la clave `anon`** y la `service_role` nunca va dentro de la app. Se resuelve con una función en el servidor de Supabase.

### 3.2 Tareas

| HU | Pantalla | Funciones | Tablas | Pruebas clave |
|---|---|---|---|---|
| **HU-02** | Hoy | `[Dom]` hoyEnZona ⏳ · `[App]` crearTarea ⏳ · `[Dat]` insertarTarea ⏳ · `[UI]` formulario de tarea ⏳ | tasks, profiles (zona) | Título vacío falla; fecha por defecto = hoy; cronómetro en 10 intentos |
| **HU-03** | Hoy | `[Dom]` seleccionarTareasDeHoy ⏳, estaVencida ⏳, ordenarTareas ⏳ · `[App]` listarTareasDeHoy ⏳ · `[Dat]` consultarTareasHasta ⏳ · `[UI]` lista Hoy con estados cargando, vacío y error ⏳ | tasks | Incluye vencidas; excluye completadas; orden; cambio de día y de zona |
| **HU-04** | Hoy | `[App]` completarTarea ⏳ · `[Dat]` actualizarTarea ⏳ · `[UI]` casilla de tarea ⏳ | tasks | Guarda hora de cierre; doble completado no falla |
| **HU-19** | Hoy | `[App]` editarTarea ⏳, eliminarTarea ⏳, reabrirTarea ⏳ · `[Dat]` borrarTarea ⏳ · `[UI]` menú de tarea ⏳ | tasks | Cada cambio se refleja al instante |

### 3.3 Gastos

| HU | Pantalla | Funciones | Tablas | Pruebas clave |
|---|---|---|---|---|
| **HU-05** | Gastos (formulario rápido) | `[Dom]` pesosACentavos ⏳, centavosAPesos ⏳, formatearCOP ⏳ · `[App]` registrarGasto ✅, sembrarCategoriasPredefinidas ⏳ · `[Dat]` insertarGasto ⏳, consultarCategorias ⏳ · `[UI]` formulario rápido ⏳ | expenses, categories | 15.000 → 1.500.000 centavos; monto 0 falla; nota de 201 caracteres falla; categoría nula permitida; cronómetro < 10 s; si falla, el gasto no se pierde |
| **HU-06** | Gastos (resumen) | `[Dom]` sumarPorCategoria ⏳, totalDelMes ⏳, inicioYFinDeMes ⏳ · `[App]` obtenerResumenMensual ⏳, listarGastosDelMes ⏳ · `[Dat]` consultarGastosPorRango ⏳ · `[UI]` resumen del mes ⏳ | expenses, categories | Suma correcta por categoría; mes vacío = 0; gasto de otro mes no cuenta |
| **HU-11** | Gastos | `[App]` editarGasto ⏳, eliminarGasto ⏳ · `[Dat]` actualizarGasto ⏳, borrarGasto ⏳ · `[UI]` detalle de gasto ⏳ | expenses | El total del mes se actualiza |
| **HU-12** | Ajustes / Gastos | `[App]` crearCategoria ⏳, renombrarCategoria ⏳, eliminarCategoria ⏳ · `[Dat]` insertarCategoria ⏳, actualizarCategoria* ⏳, borrarCategoria* ⏳ · `[UI]` pantalla de categorías ⏳ | categories, expenses | Eliminar deja los gastos sin categoría |

\* Faltaban en el inventario de funciones original.

### 3.4 Recordatorios, captura rápida y pantalla Hoy (nuevo, a raíz de las entrevistas)

| HU | Pantalla | Funciones | Tablas | Pruebas clave |
|---|---|---|---|---|
| **HU-15** | Ajustes → Recordatorios | `[Dom]` validarHoraRecordatorio ⏳, textoRecordatorio ⏳ · `[App]` crearRecordatorio ⏳, cambiarEstadoRecordatorio ⏳, eliminarRecordatorio ⏳, listarRecordatorios ⏳, sincronizarRecordatorios ⏳ · `[Dat]` insertarRecordatorio ⏳, actualizarRecordatorio ⏳, borrarRecordatorio ⏳, consultarRecordatorios ⏳ · `[Inf]` solicitarPermisoNotificaciones ⏳, consultarEstadoPermiso ⏳, programarNotificacionDiaria ⏳, cancelarNotificacion ⏳, configurarCanalAndroid ⏳ · `[UI]` pantalla Recordatorios ⏳ | reminders (nueva) | Hora inválida falla; al cambiar la hora se cancela la anterior y se programa la nueva; sin duplicados; sobrevive al reinicio de la app |
| **HU-16** | Ajustes (estado del permiso) | `[App]` verificarRecordatoriosProgramados ⏳ · `[Inf]` manejarNotificacionAbierta ⏳ (abre la pantalla correcta) · `[UI]` aviso de permiso denegado con botón a ajustes del sistema ⏳ · (reutiliza sincronizarRecordatorios de HU-15) | reminders | 7 de 7 días con la app cerrada en 1 Android y 1 iPhone; tras reiniciar el teléfono; con ahorro de batería activado; con permiso denegado |
| **HU-17** | Ícono de la app / Gastos | `[Inf]` configurarAccionesRapidas ⏳ (verificar librería o plugin compatible con tu versión de Expo), manejarEnlaceProfundo ⏳ · `[UI]` pantalla de gasto con el monto enfocado y teclado numérico ⏳ | expenses | De ícono a gasto guardado < 10 s con la app cerrada (10 intentos) |
| **HU-18** | Hoy (unificada) | `[Dom]` totalDeHoy ⏳ · `[App]` obtenerResumenDeHoy ⏳ · `[Dat]` reutiliza las consultas de HU-03, HU-06 y HU-08 · `[UI]` pantalla Hoy con 3 secciones ⏳ | tasks, expenses, habit_logs | Muestra las 3 secciones sin navegar; sección vacía no rompe; falla de una consulta no oculta las demás |

### 3.5 Hábitos (P2: incluidos por decisión tuya, versión mínima)

| HU | Pantalla | Funciones | Tablas | Pruebas clave |
|---|---|---|---|---|
| **HU-07** | Hábitos | `[App]` crearHabito ⏳, marcarHabitoHecho ⏳ · `[Dat]` insertarHabito ⏳, insertarRegistroHabito ✅ · `[UI]` pantalla Hábitos ✅ | habits, habit_logs | Doble marcado el mismo día no falla ni duplica (idempotente) |
| **HU-08** | Hábitos | `[Dom]` calcularRacha ✅, diaAnterior ⏳ · `[App]` listarHabitosConRacha ⏳ · `[Dat]` consultarRegistros ⏳ | habit_logs | Los 5 casos de `calcularRacha`: vacío → 0; solo hoy → 1; ayer y anteayer sin hoy → 2; hueco → corta; cambio de mes |
| **HU-10** | Hábitos | `[App]` desmarcarHabito ⏳ · `[Dat]` borrarRegistroHabito ⏳ · `[UI]` botón de deshacer ⏳ | habit_logs | Borra solo el registro de hoy |

### 3.6 Datos (P3)

| HU | Pantalla | Funciones | Tablas | Pruebas clave |
|---|---|---|---|---|
| **HU-09** | Ajustes | `[Dom]` convertirACSV ⏳ · `[App]` exportarDatos ⏳ · `[Inf]` compartirArchivo ⏳ | tasks, expenses | Escapa comas y comillas; sin datos genera archivo válido |

### 3.7 Transversales (sin HU propia)

`resultado` (ok / fallar) ⏳ · `traducirErrorDeBase` ⏳ · `registrarError` ⏳ · `leerConfiguracion` ⏳ · `crearClienteSupabase` ⏳

---

## 4. Cobertura del pseudocódigo

**Total: 9 de 106 piezas escritas (funciones y pantallas).** Es una cuenta mayor que la del inventario original (~55) porque ahora incluye las pantallas y las piezas de recordatorios.

| HU | Escritas | Total | HU | Escritas | Total |
|---|---|---|---|---|---|
| HU-01 | 5 | 9 | HU-11 | 0 | 5 |
| HU-02 | 0 | 4 | HU-12 | 0 | 7 |
| HU-03 | 0 | 6 | HU-13 | 0 | 3 |
| HU-04 | 0 | 3 | HU-14 | 0 | 3 |
| HU-05 | 1 | 8 | HU-15 | 0 | 17 |
| HU-06 | 0 | 7 | HU-16 | 0 | 3 |
| HU-07 | 2 | 5 | HU-17 | 0 | 3 |
| HU-08 | 1 | 4 | HU-18 | 0 | 3 |
| HU-09 | 0 | 3 | HU-19 | 0 | 5 |
| HU-10 | 0 | 3 | Transversales | 0 | 5 |

**No hace falta escribir las 97 pendientes ahora.** Escribe el pseudocódigo del módulo justo antes de implementarlo. Orden sugerido:

1. Transversales y `shared` (resultado, dinero, fechas)
2. Resto de HU-01 (iniciarSesion, obtenerSesionActual, cerrarSesion, pantalla Login)
3. Tareas: HU-02, HU-03, HU-04, HU-19
4. Gastos: HU-05, HU-06, HU-11, HU-12
5. Recordatorios: HU-15 y HU-16, **precedidos por un experimento técnico** (ver sección 7)
6. HU-17 (atajo)
7. Hábitos: HU-07, HU-08 y HU-10 (versión mínima)
8. HU-18 (pantalla Hoy), al final para que ya incluya los hábitos

---

## 5. Cambios al modelo de datos

1. En todas las tablas con `user_id`, que lo asigne la base:
```sql
user_id uuid not null default auth.uid() references profiles(id) on delete cascade
```

2. Nueva tabla para HU-15 y HU-16:
```sql
create table reminders (
  id uuid primary key default gen_random_uuid(),
  user_id uuid not null default auth.uid() references profiles(id) on delete cascade,
  kind text not null check (kind in ('gastos', 'tareas', 'habitos', 'todo')),
  time_of_day time not null,
  enabled boolean not null default true,
  created_at timestamptz not null default now(),
  unique (user_id, kind, time_of_day)
);

alter table reminders enable row level security;

create policy "solo mis filas" on reminders
  for all using (user_id = auth.uid()) with check (user_id = auth.uid());
```

3. **Hábitos (incluidos por decisión tuya):**
```sql
create table habits (
  id uuid primary key default gen_random_uuid(),
  user_id uuid not null default auth.uid() references profiles(id) on delete cascade,
  name text not null,
  archived_at timestamptz,
  created_at timestamptz not null default now()
);

create table habit_logs (
  id uuid primary key default gen_random_uuid(),
  habit_id uuid not null references habits(id) on delete cascade,
  user_id uuid not null default auth.uid() references profiles(id) on delete cascade,
  log_date date not null,
  unique (habit_id, log_date)
);
create index on habit_logs (user_id, log_date);

alter table habits enable row level security;
alter table habit_logs enable row level security;

create policy "solo mis filas" on habits
  for all using (user_id = auth.uid()) with check (user_id = auth.uid());

-- habit_logs: además verifica que el hábito referenciado también sea del usuario
create policy "solo mis filas" on habit_logs
  for all using (user_id = auth.uid())
  with check (
    user_id = auth.uid()
    and exists (select 1 from habits h where h.id = habit_id and h.user_id = auth.uid())
  );
```

**Nota de seguridad:** una llave foránea no pasa por RLS, así que sin la verificación `exists` un usuario podría insertar registros en un hábito ajeno si conociera su id. Lo mismo aplica a `expenses.category_id`: la política de `expenses` debe comprobar que la categoría también sea del usuario. Añade ambos casos a las pruebas de RLS.

La hora se guarda como hora local del usuario y la notificación se programa en el propio celular, así que usa la zona horaria del dispositivo.

---

## 6. Revisiones de consistencia

| Revisión | Resultado |
|---|---|
| HU sin funciones, tablas o pruebas | Ninguna con celdas vacías |
| Funciones sin HU | Solo las transversales (sección 3.7), que son infraestructura compartida |
| Tablas sin uso | Ninguna: todas las tablas las usa al menos una HU |
| HU sin evidencia directa en entrevistas | HU-08, 10, 11, 12, 13, 19 (de soporte) y HU-01, 14 (habilitadoras) |
| Criterios no medibles en la tabla original | Los 6 de "Recordatorios de todo en un solo lugar" y "Atajo para abrir la aplicación": reescritos en la sección 2 |

---

## 7. Presupuesto de horas y calendario

| Bloque | HU | Horas | Acumulado | Semana aprox. de cierre |
|---|---|---|---|---|
| Base técnica y cuenta | HU-01 | 12 | 12 | 1,2 |
| Tareas | HU-02, 03, 04, 19 | 14 | 26 | 2,6 |
| Gastos | HU-05, 06, 11, 12 | 18 | 44 | 4,4 |
| Recordatorios (incluye el experimento técnico) | HU-15, 16 | 12 | 56 | 5,6 |
| Atajo de captura | HU-17 | 4 | 60 | 6,0 |
| Hábitos, versión mínima (crear, marcar, racha, desmarcar) | HU-07, 08, 10 | 8 | 68 | 6,8 |
| Pantalla Hoy unificada | HU-18 | 4 | 72 | 7,2 |
| Cierre: ajustes, respaldo técnico y bugs | HU-13 | 6 | 78 | 7,8 |
| Colchón | | 10 | **88** | 8,8 |

**Con hábitos el MVP son 88 h, unas 8,8 semanas a 10 h por semana** (sin ellos serían 80 h). Las 8 h cubren crear, marcar, racha y desmarcar; la vista semanal queda en el backlog. Si el tiempo aprieta, se recorta primero el colchón y el alcance de P1, no la calidad.

**Fuera de presupuesto (P3):** HU-09 y HU-14, más la política de privacidad antes de la beta en tiendas.

**Experimento técnico de notificaciones (2 h, en la semana 1):**
1. Programar una notificación local diaria en un Android y en un iPhone reales.
2. Cerrar la app y esperar a la hora configurada.
3. Reiniciar el teléfono y repetir.
4. Activar el ahorro de batería y repetir.

Si falla en Android tras reiniciar o con ahorro de batería, hay que replantear HU-16 antes de invertir las 12 horas. Para la prueba definitiva en iPhone, Expo Go sirve para empezar pero no representa una app publicada; la prueba final requiere un build, y para un iPhone físico una cuenta de Apple de pago.

---

## 8. Decisiones registradas y pendientes

| Decisión | Estado |
|---|---|
| ADR-009: recordatorios con notificaciones **locales** programadas en el celular, sin servidor | Propuesta. Ventaja: sin servidor, funciona sin internet y se prueba rápido. Costo: no sincroniza entre dispositivos y depende del comportamiento de cada fabricante en Android |
| ADR-010: hábitos incluidos en el MVP en versión mínima (crear, marcar, racha, desmarcar) | **Decidido por ti.** Evidencia de entrevistas débil: 2 menciones solo en la redacción de la HU, 2 personas que lo llevan mentalmente sin dolor, 0 dolores explícitos. En la beta se mide si se usan; si no, no pasan a la siguiente versión |
| Exportar CSV (HU-09) pasa a P3; el respaldo lo cubre una tarea técnica invisible (copias de la base) | Propuesta |
| Guardar un gasto sin internet (cola local) | Backlog, con evidencia cualitativa (Margarita, "estable") |
| Confirmar recordatorios en la próxima ronda de entrevistas (50% vs. umbral de 60%) | Pendiente |

---

## 9. Estado

- [ ] Corregir la tabla de entrevistas (acreditación de Mariana en tareas; la de Isabella en hábitos ya se corrigió)
- [x] Decidir hábitos: incluidos en versión mínima (plazo ~9 semanas)
- [ ] Actualizar el documento del proyecto a la v0.3 con esta matriz
- [ ] Experimento técnico de notificaciones (semana 1)
- [ ] Pseudocódigo del módulo siguiente justo antes de implementarlo
