# Renderización Condicional

## `v-if`

La directiva `v-if` es utilizada para renderizar condicionalmente un bloque de código. El bloque de código solo será renderizado si la expresión de la directiva retorna un valor verdadero.

```html
<h1 v-if="awesome">¡Vue es asombroso!</h1>
```

También es posible añadir un "bloque _else_" con `v-else`:

```html
<h1 v-if="awesome">¡Vue es asombroso!</h1>
<h1 v-else>Oh no 😢</h1>
```

### Grupos Condicionales con `v-if` en `<template>`

Debido a que `v-if` es una directiva, debe adjuntarse a un solo elemento. Pero, ¿y si queremos cambiar más de un elemento? En este caso, podemos usar `v-if` en un elemento `<template>`, que sirve como un envoltorio invisible. El resultado final procesado no incluirá el elemento `<template>`.

```html
<template v-if="ok">
  <h1>Título</h1>
  <p>Párrafo 1</p>
  <p>Párrafo 2</p>
</template>
```

### `v-else`

Puede usar la directiva `v-else` para indicar un "bloque _else_" para `v-if`:

```html
<div v-if="Math.random() > 0.5">
  Ahora me ve
</div>
<div v-else>
  Ahora no
</div>
```

Un elemento `v-else` debe seguir inmediatamente a un elemento `v-if` o `v-else-if`, de lo contrario, no será reconocido.

### `v-else-if`

El `v-else-if`, como su nombre lo indica, sirve como "bloque _else if_" para `v-if`. También puede ser encadenado varias veces:

```html
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Si no es A/B/C
</div>
```

Similar a `v-else`, un elemento `v-else-if` debe seguir inmediatamente a un elemento `v-if` o `v-else-if`.

## `v-show`

Otra opción para mostrar un elemento condicionalmente es la directiva `v-show`. El uso es básicamente el mismo:

```html
<h1 v-show="ok">¡Hola!</h1>
```

La diferencia es que un elemento con `v-show` siempre se renderizará y permanecerá en el DOM; `v-show` simplemente alterna la propiedad CSS `display` del elemento.

`v-show` no es compatible con elementos `<template>`, ni tampoco funciona con `v-else`.

## `v-if` vs `v-show`

`v-if` es una renderización condicional “real” ya que garantiza que los eventos y componentes secundarios dentro del bloque condicional sean debidamente destruidos y creados nuevamente durante la alternancia.

`v-if` también es **_lazy_**: si la condición es falsa en la renderización inicial, no se hará nada. El bloque condicional no se procesará hasta que la condición se convierta en `true` por primera vez.

En comparación, `v-show` es mucho más simple: el elemento siempre se representa independientemente de la condición inicial, con alternancia basada en CSS.

En general, `v-if` tiene costos de alternancia más altos, mientras que `v-show` tiene costos de renderización iniciales más altos. Por lo tanto, prefiera `v-show` si necesita cambiar algo muy a menudo, y prefiera `v-if` si es poco probable que la condición cambie en el tiempo de ejecución.

## `v-if` with `v-for`

::: tip Nota
Utilizar `v-if` y `v-for` juntos **no es recomendado**. Vea la [guía de estilo](../style-guide/#avoid-v-if-with-v-for-essential) para mayor información.
:::

Cuando `v-if` y `v-for` son utilizados en el mismo elemento, `v-if` será evaluado primero. Consulte la [guía de renderización de listas](list#v-for-with-v-if) para obtener más detalles.
