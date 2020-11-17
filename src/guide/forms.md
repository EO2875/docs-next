# Binding en Formularios

## Uso Básico

Puede usar la directiva `v-model` para crear _bindings_ de datos bidireccionales (_two-way data bindings_) en elementos _input_, _textarea_, y _select_ de un formulario. La directiva busca automáticamente la manera correcta de actualizar el elemento según el tipo de entrada. Aunque un poco mágico, `v-model` es esencialmente _syntax sugar_ para actualización de datos a través de eventos de entradas del usuario, además de mostrar un cuidado especial para algunos casos de borde.

::: tip Nota
`v-model` ignorará el `value` inicial, los atributos `checked` o `selected` que se encuentran en cualquier elemento de formulario. Siempre tratará los datos de la instancia de Vue como la fuente confiable. Debe declarar el valor inicial del lado de JavaScript, dentro de la opción `data` de su componente.
:::

`v-model` internamente utiliza propiedades diferentes y emite eventos diferentes para elementos de entrada distintos:

- Los elementos _text_ y _textarea_ utilizan la propiedad `value` y el evento `input`;
- Los _checkboxes_ y _radio_ utilizan la propiedad `checked` y el evento `change`;
- Los campos _select_ utilizan `value` como propiedad y `change` como evento.

<span id="vmodel-ime-tip"></span>
::: tip Nota
Para los idiomas que requieren un [IME](https://en.wikipedia.org/wiki/Input_method) (Chino, Japonés, Coreano, etc.), notará que el `v-model` no se actualiza durante la composición del IME. Si también desea atender estas actualizaciones, use un evento `input` en su lugar.
:::

### Input

```html
<input v-model="message" placeholder="edíteme" />
<p>El mensaje es: {{ message }}</p>
```

<p class="codepen" data-height="300" data-theme-id="39028" data-default-tab="result" data-user="Vue" data-slug-hash="eYNPEqj" data-editable="true" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Handling forms: basic v-model">
  <span>See the Pen <a href="https://codepen.io/team/Vue/pen/eYNPEqj">
  Handling forms: basic v-model</a> by Vue (<a href="https://codepen.io/Vue">@Vue</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

### Textarea

```html
<span>El mensaje multilínea es:</span>
<p style="white-space: pre-line;">{{ message }}</p>
<br />
<textarea v-model="message" placeholder="agregar múltiples líneas"></textarea>
```

<p class="codepen" data-height="300" data-theme-id="39028" data-default-tab="result" data-user="Vue" data-slug-hash="xxGyXaG" data-editable="true" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Handling forms: textarea">
  <span>See the Pen <a href="https://codepen.io/team/Vue/pen/xxGyXaG">
  Handling forms: textarea</a> by Vue (<a href="https://codepen.io/Vue">@Vue</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

La interpolación en textareas no funciona. En su lugar, use `v-model`.

```html
<!-- bad -->
<textarea>{{ text }}</textarea>

<!-- good -->
<textarea v-model="text"></textarea>
```

### Checkbox

_Checkbox_ simples con valor booleano:

```html
<input type="checkbox" id="checkbox" v-model="checked" />
<label for="checkbox">{{ checked }}</label>
```

<p class="codepen" data-height="300" data-theme-id="39028" data-default-tab="result" data-user="Vue" data-slug-hash="PoqyJVE" data-editable="true" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Handling forms: checkbox">
  <span>See the Pen <a href="https://codepen.io/team/Vue/pen/PoqyJVE">
  Handling forms: checkbox</a> by Vue (<a href="https://codepen.io/Vue">@Vue</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

Múltiples _checkboxes_ vinculados al mismo Arreglo:

```html
<div id="v-model-multiple-checkboxes">
  <input type="checkbox" id="jack" value="Jack" v-model="checkedNames" />
  <label for="jack">Jack</label>
  <input type="checkbox" id="john" value="John" v-model="checkedNames" />
  <label for="john">John</label>
  <input type="checkbox" id="mike" value="Mike" v-model="checkedNames" />
  <label for="mike">Mike</label>
  <br />
  <span>Nombres seleccionados: {{ checkedNames }}</span>
</div>
```

```js
Vue.createApp({
  data() {
    return {
      checkedNames: []
    }
  }
}).mount('#v-model-multiple-checkboxes')
```

<p class="codepen" data-height="300" data-theme-id="39028" data-default-tab="result" data-user="Vue" data-slug-hash="bGdmoyj" data-editable="true" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Handling forms: multiple checkboxes">
  <span>See the Pen <a href="https://codepen.io/team/Vue/pen/bGdmoyj">
  Handling forms: multiple checkboxes</a> by Vue (<a href="https://codepen.io/Vue">@Vue</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

### Radio

```html
<div id="v-model-radiobutton">
  <input type="radio" id="uno" value="Uno" v-model="picked" />
  <label for="one">Uno</label>
  <br />
  <input type="radio" id="dos" value="Dos" v-model="picked" />
  <label for="two">Dos</label>
  <br />
  <span>Eligió: {{ picked }}</span>
</div>
```

```js
Vue.createApp({
  data() {
    return {
      picked: ''
    }
  }
}).mount('#v-model-radiobutton')
```

<p class="codepen" data-height="300" data-theme-id="39028" data-default-tab="result" data-user="Vue" data-slug-hash="MWwPEMM" data-editable="true" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Handling forms: radiobutton">
  <span>See the Pen <a href="https://codepen.io/team/Vue/pen/MWwPEMM">
  Handling forms: radiobutton</a> by Vue (<a href="https://codepen.io/Vue">@Vue</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

### Select

Selección de un solo elemento:

```html
<div id="v-model-select" class="demo">
  <select v-model="selected">
    <option disabled value="">Seleccione un elemento</option>
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <span>Seleccionado: {{ selected }}</span>
</div>
```

```js
Vue.createApp({
  data() {
    return {
      selected: ''
    }
  }
}).mount('#v-model-select')
```

<p class="codepen" data-height="300" data-theme-id="39028" data-default-tab="result" data-user="Vue" data-slug-hash="KKpGydL" data-editable="true" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Handling forms: select">
  <span>See the Pen <a href="https://codepen.io/team/Vue/pen/KKpGydL">
  Handling forms: select</a> by Vue (<a href="https://codepen.io/Vue">@Vue</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

:::tip Nota
Si el valor inicial de su expresión `v-model` no coincide con ninguna de las opciones, el elemento `<select>` se representará en un estado _“unselected”_. En iOS, esto hará que el usuario no pueda seleccionar el primer elemento porque iOS no dispara un evento de cambio en este caso. Por lo tanto, se recomienda proporcionar una opción deshabilitada con un valor vacío, como se muestra en el ejemplo anterior.
:::

Selección de múltiples elementos (vinculando a un Arreglo):

```html
<select v-model="selected" multiple>
  <option>A</option>
  <option>B</option>
  <option>C</option>
</select>
<br />
<span>Seleccionados: {{ selected }}</span>
```

<p class="codepen" data-height="300" data-theme-id="39028" data-default-tab="html,result" data-user="Vue" data-slug-hash="gOpBXPz" data-editable="true" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Handling forms: select bound to array">
  <span>See the Pen <a href="https://codepen.io/team/Vue/pen/gOpBXPz">
  Handling forms: select bound to array</a> by Vue (<a href="https://codepen.io/Vue">@Vue</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

Opciones dinámicas renderizadas con `v-for`:

```html
<div id="v-model-select-dynamic" class="demo">
  <select v-model="selected">
    <option v-for="option in options" :value="option.value">
      {{ option.text }}
    </option>
  </select>
  <span>Seleccionado: {{ selected }}</span>
</div>
```

```js
Vue.createApp({
  data() {
    return {
      selected: 'A',
      options: [
        { text: 'Uno', value: 'A' },
        { text: 'Dos', value: 'B' },
        { text: 'Tres', value: 'C' }
      ]
    }
  }
}).mount('#v-model-select-dynamic')
```

<p class="codepen" data-height="300" data-theme-id="39028" data-default-tab="result" data-user="Vue" data-slug-hash="abORVZm" data-editable="true" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Handling forms: select with dynamic options">
  <span>See the Pen <a href="https://codepen.io/team/Vue/pen/abORVZm">
  Handling forms: select with dynamic options</a> by Vue (<a href="https://codepen.io/Vue">@Vue</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## Vinculando a Valores

Para _radio_, _checkbox_ y _option_ de _select_, los valores de vinculación del `v-model` suelen ser cadenas estáticas (o booleanos para _checkbox_):

```html
<!-- `picked` es una cadena "a" cuando está chequeado -->
<input type="radio" v-model="picked" value="a" />

<!-- `toggle` es verdadero o falso -->
<input type="checkbox" v-model="toggle" />

<!-- `selected` es una cadena "abc" cuando se selecciona la primera opción -->
<select v-model="selected">
  <option value="abc">ABC</option>
</select>
```

Pero a veces es posible que queramos vincular el valor a una propiedad dinámica en la instancia de Vue. Podemos usar `v-bind` para lograr eso. Además, el uso de `v-bind` nos permite vincular el valor de entrada a valores que no son cadenas.

### Checkbox

```html
<input type="checkbox" v-model="toggle" true-value="yes" false-value="no" />
```

```js
// cuando está marcado:
vm.toggle === 'yes'
// cuando está desmarcado:
vm.toggle === 'no'
```

:::tip Tip
Los atributos de `true-value` y de `false-value` no afectan la entrada del atributo `value`, ya que los navegadores no incluyen casillas sin marcar en los envíos de formularios. Para garantizar que uno de los dos valores se envie en un formulario (por ejemplo, “yes” o “no”), use entradas de radio en su lugar.
:::

### Radio

```html
<input type="radio" v-model="pick" v-bind:value="a" />
```

```js
// cuando está marcado:
vm.pick === vm.a
```

### Select

```html
<select v-model="selected">
  <!-- objeto literal en línea -->
  <option :value="{ number: 123 }">123</option>
</select>
```

```js
// cuando está seleccionado:
typeof vm.selected // => 'object'
vm.selected.number // => 123
```

## Modificadores

### `.lazy`

De forma predeterminada, `v-model` sincroniza el `input` con los datos después de cada evento de tipo `input` (con la excepción de la composición IME [descrito anteriormente](#vmodel-ime-tip)). En lugar de esto, puede agregar el modificador `lazy` para realizar la sincronización después del evento `change`:

```html
<!-- sincronizado después de "change" en lugar de "input" -->
<input v-model.lazy="msg" />
```

### `.number`

Si desea que las entradas del usuario se escriban automáticamente como un número, puede agregar el modificador `number` al `v-model` del elemento:

```html
<input v-model.number="age" type="number" />
```

Esto suele ser útil, porque incluso con `type="number"`, el valor retornado por el elemento HTML siempre es una cadena de texto. Si el valor no se puede analizar con `parseFloat()`, se retorna el valor original.

### `.trim`

Si desea que las entradas del usuario se recorten automáticamente, puede agregar el modificador `trim` al `v-model` de su elemento:

```html
<input v-model.trim="msg" />
```

## `v-model` con Componentes

> Si aún no está familiarizado con los componentes de Vue, puede omitir esto por ahora.

Los tipos de _input_ nativos de HTML no siempre satisfarán sus necesidades. Afortunadamente, los componentes de Vue le permiten crear _inputs_ reutilizables con un comportamiento completamente personalizado. ¡Estos componentes también funcionan con `v-model`! Para aprender más, lea acerca de [_inputs_ personalizados](./component-basics.html#using-v-model-on-components) en la guía de Componentes.
