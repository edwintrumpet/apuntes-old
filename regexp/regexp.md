# Expresiones regulares
### Regex - Regexp

## Índice
- [Introducción](#introduccion)
- [Clases (modelos)](#clases)
    - [Negación](#negacion)
    - [Clases predefinidas](#clases-predefinidas)
- [Caracteres especiales](#caracteres-especiales)

<a name="introduccion" />

## Introducción

Las expresiones regulares nos sirven para representar modelos de patrones que coincidan con textos que sean de nuestro interés.

Con estos patrones podemos realizar búsquedas o filtrar información para solo recibir los datos que nos interesan o solo aceptar un formato válido en la presentación de dicha información.

<a name="clases" />

## Clases (modelos)

Se pueden representar caracteres o conjuntos de caracteres por medio de expresiones envueltas en corchetes "[ ]". Estas expresiones pueden contener literalmente los caracteres con los que hará coincidencia la clase, por ejemplo:

`[aym]` coincide con las letras **_a_**, **_y_** y **_m_**.

También pueden contener rangos de caracteres como por ejemplo:

- `[0-6]` Coincide con los dígitos del 0 al 6.
- `[a-d]` Coincide con las letra minúsculas desde la **_a_** hasta la **_d_** sin acentos.

<a name="negacion" />

### Negación

Cuando lo que deseamos especificar no es el carácter o conjunto de caracteres a representar sino todo lo contrario (los caracteres que no deseamos aceptar), entonces usamos el carácter `^` al comienzo de la clase así:

- `[^dgy]` Coincide con cualquier carácter que no sea **_d_**, **_g_** o **_y_**.
- `[0-9]` Coincide con cualquier carácter que no sea un dígito.

<a name="clases-predefinidas" />

### Clases predefinidas

Existen algunas expresiones ya definidas que coinciden con un carácter o un grupo de ellos, algunas de estas son:

- `.` = `[^n]` Cualquier carácter (no incluye el salto de línea).
- `\w` = `[a-zA-Z0-9_]` Números, letras y guión bajo (no incluye acentos ni la ñ).
- `\d` = `[0-9]` Dígitos.
- `\s` Espacios, tabulaciones y saltos de línea.
- `\t` Tabulaciones.
- `\n` Saltos de línea.
- `W` = `[^a-zA-Z0-9_]` Cualquier carácter que no sea alfanumérico.
- `\D` = `[^0-9]` Cualquier carácter que no sea un dígito.
- `\S` Cualquier carácter que no sea un espacio, una tabulación o un salto de línea.

<a name="caracteres-especiales" />

## Caracteres especiales

Algunos caracteres como los corchetes que se usan para envolver los modelos son caracteres con significados especiales que si se quisieran representar en un modelo podrían ocasionar errores, por lo que para su uso como carácter debe anteponerse el símbolo `\` que en sí mismo también es un carácter especial con la función de escapar los caracteres que poseen significados especiales o darle significado a otros que no lo poseen.

Los carácteres especiales son: `.`, `\`, `/`, `^`, `[`, `]`, `(`, `)`, `-`, `+`, `*`, `?`, `{`, `}`, `$`, `|`, `<`, `>`.

Por ejemplo si queremos incluir el carácter `\` y los dígitos en una clase lo usaríamos así `[0-9\\]`. Y si quisieramos incluir las letras mayúsculas y minúsculas sin acentos y los paréntesis usaríamos `[a-zA-Z\(\)]`