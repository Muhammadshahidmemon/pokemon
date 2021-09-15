<template>
  <div @click="onDetail">
    <img :src="row.details.sprites.front_default" class="w-40 mx-auto" alt />
      <h3 class="text-lg text-white mb-1 md:mb-2 font-semibold text-center">{{ row.name }}</h3>
      <div class="text-center">
        <button
          v-for="(view, key) in row.details.types"
          :key="key"
          class="rounded-full focus:outline-none md:text-base text-xs text-white my-1 font-bold bg-green-300 py-1 md:pt-1 pb-2 px-3 leading-4 ml-1"
        >{{ view.type.name }}</button>
      </div>
  </div>
</template>

<script>
export default {
  props: {
    row: {
      type: Object,
      required: true
    },
  },
  methods: {
    onDetail () {
      this.$emit('onDetail')
    },
  }
}
</script>
