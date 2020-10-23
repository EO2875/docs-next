# Renderizado de Lista

## Mapeando una matriz a Elementos con `v-for`

Podemos usar la directiva `v-for` para renderizar una lista de elementos basada en una matriz. La directiva `v-for` requiere una sintaxis especial en forma de `item in items`, donde los `items` son la matriz de datos de origen y el `item` es un **alias** para el elemento de matriz que se está iterando:

```html
<ul id="array-rendering">
  <li v-for="item in items">
    {{ item.message }}
  </li>
</ul>
```

```js
Vue.createApp({
  data() {
    return {
      items: [{ message: 'Foo' }, { message: 'Bar' }]
    }
  }
}).mount('#array-rendering')
```

Resultado:

<p class="codepen" data-height="300" data-theme-id="39028" data-default-tab="js,result" data-user="Vue" data-slug-hash="VwLGbwa" data-editable="true" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="v-for with Array">
  <span>See the Pen <a href="https://codepen.io/team/Vue/pen/VwLGbwa">
  v-for with Array</a> by Vue (<a href="https://codepen.io/Vue">@Vue</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

Dentro de los bloques `v-for` tenemos acceso completo a las propiedades del ámbito principal. `v-for` también admite un segundo argumento opcional para el índice del elemento actual.

```html
<ul id="array-with-index">
  <li v-for="(item, index) in items">
    {{ parentMessage }} - {{ index }} - {{ item.message }}
  </li>
</ul>
```

```js
Vue.createApp({
  data() {
    return {
      parentMessage: 'Padre',
      items: [{ message: 'Foo' }, { message: 'Bar' }]
    }
  }
}).mount('#array-with-index')
```

Resultado:

<p class="codepen" data-height="300" data-theme-id="39028" data-default-tab="js,result" data-user="Vue" data-slug-hash="wvaEdBP" data-editable="true" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="v-for with Array and index">
  <span>See the Pen <a href="https://codepen.io/team/Vue/pen/wvaEdBP">
  v-for with Array and index</a> by Vue (<a href="https://codepen.io/Vue">@Vue</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

También puede usar `of` como delimitador en lugar de `in`, de modo que esté más cerca de la sintaxis de JavaScript para los iteradores:

```html
<div v-for="item of items"></div>
```

## `v-for` con un Objeto

También puede usar `v-for` para iterar a través de las propiedades de un objeto.

```html
<ul id="v-for-object" class="demo">
  <li v-for="value in myObject">
    {{ value }}
  </li>
</ul>
```

```js
Vue.createApp({
  data() {
    return {
      myObject: {
        title: 'Cómo hacer listas en Vue',
        author: 'Jane Doe',
        publishedAt: '2016-04-10'
      }
    }
  }
}).mount('#v-for-object')
```

Resultado:

<p class="codepen" data-height="300" data-theme-id="39028" data-default-tab="js,result" data-user="Vue" data-slug-hash="NWqLjqy" data-editable="true" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="v-for with Object">
  <span>See the Pen <a href="https://codepen.io/team/Vue/pen/NWqLjqy">
  v-for with Object</a> by Vue (<a href="https://codepen.io/Vue">@Vue</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

También puede proporcionar un segundo argumento para el nombre de la propiedad (también conocida como clave):

```html
<li v-for="(value, name) in myObject">
  {{ name }}: {{ value }}
</li>
```

<p class="codepen" data-height="300" data-theme-id="39028" data-default-tab="js,result" data-user="Vue" data-slug-hash="poJOPjx" data-editable="true" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="v-for with Object and key">
  <span>See the Pen <a href="https://codepen.io/team/Vue/pen/poJOPjx">
  v-for with Object and key</a> by Vue (<a href="https://codepen.io/Vue">@Vue</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

Y otro para el índice:

```html
<li v-for="(value, name, index) in myObject">
  {{ index }}. {{ name }}: {{ value }}
</li>
```

<p class="codepen" data-height="300" data-theme-id="39028" data-default-tab="js,result" data-user="Vue" data-slug-hash="abOaWdo" data-editable="true" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="v-for with Object key and index">
  <span>See the Pen <a href="https://codepen.io/team/Vue/pen/abOaWdo">
  v-for with Object key and index</a> by Vue (<a href="https://codepen.io/Vue">@Vue</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

:::tip Nota
Al iterar sobre un objeto, el orden se basa en el orden de enumeración de claves de `Object.keys()`, que no se garantiza que sea consistente en todas las implementaciones del motor de JavaScript.
:::

## Manteniendo el Estado

Cuando Vue está actualizando una lista de elementos renderizados con `v-for`, por defecto utiliza una estrategia de “parche _in situ_”. Si el orden de los elementos de datos ha cambiado, en lugar de mover los elementos DOM para que coincidan con el orden de los elementos, Vue aplicará parches a cada elemento en el lugar y se asegurará de que refleje lo que se debe representar en ese índice en particular.

Este modo predeterminado es eficiente, pero **solo es adecuado cuando la salida de su lista renderizada no depende del estado del componente hijo o del estado temporal del DOM (por ejemplo, valores de entrada de formulario)**.

Para proporcionar a Vue una sugerencia para que pueda rastrear la identidad de cada nodo y, por lo tanto, reutilizar y reordenar los elementos existentes, debe proporcionar un atributo `key` único para cada elemento:

```html
<div v-for="item in items" :key="item.id">
  <!-- contenido -->
</div>
```

Se recomienda proporcionar un atributo `key` con `v-for` siempre que sea posible, a menos que el contenido DOM iterado sea simple, o esté confiando intencionalmente en el comportamiento predeterminado para obtener ganancias en el rendimiento.

Como identificar nodos es un mecanismo genérico de Vue, `key` también tiene otros usos que no están específicamente vinculados a `v-for`, como veremos más adelante en la guía.

:::tip Nota
No utilice valores no primitivos como objetos y vectores como clave para `v-for`. Utilice _string_ o valores numéricos en su lugar.
:::

Para ver el uso en detalle del atributo `key`, por favor vea la [documentación de la API para `key`](../api/special-attributes.html#key).

## Detección del cambios en Array

### Métodos de Mutación

Vue envuelve los métodos de mutación de una matriz observada para que también activen las actualizaciones de vista. Los métodos envueltos son:

- `push()`
- `pop()`
- `shift()`
- `unshift()`
- `splice()`
- `sort()`
- `reverse()`

Puede abrir la consola y probar con la matriz de `items` de los ejemplos anteriores llamando a sus métodos de mutación. Por ejemplo: `example1.items.push({ message: 'Baz' })`.

### Mutando un Array

Los métodos de mutación, como sugiere su nombre, mutan la matriz original a la que se llama. En comparación, también hay métodos no mutantes, por ejemplo `filter()`, `concat()` y `slice()`, que no mutan la matriz original sino que **siempre devuelven una nueva matriz**. Cuando trabaje con métodos no mutantes, puede reemplazar la matriz anterior por la nueva:

```js
example1.items = example1.items.filter(item => item.message.match(/Foo/))
```

Podría pensar que esto hará que Vue elimine el DOM existente y vuelva a renderizar la lista completa; afortunadamente, ese no es el caso. Vue implementa algunas heurísticas inteligentes para maximizar la reutilización de elementos DOM, por lo tanto, reemplazar una matriz con otra matriz que contenga objetos superpuestos es una operación muy eficiente.

## Mostrando Resultados Filtrados/Ordenados

A veces, queremos mostrar una versión filtrada u ordenada de una matriz sin mutar o restablecer los datos originales. En este caso, puede crear una propiedad computada que devuelva la matriz filtrada o ordenada.

Por ejemplo:

```html
<li v-for="n in evenNumbers">{{ n }}</li>
```

```js
data() {
  return {
    numbers: [ 1, 2, 3, 4, 5 ]
  }
},
computed: {
  evenNumbers() {
    return this.numbers.filter(number => number % 2 === 0)
  }
}
```

En situaciones donde las propiedades computadas no son factibles (por ejemplo, dentro de los bucles `v-for` anidados), puede usar un método:

```html
<ul v-for="numbers in sets">
  <li v-for="n in even(numbers)">{{ n }}</li>
</ul>
```

```js
data() {
  return {
    sets: [[ 1, 2, 3, 4, 5 ], [6, 7, 8, 9, 10]]
  }
},
methods: {
  even(numbers) {
    return numbers.filter(number => number % 2 === 0)
  }
}
```

## `v-for` con un Rango

`v-for` también puede tomar un entero. En este caso repetirá la plantilla muchas veces.

```html
<div id="range" class="demo">
  <span v-for="n in 10">{{ n }} </span>
</div>
```

Resultado:

<p class="codepen" data-height="300" data-theme-id="39028" data-default-tab="html,result" data-user="Vue" data-slug-hash="NWqLjNY" data-editable="true" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="v-for with a range">
  <span>See the Pen <a href="https://codepen.io/team/Vue/pen/NWqLjNY">
  v-for with a range</a> by Vue (<a href="https://codepen.io/Vue">@Vue</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## `v-for` en un `<template>`

De forma similar a la plantilla `v-if`, también puede usar una etiqueta `<template>` con `v-for` para renderizar un bloque de varios elementos. Por ejemplo:

```html
<ul>
  <template v-for="item in items">
    <li>{{ item.msg }}</li>
    <li class="divider" role="presentation"></li>
  </template>
</ul>
```

## `v-for` con `v-if`

:::tip
Tenga en cuenta que **no se recomienda** usar `v-if` y `v-for` juntos. Consulte la [guía de estilo](../style-guide/#avoid-v-if-with-v-for-essential) para más detalles.
:::

Cuando existen en el mismo nodo, `v-if` tiene una prioridad más alta que `v-for`. Eso significa que la condición `v-if` no tendrá acceso a las variables que se encuentren solo dentro del alcance del `v-for`:

```html
<!-- Esto mostrará un error porque la propiedad "todo" no está definida en la instancia. -->

<li v-for="todo in todos" v-if="!todo.isComplete">
  {{ todo }}
</li>
```

Esto se puede solucionar envolviendo `v-for` dentro de una etiqueta `<template>`:

```html
<template v-for="todo in todos">
  <li v-if="!todo.isComplete">
    {{ todo }}
  </li>
</template>
```

## `v-for` con un Componente

> Esta sección asume el conocimiento de [Componentes](component-basics.md). Siéntase libre de saltearlo y volver más tarde.

Puede usar `v-for` directamente en un componente personalizado, como cualquier elemento normal:

```html
<my-component v-for="item in items" :key="item.id"></my-component>
```

Sin embargo, esto no pasará automáticamente ningún dato al componente, porque los componentes tienen sus propios ámbitos aislados. Para pasar los datos iterados al componente, también debemos usar props:

```html
<my-component
  v-for="(item, index) in items"
  :item="item"
  :index="index"
  :key="item.id"
></my-component>
```

La razón para no inyectar automáticamente el `item` en el componente es porque hace que el componente esté estrechamente acoplado a cómo funciona `v-for`. Ser explícito acerca de dónde provienen sus datos hace que el componente sea reutilizable en otras situaciones.

Aquí hay un ejemplo completo de una lista de tareas simple:

```html
<div id="todo-list-example">
  <form v-on:submit.prevent="addNewTodo">
    <label for="new-todo">Agregar una Tarea</label>
    <input
      v-model="newTodoText"
      id="new-todo"
      placeholder="Por ejemplo, Alimentar al gato"
    />
    <button>Agregar</button>
  </form>
  <ul>
    <todo-item
      v-for="(todo, index) in todos"
      :key="todo.id"
      :title="todo.title"
      @remove="todos.splice(index, 1)"
    ></todo-item>
  </ul>
</div>
```

```js
const app = Vue.createApp({
  data() {
    return {
      newTodoText: '',
      todos: [
        {
          id: 1,
          title: 'Lavar los platos'
        },
        {
          id: 2,
          title: 'Sacar la basura'
        },
        {
          id: 3,
          title: 'Cortar el césped'
        }
      ],
      nextTodoId: 4
    }
  },
  methods: {
    addNewTodo() {
      this.todos.push({
        id: this.nextTodoId++,
        title: this.newTodoText
      })
      this.newTodoText = ''
    }
  }
})

app.component('todo-item', {
  template: `
    <li>
      {{ title }}
      <button @click="$emit('remove')">Eliminar</button>
    </li>
  `,
  props: ['title']
})

app.mount('#todo-list-example')
```

<p class="codepen" data-height="300" data-theme-id="39028" data-default-tab="js,result" data-user="Vue" data-slug-hash="abOaWpz" data-editable="true" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="v-for with components">
  <span>See the Pen <a href="https://codepen.io/team/Vue/pen/abOaWpz">
  v-for with components</a> by Vue (<a href="https://codepen.io/Vue">@Vue</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>
