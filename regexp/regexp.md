# Expresiones regulares
### Regex - Regexp

## Índice
- [Introducción](#introduccion)
- [Clases (modelos)](#clases)
    - [Negación](#negacion)

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