# Conceptos Básicos de Componentes

## Ejemplo Base

Aquí un ejemplo de un componente Vue:

```js
// Crea una aplicación Vue
const app = Vue.createApp({})

// Definir un nuevo componente global llamado button-counter
app.component('button-counter', {
  data() {
    return {
      count: 0
    }
  },
  template: `
    <button @click="count++">
      Me ha pulsado {{ count }} veces.
    </button>`
})
```

::: info
Le estamos mostrando un ejemplo sencillo aquí, pero en una aplicación común de Vue utilizamos _Single File Components_ en vez de una plantilla de cadena. Usted puede encontrar más información sobre estos [en esta sección](single-file-component.html).
:::

Los componentes son instancias reutilizables de Vue con un nombre: en este caso, `<button-counter>`. Podemos usar este componente como un elemento personalizado dentro de una instancia raíz de Vue:

```html
<div id="components-demo">
  <button-counter></button-counter>
</div>
```

```js
app.mount('#components-demo')
```

<p class="codepen" data-height="300" data-theme-id="39028" data-default-tab="js,result" data-user="Vue" data-slug-hash="abORVEJ" data-editable="true" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Component basics">
  <span>See the Pen <a href="https://codepen.io/team/Vue/pen/abORVEJ">
  Component basics</a> by Vue (<a href="https://codepen.io/Vue">@Vue</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

Dado que los componentes son instancias reutilizables de Vue, aceptan las mismas opciones que una instancia raíz, como `data`, `computed`, `watch`, `methods`, y _hooks_ de ciclo de vida. Las únicas excepciones son algunas opciones específicas de la raíz como `el`.

## Reutilizando Componentes

Los componentes se pueden reutilizar tantas veces como se desee:

```html
<div id="components-demo">
  <button-counter></button-counter>
  <button-counter></button-counter>
  <button-counter></button-counter>
</div>
```

<p class="codepen" data-height="300" data-theme-id="39028" data-default-tab="html,result" data-user="Vue" data-slug-hash="rNVqYvM" data-editable="true" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Component basics: reusing components">
  <span>See the Pen <a href="https://codepen.io/team/Vue/pen/rNVqYvM">
  Component basics: reusing components</a> by Vue (<a href="https://codepen.io/Vue">@Vue</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

Tenga en cuenta que al hacer clic en los botones, cada uno mantiene su propio `count` por separado. Esto se debe a que cada vez que utiliza un componente, se crea una nueva **instancia** del mismo.

## Organización de Componentes

Es común que una aplicación se organice en un árbol de componentes anidados:

![Árbol de Componentes](/images/components.png)

Por ejemplo, puede tener componentes para un encabezado, una barra lateral y un área de contenido, cada uno de los cuales generalmente contiene otros componentes para enlaces de navegación, publicaciones de blog, etc.

Para usar estos componentes en _templates_, deben registrarse para que Vue los conozca. Existen dos tipos de registro de componentes: **global** y **local**. Hasta ahora, solo hemos registrado componentes globalmente, usando el método `component` de una aplicación creada:

```js
const app = Vue.createApp({})

app.component('my-component-name', {
  // ... opciones ...
})
```

Los componentes registrados globalmente se pueden usar en el _template_ de cualquier instancia de Vue raíz (`app`) creada posteriormente, e incluso dentro de todos los subcomponentes del árbol de componentes de esa instancia de Vue.

Eso es todo lo que necesita saber sobre el registro por ahora, pero una vez que haya terminado de leer esta página y se sienta cómodo con su contenido, le recomendamos volver más tarde para leer la guía completa de [Registro de Componentes](component-registration.md).

## Pasando Datos a Componentes Hijo con Props

Anteriormente, mencionamos la creación de un componente para publicaciones de blog. El problema es que ese componente no será útil a menos que pueda pasarle datos, como el título y el contenido de la publicación específica que queremos mostrar. Ahí es donde entran las _props_.

Las _props_ son atributos personalizados que usted puede registrar en un componente. Cuando se pasa un valor a un atributo _prop_, se convierte en una propiedad en esa instancia de componente. Para pasar un título a nuestro componente de publicación de blog, podemos incluirlo en la lista de _props_ que este componente acepta, usando la opción `props`:

```js
const app = Vue.createApp({})

app.component('blog-post', {
  props: ['title'],
  template: `<h4>{{ title }}</h4>`
})

app.mount('#blog-post-demo')
```

Un componente puede tener tantas props como se desee, y se puede pasar cualquier valor a cualquier _prop_ de forma predeterminada. En el _template_ anterior, verá que podemos acceder a este valor en la instancia del componente, al igual que con `data`.

Una vez que se registra un _prop_, puede pasarle datos como un atributo personalizado, de la siguiente manera:

```html
<div id="blog-post-demo" class="demo">
  <blog-post title="Mi viaje con Vue"></blog-post>
  <blog-post title="Blogging con Vue"></blog-post>
  <blog-post title="¿Por qué Vue es tan divertido?"></blog-post>
</div>
```

<p class="codepen" data-height="300" data-theme-id="39028" data-default-tab="html,result" data-user="Vue" data-slug-hash="PoqyOaX" data-editable="true" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Component basics: passing props">
  <span>See the Pen <a href="https://codepen.io/team/Vue/pen/PoqyOaX">
  Component basics: passing props</a> by Vue (<a href="https://codepen.io/Vue">@Vue</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

En una aplicación típica, sin embargo, es probable que tenga un arreglo de _posts_ en `data`:

```js
const App = {
  data() {
    return {
      posts: [
        { id: 1, title: 'Mi viaje con Vue' },
        { id: 2, title: 'Blogging con Vue' },
        { id: 3, title: '¿Por qué Vue es tan divertido?' }
      ]
    }
  }
}

const app = Vue.createApp(App)

app.component('blog-post', {
  props: ['title'],
  template: `<h4>{{ title }}</h4>`
})

app.mount('#blog-posts-demo')
```

Entonces querrá renderizar un componente para cada uno:

```html
<div id="blog-posts-demo">
  <blog-post
    v-for="post in posts"
    :key="post.id"
    :title="post.title"
  ></blog-post>
</div>
```

Arriba, verá que podemos usar `v-bind` para pasar propiedades dinámicamente. Esto es especialmente útil cuando no se conoce el contenido exacto que se va a renderizar con anticipación.

Esto es todo lo que necesita saber sobre propiedades por ahora,pero una vez que haya terminado de leer esta página y se sienta cómodo con su contenido, le recomendamos volver más tarde para leer la guía completa de [Propiedades](component-props.html).

## Escuchando Eventos de Componentes Hijo

A medida que desarrollamos nuestro componente `<blog-post>` es posible que algunas funciones requieran la comunicación hacia el componente padre. Por ejemplo, podemos decidir incluir una función de accesibilidad para ampliar el texto de las publicaciones del blog, dejando el resto de la página en su tamaño por defecto:

En el padre, podemos admitir esta función agregando una propiedad `postFontSize` en `data`:

```js
const App = {
  data() {
    return {
      posts: [
        /* ... */
      ],
      postFontSize: 1
    }
  }
}
```

La cual puede ser usada en el _template_ para controlar el tamaño de la fuente de todas las publicaciones del blog:

```html
<div id="blog-posts-events-demo">
  <div v-bind:style="{ fontSize: postFontSize + 'em' }">
    <blog-post v-for="post in posts" :key="post.id" :title="title"></blog-post>
  </div>
</div>
```

Ahora agreguemos un botón para ampliar el texto justo antes del contenido de cada publicación:

```js
app.component('blog-post', {
  props: ['title'],
  template: `
    <div class="blog-post">
      <h4>{{ title }}</h4>
      <button>
        Enlarge text
      </button>
    </div>
  `
})
```

El problema es que este botón no hace nada:

```html
<button>
  Aumentar Tamaño del Texto
</button>
```

Cuando hacemos clic en el botón, debemos comunicar al componente padre que debe agrandar el texto de todas las publicaciones. Afortunadamente, las instancias de Vue proporcionan un sistema de eventos personalizados para resolver este problema. El padre puede elegir escuchar a cualquier evento en el componente hijo usando `v-on` o `@`, así como haríamos con un evento nativo del DOM:

```html
<blog-post ... @enlarge-text="postFontSize += 0.1"></blog-post>
```

Luego, el componente hijo puede emitir un evento en sí mismo llamando al [método **`$emit`**](../api/instance-methods.html#emit), pasando el nombre del evento:

```html
<button @click="$emit('enlarge-text')">
  Aumentar Tamaño del texto
</button>
```

Gracias al _listener_ `@enlarge-text="postFontSize += 0.1"`, el padre recibirá el evento y actualizará el valor de `postFontSize`.

<p class="codepen" data-height="300" data-theme-id="39028" data-default-tab="html,result" data-user="Vue" data-slug-hash="KKpGyrp" data-editable="true" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Component basics: emitting events">
  <span>See the Pen <a href="https://codepen.io/team/Vue/pen/KKpGyrp">
  Component basics: emitting events</a> by Vue (<a href="https://codepen.io/Vue">@Vue</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

Podemos listar los eventos emitidos en la opción `emits` del componente.

```js
app.component('blog-post', {
  props: ['title'],
  emits: ['enlarge-text']
})
```

Esto le permitirá ver todos los eventos que puede emitir un componente y, opcionalmente, [validarlos](component-custom-events.html#validate-emitted-events)

### Emitiendo un Valor con un Evento

A veces es útil emitir un valor específico con un evento. Por ejemplo, podemos querer que el componente `<blog-post>` se encargue de cuánto agrandar el texto. En esos casos, podemos usar el segundo parámetro de `$emit` para proporcionar este valor:

```html
<button @click="$emit('enlarge-text', 0.1)">
  Aumentar Tamaño del texto
</button>
```

Luego, cuando escuchamos el evento en el componente padre, podemos acceder al valor del evento emitido con `$event`:

```html
<blog-post ... @enlarge-text="postFontSize += $event"></blog-post>
```

O, si el controlador de eventos es un método:

```html
<blog-post ... @enlarge-text="onEnlargeText"></blog-post>
```

Entonces el valor se pasará como el primer parámetro de ese método:

```js
methods: {
  onEnlargeText(enlargeAmount) {
    this.postFontSize += enlargeAmount
  }
}
```

### Usando `v-model` en Componentes

Los eventos personalizados también se pueden usar para crear _inputs_ personalizados que funcionan con `v-model`. Recuerde que:

```html
<input v-model="searchText" />
```

hace lo mismo que:

```html
<input :value="searchText" @input="searchText = $event.target.value" />
```

Cuando se usa en un componente, `v-model`, en su lugar, hace esto:

```html
<custom-input
  :model-value="searchText"
  @update:model-value="searchText = $event"
></custom-input>
```

::: warning
Por favor, note que aquí usamos `model-value` con _kebab-case_ porque estamos trabajando con un _template_ dentro del DOM. Puede encontrar una explicación detallada de atributos _kebab-cased_ vs _camelCased_ en la sección de [Casos Especiales de Análisis de Plantillas DOM](#dom-template-parsing-caveats)
:::

Para que esto realmente funcione, el `<input>` dentro del componente debe:

- Enlazar el atributo `value` a una propiedad `modelValue`
- En el `input`, emitir un evento `update:modelValue` con el nuevo valor

Aquí está en acción:

```js
app.component('custom-input', {
  props: ['modelValue'],
  template: `
    <input
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
    >
  `
})
```

Ahora `v-model` debería trabajar perfectamente con este componente:

```html
<custom-input v-model="searchText"></custom-input>
```

Otra manera de crear la capacidad del `v-model` dentro de un componente personalizado es usar la capacidad de propiedades `computed` para definir un _getter_ y _setter_.

En el siguiente ejemplo, refactorizamos el componente `custom-input` usando una propiedad computada.

Tenga en cuenta que, el método `get` debería retornar la propiedad `modelValue`, o cualquiera que sea la propiedad que se está usando para enlazar, y el método `set` debería disparar el correspondiente `$emit` para esa propiedad.

```js
app.component('custom-input', {
  props: ['modelValue'],
  template: `
    <input v-model="value">
  `,
  computed: {
    value: {
      get() {
        return this.modelValue
      },
      set(value) {
        this.$emit('update:modelValue', value)
      }
    }
  }
})
```

Por ahora, eso es todo lo que necesita saber sobre los eventos de componentes personalizados, pero una vez que haya terminado de leer esta página y se sienta cómodo con su contenido, le recomendamos volver más tarde para leer la guía completa sobre [Eventos Personalizados](component-custom-events.md).

## Distribución de Contenido con Slots

Al igual que con los elementos HTML, a menudo es útil poder pasar contenido a un componente, como este:

```html
<alert-box>
  Algo salió mal.
</alert-box>
```

Lo que podría renderizar algo como:

<p class="codepen" data-height="300" data-theme-id="39028" data-default-tab="result" data-user="Vue" data-slug-hash="jOPeaob" data-editable="true" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Component basics: slots">
  <span>See the Pen <a href="https://codepen.io/team/Vue/pen/jOPeaob">
  Component basics: slots</a> by Vue (<a href="https://codepen.io/Vue">@Vue</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

Afortunadamente, esta tarea se hace muy simple con el elemento personalizado `<slot>`:

```js
app.component('alert-box', {
  template: `
    <div class="demo-alert-box">
      <strong>¡Error!</strong>
      <slot></slot>
    </div>
  `
})
```

Como verá más arriba, solo agregamos la ranura a la que queremos que el contenido vaya y eso es todo. ¡Hemos terminado!

Eso es todo lo que necesita saber acerca de slots por ahora, pero una vez que haya terminado de leer esta página y se sienta cómodo con su contenido, le recomendamos que regrese más tarde para leer la guía completa de [Slots](component-slots.md).

## Componentes Dinámicos

A veces, es útil cambiar dinámicamente entre componentes, como en una interfaz con pestañas:

<p class="codepen" data-height="300" data-theme-id="39028" data-default-tab="result" data-user="Vue" data-slug-hash="oNXaoKy" data-editable="true" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Component basics: dynamic components">
  <span>See the Pen <a href="https://codepen.io/team/Vue/pen/oNXaoKy">
  Component basics: dynamic components</a> by Vue (<a href="https://codepen.io/Vue">@Vue</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

Lo anterior es posible gracias al elemento `<component>` de Vue, con el atributo especial `is`:

```html
<!-- El componente cambia cuando currentTabComponent cambia -->
<component :is="currentTabComponent"></component>
```

En el ejemplo anterior, `currentTabComponent` puede contener:

- el nombre de un componente registrado, o
- el objeto de opciones de un componente

Vea [este sandbox](https://codepen.io/team/Vue/pen/oNXaoKy) para experimentar con el código completo, o [esta versión](https://codepen.io/team/Vue/pen/oNXapXM) para un ejemplo de enlace o _binding_ al objeto de opciones de un componente, en lugar de su nombre registrado.

Tenga en cuenta que este atributo puede ser usado con elementos comunes de HTML, sin embargo, serán tratados como componentes, lo que significa que todos los atributos **estarán enlazados como atributos del DOM**. Para que algunas propiedades funcionen como lo esperaría, como `value`, tendrá que vincularlas usando el [modificador `.prop`](../api/directives.html#v-bind).

Eso es todo lo que necesita saber sobre los componentes dinámicos por ahora, pero una vez que haya terminado de leer esta página y se sienta cómodo con su contenido, le recomendamos volver más tarde para leer la guía completa sobre [Componentes Dinámicos & Asíncronos](./component-dynamic-async.html).

## Casos Especiales de Análisis de Plantillas DOM.

Algunos elementos HTML, como `<ul>`, `<ol>`, `<table>` y `<select>` tienen restricciones en cuáles elemento pueden aparecer dentro de ellos, y algunos elementos como `<li>`, `<tr>`, y `<option>` solo pueden aparecer dentro de otros elementos determinados.

Esto conducirá a problemas cuando se utilizan componentes con elementos que tienen tales restricciones. Por ejemplo:

```html
<table>
  <blog-post-row></blog-post-row>
</table>
```

El componente personalizado `<blog-post-row>` se colocará como contenido no válido, lo que provocará errores en el resultado final. Afortunadamente, podemos usar el atributo especial `v-is` como una solución alternativa:

```html
<table>
  <tr v-is="'blog-post-row'"></tr>
</table>
```

:::warning
El valor de `v-is` debería ser una cadena de caracteres literal de JavaScript:

```html
<!-- Incorrecto, nada será renderizado -->
<tr v-is="blog-post-row"></tr>

<!-- Correcto -->
<tr v-is="'blog-post-row'"></tr>
```

:::

Además, los nombres de los atributos en HTML no distinguen entre mayúsculas y minúsculas, por lo que los navegadores web interpretarán cualquier caracter en mayúscula como uno en minúscula. Eso quiere decir que cuando esté usando _in-DOM templates_, los nombres de propiedades _camelCased_ y los parámetros de manejadores de eventos necesitan usar sus equivalentes en _kebab-cased_ (delimitados por "-"):

```js
// camelCase en JavaScript

app.component('blog-post', {
  props: ['postTitle'],
  template: `
    <h3>{{ postTitle }}</h3>
  `
})
```

```html
<!-- kebab-case en HTML -->

<blog-post post-title="¡Hola!"></blog-post>
```

Debería notarse que **estas limitaciones _no_ aplican si está usando plantillas de cadenas de texto de una de las siguientes fuentes**:

- Plantillas de cadenas de texto (por ejemplo. `template: '...'`)
- [Componentes de un solo archivo (`.vue`)](single-file-component.html)
- `<script type="text/x-template">`

Eso es todo lo que necesita saber sobre los casos especiales de análisis de plantillas DOM por ahora, y en realidad, el final de los aspectos _esenciales_ de Vue. ¡Felicidades! Todavía hay más que aprender, pero primero, recomendamos tomar un descanso para practicar con Vue usted mismo y construir algo divertido.

Una vez que se sienta cómodo con el conocimiento que acaba de digerir, le recomendamos que regrese para leer la guía completa de [Componentes Dinámicos & Asíncronos](component-dynamic-async.html), así como las otras páginas en la sección Componentes en Profundidad de la barra lateral.
