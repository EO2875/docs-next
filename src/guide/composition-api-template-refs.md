## Template Refs

> Esta sección utiliza [componentes de un solo archivo](single-file-component.html) para ejemplos

> Esta guía asume que usted ya leyo la [Introducción de la API de Composición](composition-api-introduction.html) y [Fundamentos de la Reactividad](reactivity-fundamentals.html). Lea esto antes si usted es nuevo en lo que refiere a la API de Composición.

Cuando se utiliza la API de Composición, los conceptos de [refs reactivas](reactivity-fundamentals.html#creating-standalone-reactive-values-as-refs) y [template refs](component-template-refs.html) son el mismo. Para obtener la referencia de un elemento que se encuentra dentro del _template_ o la instancia de un componente, podemos declarar una _ref_ como lo hacemos usualmente y retornarla desde [setup()](composition-api-setup.html).

```html
<template>
  <div ref="root">Este es un elemento raíz</div>
</template>

<script>
  import { ref, onMounted } from 'vue'

  export default {
    setup() {
      const root = ref(null)

      onMounted(() => {
        // El elemento del DOM será asignado a la ref luego del renderizado inicial
        console.log(root.value) // <div>Este es un elemento raíz</div>
      })

      return {
        root
      }
    }
  }
</script>
```

Aquí estamos exponiendo `root` en el _render context_ y vinculándolo a la ref del div utilizando `ref="root"`. En el algoritmo de _patch_ del DOM Virtual, si la `ref` de un VNode corresponde a una `ref` en el _render context_, el VNode correspondiente a dicho elemento o instancia de componente  será asignado al valor de dicha ref. Esto se realiza durante el proceso de montado o de _patch_ del DOM Virtual, por lo cual _template refs_ solo tendrán valores asignados luego del renderizado inicial.

Refs utilizadas como _template refs_ se comportan igual que cualquier otras refs: son reactivas y puede ser pasadas a (y retornadas de) funciones de composición.

### Uso con JSX

```js
export default {
  setup() {
    const root = ref(null)

    return () =>
      h('div', {
        ref: root
      })

    // with JSX
    return () => <div ref={root} />
  }
}
```

### Uso dentro de`v-for`

Los _template refs_ de la API de Composición no necesitan un manejo especial cuando son utilizados dentro de `v-for`. En su lugar, use _function refs_ para realizar un manejo manual:

```html
<template>
  <div v-for="(item, i) in list" :ref="el => { if (el) divs[i] = el }">
    {{ item }}
  </div>
</template>

<script>
  import { ref, reactive, onBeforeUpdate } from 'vue'

  export default {
    setup() {
      const list = reactive([1, 2, 3])
      const divs = ref([])

      // asegurese de resetear todas las refs antes de cada update
      onBeforeUpdate(() => {
        divs.value = []
      })

      return {
        list,
        divs
      }
    }
  }
</script>
```
