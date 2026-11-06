# SubvencIA

Proyectos de financiación de asociaciones de pacientes, mejorados frente a la rúbrica de la convocatoria. Aplicación web de un solo fichero, del curso «IA para asociaciones de pacientes» del Instituto ProPatiens y la Cátedra del Paciente de la Universidad Miguel Hernández de Elche.

**Usar la app:** https://fborrasumh.github.io/subvencia/ · **Portal del curso:** https://fborrasumh.github.io/propatiens/

**Idiomas:** castellano (por defecto), català, euskara, galego, English y português (selector en la barra superior o `?lang=` en la URL).

## Qué hace

1. **La convocatoria.** Se suben las bases (Word, PDF o texto) o se pega su texto.
2. **El proyecto.** Se sube el borrador o el esquema y, si se quiere, documentos de la asociación (memoria, presupuesto…) y el perfil común. El código divide el proyecto en secciones por sus títulos.
3. **La rúbrica.** La IA extrae los criterios de valoración con sus puntos, qué mira el tribunal en cada uno, los requisitos de admisión y las dudas que conviene consultar. Se puede corregir a mano.
4. **La mejora por ciclos**, con la lógica de *autoresearch*: se evalúa el proyecto; se elige el criterio con más margen; la IA formula una hipótesis y reescribe solo esa sección; se vuelve a evaluar y el cambio se conserva solo si la puntuación sube. Se repite hasta el objetivo o el número de ciclos (3 a 10).
5. **El resultado.** Comparación inicial-final por criterio, el proyecto sección a sección (editable) y descargas en Word: el proyecto mejorado y el informe de ciclos (rúbrica, puntuaciones, cada ciclo con su hipótesis y su decisión, requisitos y dudas). La rúbrica también en CSV.

## Qué comprueba el código (la IA propone, el código comprueba)

- **Rúbrica:** cada criterio y cada requisito llevan una cita literal de las bases, y el código comprueba que existe. También comprueba que los puntos aparecen en la cita y que la suma de criterios cuadra con el máximo que dicen las bases.
- **Puntuación:** la IA da a cada criterio un nivel de 0 a 4 según los elementos que mira la rúbrica, con una cita literal del proyecto. Si la cita no está en el proyecto, el nivel no pasa de 1. **La suma la hace el código** con los pesos de las bases: puntos = peso × nivel / 4.
- **Ciclos:** un cambio se rechaza, sin llegar a puntuarse, si introduce cifras que no están en las bases, el proyecto, los documentos de la asociación o el perfil; si no cambia nada; si recorta la sección a menos de la mitad; o si apunta a una sección que no existe. Se conserva solo si la puntuación sube; si no, se deshace. Un criterio que falla dos veces seguidas se aparca, y el siguiente intento sobre un criterio conoce los intentos fallidos anteriores.
- **Nada inventado:** lo que falta se marca `[COMPLETAR: …]` y se cuenta.

Este diseño busca que el ruido de la IA al puntuar no se confunda con mejora: los niveles se atan a elementos concretos y a citas comprobables, y los cambios que se inventan datos no entran aunque «puntúen».

## Perfil común de la asociación

El mismo JSON (`perfil-asociacion-pacientes`) que en DifundIA y las demás apps del curso: se sube, se rellena aquí si hace falta y se descarga para la siguiente. El kit de marca (colores y logo) no se envía a la IA.

## Cómo se usa la IA

Con la propia clave de OpenAI, Google Gemini o Anthropic Claude, guardada solo en el navegador. Cada ciclo hace dos llamadas (reescribir y puntuar); un ciclo rechazado por el código hace solo una.

## Privacidad

Bases, proyecto, documentos, rúbrica y versiones se guardan solo en el navegador (IndexedDB). Lo que sale hacia el proveedor va con correos, DNI/NIE, teléfonos e IBAN enmascarados, y antes del primer envío de las bases y del primer envío del proyecto se muestra lo que sale. Solo documentación anonimizada.

## Límites

- **La puntuación es una simulación:** orienta, pero no garantiza la del tribunal. Puntuar dos veces el mismo texto puede dar niveles algo distintos.
- La rúbrica depende de cómo estén escritas las bases: si los criterios están en un anexo o en una tabla mal extraída del PDF, revisadla a mano.
- Los PDF escaneados (sin texto) no se pueden leer.
- Las cifras pequeñas (hasta 12) no se comprueban, para no marcar fechas u ordinales corrientes.
- No comprueba que la asociación cumpla los requisitos: los lista para que lo comprobéis.
- Probada con IA simulada de los tres proveedores; no con una clave real.

## Autoría

Fernando Borrás Rocher (Universidad Miguel Hernández de Elche) · ORCID [0000-0002-5519-4573](https://orcid.org/0000-0002-5519-4573)

Licencia MIT.
