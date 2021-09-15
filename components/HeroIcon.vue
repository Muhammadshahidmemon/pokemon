<template>
<component :is="name" :class='tailwind'></component>
</template>

<script>
import * as components from '@vue-hero-icons/solid'

export default {
  components: components,
  props: {
    name: {
      type: String,
      required: true
    },
    tailwind: {
      type: String
    }
  }
}
</script>
