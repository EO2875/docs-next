# Propiedades de Data y Métodos

## Propiedades de Data

La opción `data` de un componente es una función. Vue llama a esta función como parte del proceso de construcción de una nueva instancia de un componente. Esta función debería devolver un objeto que Vue envolverá en su sistema de reactividad y almacenará en la instancia del componente como `$data`. Por conveniencia, cualquier propiedad en _top-level_ de ese objeto, también se expone directamente a través de la instancia del componente:

```js
const app = Vue.createApp({
  data() {
    return { count: 4 }
  }
})

const vm = app.mount('#app')

console.log(vm.$data.count) // => 4
console.log(vm.count)       // => 4

// Asignando un valor a vm.count también actualizará $data.count
vm.count = 5
console.log(vm.$data.count) // => 5

// ... y viceversa
vm.$data.count = 6
console.log(vm.count) // => 6
```

Estas propiedades de la instancia solo se agregan cuando la instancia se crea por primera vez, por lo que debe asegurarse que estén todas presentes en el objeto devuelto por la función `data`. Dónde sea necesario, utilice `null`, `undefined` o algún otro valor _placeholder_ para las propiedades, donde el valor deseado aún no está disponible.

Es posible agregar una nueva propiedad directamente a la instancia del componente sin incluirla en `data`. Sin embargo, debido a que esta propiedad no está respaldada por el objeto reactivo `$data`, el [sistema de reactividad de Vue](reactivity.html) no la rastreará automáticamente.

Vue utiliza un prefijo `$` cuando expone sus propias APIs integradas a través de la instancia del componente. También reserva el prefijo `_` para propiedades internas. Debe evitar el uso de nombres que comiencen con cualquiera de estos caracteres para propiedades _top-level_ de `data`.

## Métodos

Para agregar métodos a una instancia de un componente, utilizamos la opción `methods`. Debe ser un objeto que contenga los métodos deseados:

```js
const app = Vue.createApp({
  data() {
    return { count: 4 }
  },
  methods: {
    increment() {
      // `this` hace referencia a la instancia del componente
      this.count++
    }
  }
})

const vm = app.mount('#app')

console.log(vm.count) // => 4

vm.increment()

console.log(vm.count) // => 5
```

Vue vincula automáticamente el valor `this` con los métodos para que siempre refiera a la instancia del componente. Esto asegura que un método retendrá el valor correcto de `this` si se usa como _listener_ de eventos o _callback_. Debe evitar el uso de funciones _arrow_ al definir métodos, ya que este tipo de funciones evitan que Vue vincule el valor apropiado de `this`.

Al igual que todas las demás propiedades de la instancia del componente, los métodos son accesibles desde dentro del _template_ del componente. Dentro de un _template_, se usan más comúnmente como _listeners_ de eventos:

```html
<button @click="increment">Voto a favor</button>
```

En el ejemplo anterior, el método `increment` se llamará cuando se hace click en `<button>`.

También es posible llamar a un método directamente desde un _template_. Como veremos en breve, generalmente es mejor usar una [propiedad computada](computed.html) en su lugar. Sin embargo, el uso de un método puede resultar útil en situaciones en las que las propiedades computadas no sean una opción viable. Puede llamar a un método en cualquier lugar donde un _template_ admita expresiones de JavaScript:

```html
<span :title="toTitleDate(date)">
  {{ formatDate(date) }}
</span>
```

Si los métodos `toTitleDate` o` formatDate` acceden a cualquier dato reactivo, entonces se tomará como una dependencia de renderización, tal cual como si se hubiera utilizado directamente en el _template_.

Los métodos llamados desde un _template_ no deberían tener efectos secundarios, como cambiar datos o desencadenar procesos asíncronos. Si se siente tentado a hacer esto, probablemente debería usar un [_hook_ del ciclo de vida](instance.html#lifecycle-hooks).

### Debouncing y Throttling

Vue no incluye soporte incorporado para _debouncing_ o _throttling_ pero se puede implementar utilizando librerías como [Lodash](https://lodash.com/).

En los casos donde un componente solo se utiliza una vez, _debouncing_ se puede aplicar directamente dentro de `methods`:

```html
<script src="https://unpkg.com/lodash@4.17.20/lodash.min.js"></script>
<script>
  Vue.createApp({
    methods: {
      // Debouncing con Lodash
      click: _.debounce(function() {
        // ... responde al click ...
      }, 500)
    }
  }).mount('#app')
</script>
```

Sin embargo, este enfoque es potencialmente problemático para los componentes que se reutilizan porque todos compartirán la misma función _debounced_. Para mantener las instancias de los componentes independientes entre sí, podemos agregar la función _debounced_ en `created`:

```js
app.component('save-button', {
  created() {
    // Debouncing con Lodash
    this.debouncedClick = _.debounce(this.click, 500)
  },
  unmounted() {
    // Cancelar el temporizador cuando se remueve el componente
    this.debouncedClick.cancel()
  },
  methods: {
    click() {
      // ... responde al click ...
    }
  },
  template: `
    <button @click="debouncedClick">
      Guardar
    </button>
  `
})
```
