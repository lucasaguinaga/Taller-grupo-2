# Taller CineLikes

## Enunciado

Un cineclub universitario necesita una aplicación para administrar su catálogo de películas. Los participantes podrán registrar películas, localizar las que les interesan, editar sus datos y expresar sus preferencias mediante un botón «Me gusta». El cineclub desea consultar un ranking de popularidad que conserve las películas empatadas.

Aprovechen la estructura del proyecto desarrollado en clases y adáptenla a este contexto. Utilicen **ArrayList, búsqueda lineal, búsqueda binaria, ordenamiento burbuja y Java Swing**. La lógica debe quedar separada de los eventos de la interfaz.

El equipo trabajará en **un único repositorio de GitHub**, con commits de código desde **al menos tres cuentas distintas**. Cada integrante debe realizar al menos un commit significativo y poder explicar su aporte. No se exigen ramas ni pull requests.

## Datos de la película

| Atributo | Tipo | Regla |
|---|---|---|
| `codigo` | `int` | Positivo, único e inmutable después del registro. |
| `titulo` | `String` | Obligatorio; puede repetirse en películas con códigos diferentes. |
| `director` | `String` | Obligatorio; puede repetirse. |
| `genero` | `String` | Obligatorio; seleccionado de un JComboBox. |
| `anio` | `int` | Desde 1 hasta el año actual. |
| `meGusta` | `int` | No negativo; inicialmente 0 para películas nuevas. Solo aumenta mediante el botón Me gusta. |

Los géneros del ejercicio son Ciencia ficción, Drama, Aventura, Comedia, Animación, Suspenso y Documental.

## Organización de los datos

La colección principal será `ArrayList<Pelicula> peliculas`. Manténganla ordenada por código ascendente para utilizar la búsqueda binaria.

Para el ranking, construyan una copia con `new ArrayList<>(peliculas)` y ordénenla mediante burbuja por me gusta descendente. La lista principal debe conservar su orden por código: la búsqueda binaria no funciona correctamente si el orden utilizado es otro.

Las listas de resultados y ranking deben representar los mismos registros del catálogo. No creen películas nuevas para simular una edición o un incremento del contador. Una búsqueda no elimina ni reemplaza datos de la colección principal.

## Requerimientos funcionales

### R1 Cargar veinte películas al iniciar

- Precargar exactamente veinte películas mediante `DatosIniciales.cargar()` o un método equivalente.
- Incluir códigos únicos y valores repetidos en género, año y me gusta.
- Los datos se entregan desordenados: aplicar burbuja por código antes de permitir búsquedas binarias.
- Cargar una sola vez durante la ejecución. Cambiar de pestaña, buscar o actualizar el ranking no debe reiniciar los contadores ni duplicar registros.
- Mostrar la cantidad actual de películas. Trabajar en memoria; no se exige persistencia en archivos ni base de datos.

### R2 Registrar nuevas películas

- Incorporar un formulario con código, título, director, género y año, más el botón Registrar.
- Validar los campos obligatorios, el código positivo, el año permitido y que no exista otro registro con el mismo código.
- Las nuevas películas comienzan con cero me gusta; ese valor no se escribe en el formulario.
- Registrar el objeto en el ArrayList y ordenar por código con burbuja para conservar la condición necesaria de la búsqueda binaria.
- Si hay errores, mostrar un mensaje sin registrar parcialmente la película ni cambiar los datos anteriores.

Debe poder registrarse un código menor que los existentes, por ejemplo 1999. No se puede asumir que los nuevos códigos siempre pertenecen al final de la lista ordenada.

### R3 Buscar coincidencias de texto y devolver un ArrayList

Implementar una búsqueda lineal sobre título y director:

```java
public ArrayList<Pelicula> buscarCoincidencias(String texto)
```

- Ignorar mayúsculas y minúsculas y quitar espacios al inicio y al final de la consulta.
- Incluir una película si el fragmento aparece en el título **o** en el nombre del director.
- Si coincide en ambos campos, incluirla una sola vez.
- Devolver un ArrayList nuevo con todas las coincidencias, en orden por código. Si no existen, devolver uno vacío y no `null`.
- Rechazar consultas nulas, vacías o formadas solo por espacios.
- Mostrar los resultados en una `JList<Pelicula>` mediante `DefaultListModel<Pelicula>`, junto con su cantidad.
- Permitir seleccionar una película para ver sus datos.

No es obligatorio ignorar tildes. La búsqueda `ESPACIO` debe obtener lo mismo que `espacio`.

### R4 Buscar por código mediante búsqueda binaria y editar

Implementar una búsqueda binaria propia sobre la colección principal:

```java
public int buscarIndicePorCodigo(int codigo)
```

- Devolver el índice correspondiente o `-1` cuando no exista.
- Rechazar códigos no positivos.
- Al encontrar una película, cargar sus datos en el formulario de edición y seleccionarla en la lista de resultados.
- Permitir modificar título, director, género y año. No editar código ni escribir manualmente el contador de me gusta.
- Validar todos los nuevos valores antes de guardar. Si uno falla, no modificar parcialmente los otros atributos.
- Si el código no se encuentra, mostrar un aviso y limpiar cualquier selección anterior para evitar cambios sobre otra película.
- Actualizar los datos visibles después de guardar, conservando el código y la cantidad de me gusta.

También se puede seleccionar para editar una película encontrada por texto. Identifiquen el registro por su código único: el índice de la JList puede ser diferente al índice del ArrayList principal.

### R5 Dar me gusta a una película seleccionada

- Buscar por código o texto, seleccionar una película y pulsar **Me gusta +1**.
- Cada clic válido incrementa únicamente en uno el contador del registro seleccionado.
- Mostrar el contador actualizado y refrescar las vistas correspondientes.
- Sin una selección válida, mostrar un aviso sin modificar ninguna película.
- Para esta simulación se permiten varios clics sobre la misma película. No se requieren cuentas de espectadores ni la operación de quitar me gusta.

Seleccionar una película o refrescar una JList no incrementa el contador. El incremento ocurre únicamente al pulsar el botón.

### R6 Ordenar el ranking mediante burbuja

Implementar el siguiente método:

```java
public ArrayList<Pelicula> obtenerRanking()
```

- Devolver una copia de la colección principal, ordenada por me gusta descendente mediante burbuja.
- Conservar todas las películas, incluso cuando tengan la misma cantidad de me gusta.
- En los empates, presentar primero el código menor. Pueden partir de la lista principal ordenada por código y aplicar burbuja estable, intercambiando solo cuando el contador de la izquierda sea menor que el de la derecha.
- Mostrar en una JList el código, título y contador de cada película.
- Actualizar el ranking al entrar en la pestaña o pulsar Actualizar.
- Después de consultar el ranking, la búsqueda binaria por código debe seguir funcionando correctamente.

## Interfaz requerida con JTabbedPane

La interfaz debe ser elaborada manualmente por los estudiantes. El equipo deberá identificar y explicar los controles y eventos que implementó.

| Pestaña | Controles mínimos | Función |
|---|---|---|
| Registro | JTextField para código, título y director; JComboBox para género; JSpinner para año; botón Registrar y total de películas. | Ingreso validado de nuevos registros. |
| Búsquedas y edición | Campos para código y texto; botones Buscar por código y Buscar coincidencias; JList de resultados; formulario editable; botones Guardar cambios y Me gusta +1. | Localizar, seleccionar, editar y expresar una preferencia. |
| Ranking | JList con código, título y contador; botón Actualizar. | Mostrar popularidad sin cambiar el orden principal. |

Después de registrar, editar o dar me gusta, actualizar las vistas afectadas sin volver a cargar los datos iniciales. Si una edición hace que la película deje de coincidir con el texto buscado, recalcular los resultados y limpiar la selección cuando corresponda.

## Organización sugerida del código

| Clase | Responsabilidad |
|---|---|
| `Pelicula` | Datos y operaciones de los atributos de la película. |
| `CatalogoPeliculas` | ArrayList principal, validaciones, búsquedas, edición, me gusta y ordenamientos. |
| `DatosIniciales` | Construir las veinte películas de demostración. |
| `Ventana` | JTabbedPane, componentes, selección y mensajes. |

Firmas orientativas adicionales:

```java
public void registrar(Pelicula pelicula)
public void editarPelicula(int codigo, String titulo, String director,
                           String genero, int anio)
public void darMeGusta(int codigo)
public void ordenarPorCodigoBurbuja()
```

Los nombres pueden adaptarse al proyecto, conservando los comportamientos solicitados. Para resolver los algoritmos evaluados no utilizar `Collections.sort()`, `List.sort()`, `Arrays.sort()`, `stream().sorted()` ni `Collections.binarySearch()`.

## Veinte registros de demostración

Todas las películas, directores y años siguientes son ficticios y se utilizan exclusivamente como datos de práctica.

| Código | Título | Director | Género | Año | Me gusta |
|---|---|---|---|---|---|
| 2001 | La ciudad de los faros | Ana Torres | Drama | 2021 | 4 |
| 2002 | Viaje al espacio | Luis Mora | Ciencia ficción | 2020 | 1 |
| 2003 | El último mapa | Carla Ríos | Aventura | 2022 | 6 |
| 2004 | Una tarde inesperada | Pedro Vega | Comedia | 2019 | 2 |
| 2005 | Guardianes del bosque | Sofía Paz | Animación | 2023 | 0 |
| 2006 | Sombras del puerto | Diego Ruiz | Suspenso | 2021 | 6 |
| 2007 | Ecos del espacio | Elena Ramos | Ciencia ficción | 2020 | 3 |
| 2008 | Caminos de la montaña | Pablo Soto | Documental | 2024 | 1 |
| 2009 | El café de la esquina | Laura Díaz | Drama | 2022 | 5 |
| 2010 | La isla del viento | Marco Vera | Aventura | 2020 | 2 |
| 2011 | Un plan diferente | Daniela Vega | Comedia | 2025 | 6 |
| 2012 | El reloj de papel | Andrés Luna | Animación | 2021 | 0 |
| 2013 | Señales desde el espacio | Gabriela Ríos | Ciencia ficción | 2023 | 4 |
| 2014 | La puerta cerrada | Mateo Peña | Suspenso | 2022 | 3 |
| 2015 | Memorias del río | Valeria Sol | Documental | 2024 | 5 |
| 2016 | Cartas de invierno | Nicolás López | Drama | 2025 | 1 |
| 2017 | La ruta del tesoro | Paula Reyes | Aventura | 2021 | 2 |
| 2018 | Vecinos por un día | Camila Vega | Comedia | 2023 | 0 |
| 2019 | Amigos del espacio | Tomás León | Animación | 2024 | 4 |
| 2020 | El puente de cristal | Isabel Rojas | Suspenso | 2025 | 3 |

El archivo `DatosIniciales.java` entrega estos registros desordenados para que el equipo aplique burbuja al iniciar. Se proporciona `Pelicula.java` como modelo de referencia; pueden adaptar el constructor y los paquetes a su proyecto. No se entrega la implementación de los algoritmos evaluados.

## Casos de comprobación

| Caso | Acción | Resultado esperado |
|---|---|---|
| 1 | Iniciar y cambiar repetidamente de pestaña. | Veinte registros sin duplicaciones ni reinicio de contadores. |
| 2 | Registrar de nuevo el código 2001. | Error; se conserva el registro y la cantidad original. |
| 3 | Registrar código 1999 con datos válidos. | Total 21; queda antes de 2001 en la lista principal y la búsqueda binaria lo encuentra. |
| 4 | Buscar `ESPACIO` con los datos iniciales. | ArrayList con 2002, 2007, 2013 y 2019. |
| 5 | Buscar `vega` con los datos iniciales. | ArrayList con 2004, 2011 y 2018 por coincidencia en director. |
| 6 | Buscar `puerto` con los datos iniciales. | ArrayList con 2006. |
| 7 | Buscar texto inexistente. | ArrayList vacío; se limpia la selección anterior. |
| 8 | Buscar 2015 mediante binaria y editar el título. | Solo se modifica esa película; código y contador se conservan. |
| 9 | Intentar editar con director vacío. | Error; no hay cambios parciales en ningún atributo. |
| 10 | Consultar el ranking inicial. | Primeros: 2003, 2006 y 2011, todos con 6; permanecen las veinte películas. |
| 11 | Buscar 2001 y pulsar tres veces Me gusta +1. | El contador pasa de 4 a 7; al actualizar el ranking queda primero. |
| 12 | Después de ordenar el ranking, buscar 2005 mediante binaria. | Se encuentra correctamente porque la lista principal conserva el orden por código. |
| 13 | Pulsar Me gusta +1 sin selección. | Aviso; ningún contador cambia. |
| 14 | Buscar 9999 después de una búsqueda exitosa. | No encontrado; Guardar y Me gusta no afectan a la película anterior. |

Realizar los casos que mencionan los datos iniciales antes de modificar ese conjunto o en una sesión nueva.

## Trabajo en GitHub y entrega

1. Crear un repositorio único por equipo, accesible al docente.
2. Incluir commits de código desde al menos tres cuentas diferentes y al menos un aporte significativo por integrante.
3. No se exige utilizar ramas ni pull requests. Pueden trabajar con el procedimiento de carga que aprendieron en clase, conservando la autoría de cada aporte.
4. Entregar enlace al repositorio, código integrado, capturas de las tres pestañas y evidencia de los casos de comprobación.
5. Completar la tabla de aportes. Cualquier integrante debe poder ejecutar el programa y explicar la colaboración del equipo.

| Integrante | Usuario GitHub | Funcionalidad desarrollada | Enlace al commit |
|---|---|---|---|
| Por completar | Por completar | Por completar | Por completar |
| Por completar | Por completar | Por completar | Por completar |
| Por completar | Por completar | Por completar | Por completar |

Verificar que GitHub muestre correctamente la autoría. Si utilizan Git local, configurar un correo asociado a la cuenta correspondiente. No compartir cuentas.

## Rúbrica sobre 10 puntos

Se mantiene la distribución del taller original adjunto.

| Criterio | Puntaje máximo | Evidencia |
|---|---|---|
| Funcionamiento | 3 | Los seis requerimientos y los casos de comprobación funcionan. Cualquier integrante ejecuta la demostración. |
| Interfaz realizada manualmente por los estudiantes | 3 | JTabbedPane, formularios, JList, botones y mensajes conectados correctamente. Cualquier integrante ejecuta y muestra la interfaz. |
| GitHub | 4 | Un repositorio, commits desde al menos tres cuentas y aportes identificables. Cualquier integrante explica cómo colaboraron. |

Las preguntas del docente verifican la comprensión y las evidencias dentro de estos criterios; no agregan un cuarto criterio de calificación.
