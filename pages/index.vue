<template>
  <div class="w-full md:min-h-screen bg-gray-100" v-if="!isLoading">
    <div class="md:container md:mx-auto md:p-10">
      <div class>
        <div class="w-full py-8 px-6 bg-white">
          <h1 class="text-3xl text-black font-bold mb-4 pt-6">Pokedex</h1>
          <div class="grid md:gap-4 gap-2 grid-cols-2 md:grid-cols-5">
            <pokeitem
              v-for="(row, key) in data.results"
              :key="key"
              :row="row"
              @onDetail="fullView.view = row.details;slideoverStates.view = true;"
              class="rounded-lg bg-green-400 py-2 md:py-4 cursor-pointer"
            />
          </div>
          <div class="mt-8 text-center">
            <span v-show="!isSpinnerLoading" @click="fetchMoreData" class="border-2 cursor-pointer border-green-500 py-1 px-3 rounded-full text-xl text-green-500 hover:bg-gray-100 text-center">Load More</span>
            <LoadingSpinner v-if="isSpinnerLoading" />
          </div>
        </div>
      </div>
      <Slideover v-show="slideoverStates.view" v-model="slideoverStates.view" @onCancel="slideoverStates.view = false" :fullView="fullView" />
    </div>
  </div>
  <div v-else><ScreenLoader :isLoading="isLoading" /></div>
</template>

<script>
import pokeitem from "@/components/pokeitem.vue";
export default {
  data() {
    return {
      slideoverStates: {
        view: false
      },
      isLoading: false,
      fullView: {
        view: {
          sprites: {}
        }
      },
      data: {
        results: []
      },
      isSpinnerLoading: false,
      nextFetchUrl: ''
    };
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    async fetchMoreData() {
      this.isSpinnerLoading = true
      this.page = this.page += 1
      const responseList = await this.$axios.get(this.nextFetchUrl);
      this.nextFetchUrl = responseList.data.next
      const responseSet = []
      responseList.data.results.map(x => {
        responseSet.push({
          name: x.name,
          url: x.url,
          details: {
            sprites: {
              front_default:""
            },
            "types": [
            {
              "type": {
                "name": "",
                "url": ""
              }
            }
          ],
          }
        })
      })
      responseList.data.results = responseSet
      responseList.data.results.map(async x => {
        const responseView = await this.$axios.get(x.url);

        x.details = responseView.data
        this.data.results.push(x)
      })
      this.isSpinnerLoading = false
    },
  async fetchData() {
      this.isLoading = true
      var responseList = await this.$axios.get('https://pokeapi.co/api/v2/pokemon/?limit=20&offset=0');
      this.nextFetchUrl = responseList.data.next
      var responseSet = []
      responseList.data.results.map(x => {
        const obj = {
            sprites: {
              front_default:""
            },
            types: [
            {
              type: {
                name: "",
                url: ""
              }
            }
          ],
          }
          const obb = {
          name: x.name,
          url: x.url,
          details: obj
        }
        responseSet.push(obb)
      })
      responseList.data.results = responseSet
      responseList.data.results.map(async x => {
        const responseView = await this.$axios.get(x.url);
        
        x.details = responseView.data
      });
      this.data = responseList.data
      this.isLoading = false
    }
  }
};
</script>