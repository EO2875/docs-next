# Data

## data

- **Tipo:** `Function`

- **Detalles:**

  La función que regresa un objeto de datos para el la instancia del componente. En `data`, no recomendamos observar objetos con su propio comportamiento de estado, como por ejemplo los objetos de la API del browser y las propiedades del prototype. Una buena idea sería tener un objeto sencillo que represente los datos del componente.

  Una vez observado, ya no se podrán agregar propiedades reactivas al objeto raíz. Es entonces recomendado delcarar todas las propiedades reactivas de raíz desde el principio, antes de crear la instancia del componente.

  Una vez que la instancia es creada, el objeto de datos original puede ser accedido como `vm.$data`. La instancia del componente también imita todas las propiedades encontradas en el objeto de datos, por lo que `vm.a` será equivalente a `vm.$data.a`.

  Las propiedades que empiezan con `_` o `$` **no** serán reflejadas en la instancia del componente porque puede que entren en conflicto con las propiedades internas y métodos del API de Vue. Tendrás acceso a ellas a través de `vm.$data._propiedad`.

- **Ejemplo:**

  ```js
  // direct instance creation
  const data = { a: 1 }

  // The object is added to a component instance
  const vm = Vue.createApp({
    data() {
      return data
    }
  }).mount('#app')

  console.log(vm.a) // => 1
  ```

  Nótese que si usas una función flecha con la propiedad `data`, `this` no será la instancia del componente, pero aún tendrás acceso a la instancia a través del primer argumento de la función:

  ```js
  data: vm => ({ a: vm.miProp })
  ```

- **Véase también:** [Reactividad a profundidad](../guide/reactivity.html)

## props

- **Tipo:** `Array<string> | Object`

- **Detalles:**

  Una lista o hash de atributos que son expuestos para aceptar información del componente pariente. Para configuraciones simples se usa una sintaxis de Array, y para configuraciones avanzadas se usa una sintaxis de objeto que permite compobación de tipo, validación personalizada, y valores por defecto.

  La sintaxis de Array es una lista de `String` de los nombres de las props.

  Usando la sintaxis de Objeto, las llaves representan los nombres de las props y el valor es un objeto en el que se pueden usar las siguientes opciones:

  - `type`: puede ser cualquiera de los constrcutores nativos siguientes: `String`, `Number`, `Boolean`, `Array`, `Object`, `Date`, `Function`, `Symbol`, cualquier función constructor, o una lista de estos. Se comprobará que el prop sea del tipo dado, y de ser lo contrario lanzará una advertencia. [Más información](../guide/component-props.html#prop-types) en tipos de prop.
  - `default`: `any`
    Especifica un valor por defecto de la prop. Si el prop no es dado, este valor se tomará. Si se desea que el valor por defecto sea un objeto o un array, se requerirá de una función fábrica.
  - `required`: `Boolean`
    Define si la prop es requerida. Si no se está en un ambiente de producción, se lanzará una advertencia en la consola si este valor es verdadero y si la prop no fue dada.
  - `validator`: `Function`
    Función validadora personalizada que toma el valor del prop como único argumento. Si no se está en un ambiente de producción, se lanzará una advertencia en la consola si esta función regresa un valor negativo (i.e. si la validación falla). Puedes leer más acerca de la validación de props [aquí](../guide/component-props.html#prop-validation).

- **Ejemplo:**

  ```js
  const app = Vue.createApp({})

  // sintaxis simple
  app.component('props-demo-simple', {
    props: ['tamano', 'miMensaje']
  })

  // sintáxis de objeto con validación
  app.component('props-demo-advanced', {
    props: {
      // comprobación de tipo
      altura: Number,
      // comprobación de tipo y otros validadores
      edad: {
        type: Number,
        default: 0,
        required: true,
        validator: valor => {
          return valor >= 0
        }
      }
    }
  })
  ```

- **Véase también:** [Props](../guide/component-props.html)

## computed

- **Tipo:** `{ [key: string]: Function | { get: Function, set: Function } }`

- **Detalles:**

  Propiedades computadas que serán mezcladas a la instancia del componente. Todos los getters y setters tendrán el contexto `this` automáticamente atados a la instancia del componente.

  Nótese que si se usa una función flecha con una propiedad computada, `this` no será la instancia del componente, pero aún podrás acceder a la instance a través del primer argumento de la función.

  ```js
  computed: {
    aDouble: vm => vm.a * 2
  }
  ```

  Las propiedades computadas serán cacheadas, y re-computadas cuando las dependencias reactivas cambien. Nótese que si cierta dependencia está fuera del ámbito de la instancia (i.e. no reactiva), la propiedad computada **no** será actualizada.

- **Ejemplo:**

  ```js
  const app = Vue.createApp({
    data() {
      return { a: 1 }
    },
    computed: {
      // solo de acceso/get
      aDoble() {
        return this.a * 2
      },
      // de acceso/get y edicion/set
      aMasUno: {
        get() {
          return this.a + 1
        },
        set(v) {
          this.a = v - 1
        }
      }
    }
  })

  const vm = app.mount('#app')
  console.log(vm.aDoble) // => 2
  vm.aDoble = 3
  console.log(vm.a) // => 2
  console.log(vm.aDoble) // => 4
  ```

- **Véase también:** [Propiedades Computadas](../guide/computed.html)

## methods

- **Tipo:** `{ [key: string]: Function }`

- **Detalles:**

  Métodos que serán mezclados a la instancia del componente. Podras acceder a estos métodos directamente de la instancia del componente, o usarlos en expresiones de directivas. Todos los métodos tendrán el contexto `this` automáticamente atados a la instancia del componente.

  :::tip Notas
  Nótese que **no deberías de usar una función flecha para definir un método** (e.g. `anadir: () => this.a++`), ya que las funciones flecha atarán el contexto pariente, y `this` **no** será la instancia del componente como esperas y `this.a` será indefinido.
  :::

- **Ejemplo:**

  ```js
  const app = Vue.createApp({
    data() {
      return { a: 1 }
    },
    methods: {
      anadir() {
        this.a++
      }
    }
  })

  const vm = app.mount('#app')

  vm.anadir()
  console.log(vm.a) // => 2
  ```

- **Véase también:** [Manejo de Eventos](../guide/events.html)

## watch

- **Tipo:** `{ [key: string]: string | Function | Object | Array}`

- **Detalles:**

  Un objeto donde las llaves son expresiones a observar y los valores son las callbacks correspondientes. El valor también puede ser una string con el nombre del método, o un objeto que contiene opciones adicionales. La instancia del componente llamará a `$watch()` por cada entrada en este objeto al momento de instanciación. Véase [$watch](instance-methods.html#watch) para más información acerca de las opciones `deep`, `immediate` y `flush`.

- **Ejemplo:**

  ```js
  const app = Vue.createApp({
    data() {
      return {
        a: 1,
        b: 2,
        c: {
          d: 4
        },
        e: 'prueba',
        f: 5
      }
    },
    watch: {
      a(valor, valorViejo) {
        console.log(`nuevo: ${val}, viejo: ${oldVal}`)
      },
      // string method name
      b: 'unMetodo',
      // the callback will be called whenever any of the watched object properties change regardless of their nested depth
      c: {
        handler(valor, valorViejo) {
          console.log('c cambió')
        },
        deep: true
      },
      // the callback will be called immediately after the start of the observation
      e: {
        handler(valor, valorViejo) {
          console.log('e cambió')
        },
        immediate: true
      },
      // you can pass array of callbacks, they will be called one-by-one
      f: [
        'manejador1',
        function manejador2(valor, valorViejo) {
          console.log('manejador2 ha sido llamado')
        },
        {
          handler: function manejador3(valor, valorViejo) {
            console.log('manejador3 ha sido llamado')
          }
          /* ... */
        }
      ]
    },
    methods: {
      unMetodo() {
        console.log('b cambió')
      },
      manejador1() {
        console.log('handle 1 ha sido llamado')
      }
    }
  })

  const vm = app.mount('#app')

  vm.a = 3 // => new: 3, old: 1
  ```

  ::: tip Note
  Nótese **no deberías de usar una función flecha para definir un watcher** (e.g. `busqueda: nuevoValor => this.actualizarAutocompletado(nuevoValor)`), ya que las funciones flecha atarán el contexto pariente, y `this` **no** será la instancia del componente como esperas y `this.actualizarAutocompletado` será indefinido.
  :::

- **Véase también:** [Watchers](../guide/computed.html#watchers)

## emits

- **Tipo:** `Array<string> | Object`

- **Detalles:**

  Una lista o hash de eventos personalizados que pueden ser emitidos desde el componente. Para configuraciones simples se usa una sintaxis de Array, y para configuraciones avanzadas se usa una sintaxis de objeto que permite configurar la validación del evento.

  Usando la sintaxis de objeto, el valor de cada propiedad puede ser o `null` o una función de validación. La función de validación recibirá los argumentos adiciones pasados a `$emit`. Por ejemplo, si se llama a `this.$emit('foo', 1)`, el validador para `foo` recibirá el argumento `1`. La función validadora debería regresar un boolean para indicar si el evento fue válido.

- **Uso:**

  ```js
  const app = Vue.createApp({})

  // Sintaxis de Array
  app.component('todo-item', {
    emits: ['checado'],
    created() {
      this.$emit('checado')
    }
  })

  // Sintaxis de Objeto
  app.component('reply-form', {
    emits: {
      // sin validación
      checado: null,

      // con validación
      enviar: info => {
        if (info.email && info.password) {
          return true
        } else {
          console.warn(`El evento 'enviar' recibió información inválida!`)
          return false
        }
      }
    }
  })
  ```

  ::: tip Nota
  Los eventos escuchados el la opción `emits` **no** serán heredados por el elemento raíz del componente y también serán excluidos de la propiedad `$attrs`.
  :::

* **Véase también:** [Herencia de Atributos](../guide/component-attrs.html#attribute-inheritance)
