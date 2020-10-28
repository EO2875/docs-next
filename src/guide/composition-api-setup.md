# Setup

> Esta sección utiliza [componentes de un solo archivo](single-file-component.html) para ejemplos

> Esta guía asume que usted ya leyo la [Introducción de la API de Composición](composition-api-introduction.html) y [Fundamentos de la Reactividad](reactivity-fundamentals.html). Lea esto antes si usted es nuevo en lo que refiere a la API de Composición.

## Argumentos

La función `setup` recibe dos argumentos:

1. `props`
2. `context`

Veamos en profundidad como se puede utilizar cada argumento.

### Props

El primer argumento de la función `setup` es el argumento `props`. Tal y como usted esperaría en un componente normal, `props` dentro de la función `setup` son reactivas y serán actualizadas cuando nuevas _props_ sean pasadas.

```js
// MyBook.vue

export default {
  props: {
    title: String
  },
  setup(props) {
    console.log(props.title)
  }
}
```

:::warning
Sin embargo, dado que las `props` son reactivas, usted **no puede utilizar desestructuración de ES6** porque esto removera la reactividad de las mismas.
:::

Si usted necesita destrucutrar sus _props_, puede hacerlo de forma segura utilizando [toRefs](reactivity-fundamentals.html#destructuring-reactive-state) dentro de su función `setup`.

```js
// MyBook.vue

import { toRefs } from 'vue'

setup(props) {
	const { title } = toRefs(props)

	console.log(title.value)
}
```

### Context

El segundo argumento de la función `setup` es `context`. El argumento `context` es un objeto JavaScript común y corriente que expone tres propiedades de componente:

```js
// MyBook.vue

export default {
  setup(props, context) {
    // Atributos (objeto no reactivo)
    console.log(context.attrs)

    // Slots (objeto no reactivo)
    console.log(context.slots)

    // Eventos Emit (Método)
    console.log(context.emit)
  }
}
```

El objeto `context` es un objeto JavaScript común y corriento, es decir, no es reactivo, esto quiere decir que puede utilizar desestructuración de ES6 de forma segura sobre `context`.

```js
// MyBook.vue
export default {
  setup(props, { attrs, slots, emit }) {
    ...
  }
}
```

`attrs` y `slots` son objetos que siempre son actualizados cuando la componente es actualizada. Esto quiere decir que usted debe evitar destructurarlos y siempre referenciar sus propiedades como `attrs.x` o `slots.x`. También tenga que cuenta que, a diferencia de `props`, `attrs` y `slots` **no** son reactivos. Si usted se ve tentado a invocar efectos secundarios basados en cambios en `attrs` o `slots`, usted debería realizarlo dentro del _hook_ del ciclo de vida `onUpdated`.

## Accediendo a Propiedades del Componente

Cuando `setup` es ejecutada, la instancia del componente aún no ha sido creada. Como resultado, usted solo podrá acceder a las siguientes propiedades:

- `props`
- `attrs`
- `slots`
- `emit`

En otras palabras, usted **no tendrá acceso** a las siguientes opciones del componente:

- `data`
- `computed`
- `methods`

## Uso con _Templates_

Si `setup` retorna un objeto, las propiedad de dicho objeto pueden ser accedidas desde el _template_ del componente, así como las propiedades de `props` pasadas a la función `setup`:

```vue-html
<!-- MyBook.vue -->
<template>
  <div>{{ collectionName }}: {{ readersNumber }} {{ book.title }}</div>
</template>

<script>
  import { ref, reactive } from 'vue'

  export default {
    props: {
      collectionName: String
    },
    setup(props) {
      const readersNumber = ref(0)
      const book = reactive({ title: 'Vue 3 Guide' })

      // exponer al template
      return {
        readersNumber,
        book
      }
    }
  }
</script>
```

Note que [refs](../api/refs-api.html#ref) retornadas desde `setup` son [desenvueltas automáticamente](/guide/reactivity-fundamentals.html#ref-unwrapping) cuando son accedidas en el _template_ así usted no debe utilizar `.value` en los mismos.

## Uso con _Render Functions_

`setup` también puede retornar una _render function_, la cual puede directamente hacer uso del estado reactivo declarado anteriormente:

```js
// MyBook.vue

import { h, ref, reactive } from 'vue'

export default {
  setup() {
    const readersNumber = ref(0)
    const book = reactive({ title: 'Vue 3 Guide' })
    // Por favor note que aquí debemos exponer explícitamente el valor
    return () => h('div', [readersNumber.value, book.title])
  }
}
```

## Uso de `this`

**Dentro de `setup()`, `this` no será una referencia a la instancia activa** Dado que `setup()` es llamada antes de que las opciones del componente sean resueltas, `this` dentro de `setup()` se comportará diferente que `this` en otra opción. Esto puede causar confusiones al utilizar `setup()` junto con la API de opciones.
