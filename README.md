# SubvencIA

Proyectos de financiación de asociaciones de pacientes, mejorados frente a la rúbrica de la convocatoria. Aplicación web de un solo fichero, del curso «IA para asociaciones de pacientes» del Instituto ProPatiens y la Cátedra del Paciente de la Universidad Miguel Hernández de Elche.

**Usar la app:** https://fborrasumh.github.io/subvencia/ · **Portal del curso:** https://fborrasumh.github.io/propatiens/

**Idiomas:** castellano (por defecto), català, euskara, galego, English y português (selector en la barra superior o `?lang=` en la URL).

## Qué hace

1. **La convocatoria.** Se suben las bases (Word, PDF o texto) o se pega su texto.
2. **El proyecto.** Se sube el borrador o el esquema y, si se quiere, otros ficheros en la misma zona o en la de datos de la asociación: memoria, presupuesto, cronograma, encuestas… en Word, PDF, texto u hoja de cálculo (`.xlsx`, `.ods`, `.csv`; las `.xls` antiguas hay que guardarlas antes como `.xlsx`). El primer documento de texto es el borrador; lo demás pasa a los datos de la asociación, que la IA usa como material (hasta unos 20.000 caracteres por llamada). De las hojas de cálculo se lee cada hoja fila a fila, con el valor calculado de las fórmulas. También se puede subir el perfil común. El código divide el proyecto en secciones por sus títulos.
3. **La rúbrica.** La IA extrae los criterios de valoración con sus puntos, qué mira el tribunal en cada uno, los requisitos de admisión y las dudas que conviene consultar. Se puede corregir a mano.
4. **Ciclo 0: la estructura** (opcional). Antes de los ciclos, la IA propone poner el proyecto en la forma que pide la convocatoria: secciones en el orden de las bases, las que faltan redactadas como propuesta y lo que ya está escrito movido a su sitio sin reescribirlo. El código compara cada sección propuesta con la original (sin cambios, renombrada, cambiada o nueva; qué parte del texto conserva; cifras nuevas) y **la asociación acepta o descarta cada sección**. Una sección con cifras que no están en el material y que la IA no declara como propuesta no se puede aceptar. Las secciones originales que la propuesta no usa se conservan al final salvo que se desmarquen; si su texto ya está dentro de otra sección, se proponen desmarcadas para no duplicarlas. Se puntúa antes y después, queda en el historial como ciclo 0 y se puede deshacer mientras no haya ciclos de mejora.
5. **La mejora por ciclos**, con la lógica de *autoresearch*: se evalúa el proyecto; se elige el criterio con más margen; la IA formula una hipótesis y reescribe solo esa sección; se vuelve a evaluar y el cambio se conserva solo si sube el nivel de ese criterio sin que otro baje dos niveles. Se repite hasta el objetivo o el número de ciclos (3 a 10).
6. **El resultado.** Comparación inicial-final por criterio, el proyecto sección a sección (editable) con las cifras propuestas que hay que revisar, y descargas en Word: el proyecto mejorado y el informe de ciclos (rúbrica, puntuaciones, cada ciclo con su hipótesis y su decisión, cifras propuestas, requisitos como lista para marcar y dudas). La rúbrica también en CSV.
7. **Borrar.** «Borrar los ciclos» (paso de mejora) devuelve el proyecto a su versión inicial y conserva bases, documentos y rúbrica. «Empezar de cero» (inicio) borra bases, proyecto, documentos, rúbrica y ciclos de este navegador; el perfil de la asociación se conserva.

## Qué comprueba el código (la IA propone, el código comprueba)

- **Rúbrica:** cada criterio y cada requisito llevan una cita literal de las bases, y el código comprueba que existe. También comprueba que los puntos aparecen en la cita y que la suma de criterios cuadra con el máximo que dicen las bases.
- **Puntuación:** la IA da a cada criterio un nivel de 0 a 4 según los elementos que mira la rúbrica, con una cita literal del proyecto. Si la cita no está en el proyecto, el nivel no pasa de 1. **La suma la hace el código** con los pesos de las bases: puntos = peso × nivel / 4.
- **Ciclos:** un cambio se rechaza, sin llegar a puntuarse, si introduce cifras que no están en las bases, el proyecto, los documentos de la asociación o el perfil y que la IA no declara como propuesta; si deja más de 2 `[COMPLETAR]` en la sección (o más de los que ya tenía); si no cambia nada; si recorta la sección a menos de la mitad; o si apunta a una sección que no existe. Decide el criterio del ciclo, no la suma: se conserva solo si sube su nivel y ningún otro criterio baja 2 niveles o más; los demás criterios conservan su nivel anterior (salvo que su cita haya desaparecido), para que el ruido de la IA al volver a puntuarlo todo no tumbe una mejora real. «Volver a puntuar» puntúa todo de nuevo. Un criterio que falla dos veces seguidas se aparca, y el siguiente intento sobre un criterio conoce los intentos fallidos anteriores.
- **Hechos y propuestas:** la IA redacta y propone lo que la asociación puede decidir (objetivos y metas, sesiones, plazas, horas, calendario, criterios de acceso, participación, desglose del presupuesto en unidades × coste unitario, medidas de igualdad, accesibilidad, voluntariado y continuidad) y declara cada cifra que propone; el código la deja pasar y la lista para revisar. Los hechos (datos del territorio, resultados anteriores, fechas, entidades, acuerdos) solo salen del material: una cifra nueva no declarada se rechaza, y lo que de verdad falta se marca `[COMPLETAR: …]`, como mucho 2 por sección. Los trámites (estar al corriente, estatutos, plazo…) no van en la memoria: están en la lista de requisitos.
- **Cifras con separador de miles:** «3.500», «3,500» y «3500» se tratan como la misma cifra, así que un importe inventado no se cuela por llevar punto de miles.

Este diseño busca que el ruido de la IA al puntuar no se confunda con mejora: los niveles se atan a elementos concretos y a citas comprobables, y los cambios que se inventan hechos no entran aunque «puntúen».

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
