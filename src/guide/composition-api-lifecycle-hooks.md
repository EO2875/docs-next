# Hooks del Ciclo de Vida

> Esta guía asume que usted ya leyo la [Introducción de la API de Composición](composition-api-introduction.html) y [Fundamentos de la Reactividad](reactivity-fundamentals.html). Lea esto antes si usted es nuevo en lo que refiere a la API de Composición.

<VideoLesson href="https://www.vuemastery.com/courses/vue-3-essentials/lifecycle-hooks" title="Learn about how Lifecycle Hooks work with Vue Mastery">Vea un video gratuito sobre setup en Vue Mastery</VideoLesson>

Usted puede acceder a los _hooks_ del ciclo de vida agregando el prefijo "on" al _hook_.

La siguiente table contiene como los _hooks_ del ciclo de vida son invocados desde [setup()](composition-api-setup.html):

| API de Opciones   | _Hook_ dentro de`setup`    |
| ----------------- | -------------------------- |
| `beforeCreate`    | No es necesario\*          |
| `created`         | No es necesario\*          |
| `beforeMount`     | `onBeforeMount`            |
| `mounted`         | `onMounted`                |
| `beforeUpdate`    | `onBeforeUpdate`           |
| `updated`         | `onUpdated`                |
| `beforeUnmount`   | `onBeforeUnmount`          |
| `unmounted`       | `onUnmounted`              |
| `errorCaptured`   | `onErrorCaptured`          |
| `renderTracked`   | `onRenderTracked`          |
| `renderTriggered` | `onRenderTriggered`        |

:::tip
Como `setup` es ejecutado alrededor de los _hooks_ del ciclo de vida `beforeCreate` y `created`, usted no necesita definirlos explícitamente. En otras palabras, cualquier código que fuera a ser escrito dentro de estos, puede ser escrito directamente en la función `setup`.
:::

Esta función acepta un _callback_ que será ejecutado cuando el _hook_ sea invocado por el componente:

```js
// MyBook.vue

export default {
  setup() {
    // mounted
    onMounted(() => {
      console.log('El componente fue montado!')
    })
  }
}
```
