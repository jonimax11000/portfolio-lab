# SPEC-001 — Portfolio personal de desarrollador

|          |            |
| -------- | ---------- |
| Estado   | Aprobado   |
| Versión | v1.0       |
| Fecha    | 2026-10-16 |

## 1. Context

Arema Software recibe cada curso CVs y enlaces a GitHub de alumnos de prácticas. El problema es que un repositorio sin contexto no permite valorar bien: no se sabe qué hacía el proyecto, con qué se hizo ni si llegó a usarlo alguien.

Marta Ibáñez (Arema Software) pide una página web por alumno que funcione como portfolio, se abra bien desde el móvil y muestre quién es el alumno, sus proyectos y cómo contactarle. De cada proyecto quiere ver la imagen, el nombre, de qué va, con qué está hecho y el enlace al repositorio.

Se confirma que **no hay backend ni validación de correo corporativo**: no hay dominio para ello. El contacto se resuelve con `mailto:` estándar. Se documenta como limitación conocida que un correo no corporativo puede escribir igualmente.

Este spec es el vehículo para practicar SDD. El producto importa, pero el objetivo principal es el proceso: agentes + specs + desarrollo + QA + cambios.

## 2. Goal

Una página web que sirva de portfolio personal de un alumno, responsive, multiidioma (ES/EN), accesible para ciegos y daltónicos, con filtros, paginación y una forma de contactarle, pensada para que una empresa de prácticas valore al alumno desde el móvil.

## 3. Users

- **Empresas que buscan contratar alumnos de prácticas.** Usuario primario. Revisan el portfolio desde móvil o escritorio, filtran proyectos y contactan.
- **Alumno autor.** Mantiene `profile.json`, `projects.json` y los assets a mano. No toca código.
- **Agente desarrollador.** Implementa a partir de este spec. No decide nada que esté en Open Questions.
- **Agente QA.** Verifica los Acceptance Criteria. Tiene su propio spec.
- **Agente PM.** Gestiona alcance y cambios. Tiene su propio spec.

El visitante es un usuario no autenticado y solo lectura.

## 4. Scope

### In scope

- Página web responsive, mobile-first, que funcione en pantallas muy pequeñas.
- Multiidioma ES/EN con botón en la cabecera.
- Cabecera sticky y colapsable si la resolución no da.
- Cabecera con foto, nombre, titular, claim, ubicación, disponibilidad, stack, correo, GitHub y LinkedIn.
- Secciones “Sobre mí” y “Ahora” en páginas aparte.
- Listado dinámico de proyectos desde `projects.json`.
- Tarjetas de proyecto con imagen, nombre, descripción, tecnología, fecha y enlace.
- Placeholder cuando no hay imagen o falla.
- Expansión de tarjetas una a la vez, con animación y bloqueo durante la transición + 300 ms.
- “No disponible” cuando un proyecto no tiene enlace.
- Paginación de 5 proyectos.
- Filtros por tecnología y año, combinables.
- Contador de resultados.
- Sección de destacados independiente del listado general.
- Orden ascendente y descendente por fecha.
- Contacto vía `mailto:` estándar.
- Modo claro/oscuro y modo alto contraste.
- Accesibilidad para ciegos y daltónicos.
- Carga inicial <1s en 3G, con carga dinámica de lo necesario.
- Arquitectura MVC.
- Web Components para las tarjetas de proyecto.
- Metodología SOLID.
- Datos en `data/profile.json` y `data/projects.json`.
- Diseño según `design/DESIGN.md`.
- Assets en `assets/`.
- Estado vacío global y estado vacío por filtros, con mensajes distintos.

### Out of scope

Guardarraíles: el agente **no debe** hacer nada de esto.

- No cambiar requisitos.
- No inventar funcionalidades.
- No modificar decisiones de producto.
- No ignorar criterios de aceptación.
- No implementar nada que esté en Open Questions sin preguntar antes.
- No añadir backend.
- No añadir validación de correo corporativo.
- No añadir buscador de texto.
- No añadir edición online de datos.
- No añadir CMS.
- No añadir autenticación.
- No añadir demo en vivo de proyectos.
- No añadir analítica.
- No añadir CI.
- No añadir SemVer ni CHANGELOG.
- No añadir persistencia de filtros en URL.
- No añadir scroll infinito.
- No restringir navegador ni sistema operativo.
- No cambiar la estructura de los JSON.

## 5. Requirements

### R1

La cabecera muestra foto, nombre, titular, claim, ubicación, disponibilidad, stack, correo, GitHub y LinkedIn.

### R2

La cabecera incluye un botón de cambio de idioma ES/EN.

### R3

La cabecera incluye un botón de contacto.

### R4

La cabecera es sticky y colapsa si la resolución no da.

### R5

Al colapsar la cabecera solo se muestran imagen, nombre y botón de contacto.

### R6

La sección “Sobre mí” muestra `presentacion` y `sobreMi[]` en una página aparte.

### R7

La sección “Ahora” muestra `ahora[]` en una página aparte.

### R8

La web carga `projects.json` y renderiza una tarjeta por proyecto.

### R9

Cada tarjeta muestra imagen, nombre, descripción, tecnología, fecha y enlace.

### R10

Si falta la imagen o falla, se muestra un placeholder.

### R11

Al pulsar una tarjeta, se expande con información adicional. Al volver a pulsarla, se colapsa.

### R12

Solo puede haber una tarjeta expandida a la vez.

### R13

Durante la transición de expansión/colapso y 300 ms después, los clicks en tarjetas quedan bloqueados.

### R14

Si un proyecto no tiene enlace, se muestra “No disponible”.

### R15

El enlace del proyecto abre en nueva pestaña con `rel="noopener noreferrer"`.

### R16

Hay paginación de 5 proyectos.

### R17

Hay filtros por tecnología y año.

### R18

Los filtros son combinables entre sí.

### R19

Hay un contador de resultados.

### R20

Hay una sección de destacados independiente del listado general.

### R21

El listado general y la sección de destacados se ordenan por fecha, con opción ascendente y descendente.

### R22

El badge de destacado lleva texto, no solo color.

### R23

El botón de contacto abre el cliente de correo con `mailto:` estándar.

### R24

Los datos se cargan dinámicamente desde JSON, sin datos hardcodeados.

### R25

La web es responsive de 320px a 1920px sin scroll horizontal.

### R26

La información es solo lectura; no hay formularios de edición.

### R27

Si `profile.json` o `projects.json` fallan, se muestra un estado de error controlado.

### R28

Si no hay proyectos, se muestra un mensaje de estado vacío global.

### R29

Si los filtros no devuelven resultados, se muestra un mensaje de estado vacío por filtros, distinto al global.

### R30

Hay modo claro/oscuro.

### R31

Hay modo alto contraste.

### R32

La web es accesible para ciegos y daltónicos.

### R33

La carga inicial es <1s en 3G, con carga dinámica de lo necesario.

### R34

Se respeta `prefers-reduced-motion`: si está activo, no se animan las tarjetas.

## 6. Business Rules

- No se permite a los lectores modificar información.
- Cuando se pulsa un proyecto, la información se expande.
- Cuando se pulsa la dirección del repositorio, accede a este en la web en otra pestaña.
- Si no hay imagen en los proyectos, sale un placeholder.
- Cuando se pulsa el botón de contacto, se abre la app o web de correo del usuario mediante `mailto:` estándar. No se puede forzar desde HTML que sea app o web; lo decide el sistema.
- La información personal no incluye datos que permitan saltarse la validación del correo corporativo, porque no hay validación.
- Los datos se cargan de JSON, de manera dinámica.
- Los JSON son inmutables en esta versión.
- `tecnologia` es un string único por proyecto.
- La fecha se normaliza en el front a formato “9 de Septiembre” a partir de `YYYY-MM`. El JSON no se toca.
- No hay campo `tipo` en los proyectos.
- Los filtros son Must y combinables.
- Al filtrar, la paginación no cambia de página.
- El orden por defecto es fecha descendente.
- El idioma por defecto es español.
- Los archivos JSON no se traducen, pero la web tiene que verse en el otro idioma.
- `sobreMi`, `ahora`, `claim` y `presentacion` se traducen.
- El botón de contacto está solo en la cabecera.
- Si la resolución da, la cabecera no colapsa; si no da, colapsa por scroll.
- Al colapsar la cabecera solo quedan imagen, nombre y botón de contacto.
- El agente no decide nada. Todo lo que esté en Open Questions se pregunta antes de programar.
- Los guardarraíles de Out of scope son obligatorios.

## 7. User Flow

1. La empresa entra a la web.
2. Ve la cabecera con información personal, botón de idioma y botón de contacto.
3. Ve la sección de destacados.
4. Ve el listado general paginado de 5 en 5.
5. Aplica filtros por tecnología y/o año.
6. Ve el contador de resultados.
7. Pulsa una tarjeta: se expande con animación.
8. Vuelve a pulsarla: se colapsa.
9. Pulsa el enlace del repositorio: se abre en nueva pestaña.
10. Si no hay enlace: ve “No disponible”.
11. Cambia el idioma: toda la UI se traduce.
12. Cambia el orden: ascendente o descendente por fecha.
13. Pulsa el botón de contacto: se abre el cliente de correo con `mailto:`.
14. Accede a “Sobre mí” y “Ahora” en páginas aparte.

**Flujos alternativos:**

- Si `projects.json` falla: estado de error controlado.
- Si no hay proyectos: estado vacío global.
- Si los filtros no devuelven resultados: estado vacío por filtros.
- Si `prefers-reduced-motion` está activo: sin animaciones.

## 8. Acceptance Criteria

- [ ] Los proyectos tienen imagen, nombre, descripción, tecnología, fecha y enlace.
- [ ] Si no hay imagen, aparece un placeholder.
- [ ] Si no hay enlace, aparece “No disponible”.
- [ ] El enlace del repositorio se abre en nueva pestaña.
- [ ] Al pulsar una tarjeta se expande; al volver a pulsarla se colapsa.
- [ ] Solo hay una tarjeta expandida a la vez.
- [ ] Durante la transición y 300 ms después, los clicks quedan bloqueados.
- [ ] Hay paginación de 5 proyectos.
- [ ] Hay filtros por tecnología y año, combinables.
- [ ] Hay contador de resultados.
- [ ] Hay sección de destacados independiente.
- [ ] Hay orden ascendente y descendente por fecha.
- [ ] El badge de destacado lleva texto.
- [ ] El botón de contacto abre el cliente de correo con `mailto:`.
- [ ] La web se ve bien en móviles y pantallas muy pequeñas.
- [ ] No hay scroll horizontal de 320px a 1920px.
- [ ] La cabecera es sticky y colapsa si la resolución no da.
- [ ] Al colapsar solo quedan imagen, nombre y botón de contacto.
- [ ] Hay cambio de idioma ES/EN en la cabecera.
- [ ] La UI se traduce, los JSON no.
- [ ] Hay modo claro/oscuro.
- [ ] Hay modo alto contraste.
- [ ] La web es accesible para ciegos y daltónicos.
- [ ] Se respeta `prefers-reduced-motion`.
- [ ] Si no hay proyectos, aparece un mensaje de estado vacío global.
- [ ] Si los filtros no devuelven resultados, aparece un mensaje de estado vacío por filtros.
- [ ] Si `profile.json` o `projects.json` fallan, aparece un estado de error controlado.
- [ ] La carga inicial es <1s en 3G.
- [ ] La información es solo lectura.
- [ ] Los datos se cargan desde JSON, sin hardcodeos.
- [ ] La arquitectura es MVC.
- [ ] Las tarjetas de proyecto son Web Components.
- [ ] Se sigue SOLID.

## 9. Edge Cases

- Doble click o click rápido repetido en una tarjeta.
- Click durante la transición de expansión/colapso.
- Email no corporativo: puede escribir igualmente, es limitación conocida de `mailto:`.
- Imagen ausente, rota o con URL inválida.
- `projects.json` vacío, malformado o con campos faltantes.
- Proyecto sin enlace.
- Proyecto sin `tecnologia`.
- Proyecto sin `fecha`.
- `fecha` inválida.
- Filtro sin resultados.
- Filtro combinado sin resultados.
- Filtro + paginación + expansión simultánea.
- Filtro por tecnología con mayúsculas o tildes.
- Búsqueda con acentos (no aplica, no hay buscador, pero aplica a filtros).
- Muchos proyectos (>50).
- Cambio de idioma con tarjeta expandida.
- Cambio de idioma con filtros activos.
- Cabecera colapsada con tarjeta expandida.
- Cabecera colapsada con contacto abierto.
- Navegación por teclado en filtros y tarjetas.
- `prefers-reduced-motion` activo.
- Lector de pantalla en tarjeta expandida.
- Zoom 200%.
- Viewport 320px.
- `correo` con formato raro.
- `github` o `linkedin` ausentes.
- `sobreMi` vacío.
- `ahora` vacío.
- `stack` con categorías vacías.
- Proyectos sin fecha en orden ascendente/descendente.
- Paginación con proyectos sin fecha.
- Estado vacío global vs estado vacío por filtros.

## 10. Open Questions

El agente **debe preguntar todo esto antes de programar**. No puede inventar ni asumir.

1. ¿Cómo se marcan los destacados? ¿Campo `destacado` en JSON, `order` manual, array aparte?
2. ¿Cuántos destacados se muestran?
3. ¿Los destacados también se paginan o van en su propia sección sin paginación?
4. ¿El badge de destacado lleva texto y qué texto?
5. ¿Los destacados se ordenan por fecha como el resto?
6. ¿Los filtros en móvil se muestran inline, en drawer o colapsables?
7. ¿Los filtros se recuerdan al cambiar de idioma?
8. ¿Los filtros se recuerdan al paginar?
9. ¿El estado vacío por filtros incluye botón “limpiar filtros”?
10. ¿Qué pasa con proyectos sin fecha en el orden ascendente/descendente?
11. ¿El orden se recuerda al paginar o se resetea?
12. ¿El idioma persiste en `localStorage`, sesión o solo en memoria?
13. ¿Se detecta el idioma del navegador al entrar o siempre español por defecto?
14. ¿La paginación es de 5 fija o configurable?
15. ¿`mailto:` lleva asunto y cuerpo predefinidos? ¿En ES/EN según idioma?
16. ¿El correo se muestra en texto visible o solo como enlace `mailto:`?
17. ¿Al colapsar la cabecera se oculta también `claim`, `disponibilidad` y `stack`, o solo foto/nombre?
18. ¿<1s en 3G es AC formal o recomendación?
19. ¿Carga dinámica significa lazy load de imágenes y de secciones, o solo de proyectos?
20. ¿Stack concreto (Vanilla + Vite, Lit, Preact, Svelte…)?
21. ¿Web Components nativos o con librería?
22. ¿Estructura de carpetas exacta?
23. ¿WCAG 2.1 AA es AC formal?
24. ¿Modo alto contraste y claro/oscuro son AC formales o nice-to-have?
25. ¿El agente debe preguntar todo esto antes de empezar o puede empezar por lo cerrado y preguntar después?

## 11. Assumptions

- El visitante es un reclutador de una empresa.
- El alumno mantiene los JSON y los assets a mano.
- Los JSON son inmutables en esta versión.
- `tecnologia` es un string único por proyecto.
- No hay campo `tipo`.
- No hay backend.
- No hay validación de correo corporativo.
- El hosting es GitHub Pages.
- El deploy es manual.
- El diseño está en `design/DESIGN.md`.
- Las imágenes están en `assets/`.
- El objetivo principal es el proceso SDD, no solo el portfolio.
- El agente QA tiene su propio spec.
- El agente PM tiene su propio spec.
- Este es el spec del proyecto, no el del agente.
- El agente desarrollador no decide nada; pregunta lo que esté en Open Questions.
- Los guardarraíles de Out of scope son obligatorios.

## 12. Dependencies

- `Datos del perfil del alumno data/profile.json`
- `Datos de los proyectos data/projects.json`
- `Documentación para el diseño design/DESIGN.md`
- `Imagenes en assets/`
- Arquitectura MVC
- Web Components
- SOLID
