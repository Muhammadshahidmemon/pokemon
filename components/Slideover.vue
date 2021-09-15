<template>
    <section class="fixed w-full overflow-hidden inset-y-0 right-0 flex" v-show="value">  <transition
      mode="out-in"
        enter-active-class="ease-out duration-300"
        enter-class="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
        enter-to-class="opacity-100 translate-y-0 sm:scale-100"
        leave-active-class="ease-in duration-200"
        leave-class="opacity-100 translate-y-0 sm:scale-100"
        leave-to-class="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
    >
  <div class="fixed w-full overflow-hidden min-h-screen bg-black bg-opacity-50 transition-opacity cursor-pointer" v-show="value" @click="onCancel">
  </div>
    </transition>
    <div class="flex-1">
    </div>


  <transition
    enter-active-class="transform transition ease-in-out duration-500 sm:duration-700"
    enter-class="translate-x-full"
    enter-to-class="translate-x-0"
    leave-active-class="transform transition ease-in-out duration-500 sm:duration-700"
    leave-class="translate-x-0"
    leave-to-class="translate-x-full">
    <div class="w-screen max-w-lg z-50 static"  v-show="value">
      <div class="h-full flex flex-col bg-white shadow-xl overflow-y-auto">
        <div class="flex-1">
          <div class="bg-white">
            <div class="bg-green-400 px-6 pt-8">
              <div @click="onCancel">
                <HeroIcon name="ArrowLeftIcon" tailwind="w-6 cursor-pointer text-white block md:hidden" />
              </div>
              <div class="flex justify-between">
                <h1 class="text-4xl text-white font-bold mb-4 pt-2">{{ fullView.view.name }}</h1>
                <h1 class="text-xl text-white font-bold mb-4 pt-8">#0{{ fullView.view.order }}</h1>
              </div>
              <button
                class="rounded-full focus:outline-none md:text-base text-xs text-white font-bold bg-green-300 py-1 md:py-2 px-4 leading-4 mr-2"
                v-for="(button, key) in fullView.view.types"
                :key="key"
              >{{ button.type.name }}</button>
              <img :src="fullView.view.sprites.front_default" class="w-full mx-auto" style="margin-bottom: -50px;" />
            </div>
            <div class="bg-green-400">
              <div class="bg-white px-6 py-16" style="border-top-left-radius: 2rem;
    border-top-right-radius: 2rem;">
                <div class="flex py-1">
                  <div class="w-4/12">
                    <h4 class="text-lg text-gray-500 font-semibold">Experience</h4>
                  </div>
                  <div class="w-8/12">
                    <h4 class="text-lg font-semibold text-gray-900">{{ fullView.view.base_experience }} EX</h4>
                  </div>
                </div>
                <div class="flex py-1">
                  <div class="w-4/12">
                    <h4 class="text-lg text-gray-500 font-semibold">Height</h4>
                  </div>
                  <div class="w-8/12">
                    <h4 class="text-lg font-semibold text-gray-900">{{ fullView.view.height }} m</h4>
                  </div>
                </div>
                <div class="flex py-1">
                  <div class="w-4/12">
                    <h4 class="text-lg text-gray-500 font-semibold">Weight</h4>
                  </div>
                  <div class="w-8/12">
                    <h4 class="text-lg font-semibold text-gray-900">{{ fullView.view.weight }} kg</h4>
                  </div>
                </div>
                <div class="flex py-1">
                  <div class="w-4/12">
                    <h4 class="text-lg text-gray-500 font-semibold">Abilities</h4>
                  </div>
                  <div class="w-8/12">
                    <h4 class="text-lg font-semibold text-gray-900">
                      <span
                        v-for="(row, key) in fullView.view.abilities"
                        :key="key"
                      > {{ row.ability.name }}</span>
                    </h4>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </transition>
    </section>
</template>

<script>
const bodyScroll = require('body-scroll-toggle')
export default {
  props: {
    value: {
      type: Boolean,
      required: true
    },
    fullView: {
      type: Object,
      required: true
    },
  },
  watch: {
    value() {
      if (this.value) {
        bodyScroll.disable()
      } else {
        bodyScroll.enable()
      }
    }
  },
  mounted() {
    console.log(this.fullView);
  },
  methods: {
    updateValue: function (value) {
      this.$emit('input', value)
    },
    onCancel () {
      this.$emit('onCancel')
    },
  }
}
</script>
