<template>
  <q-page class="flex flex-center bg-black">
    <div class="container">
      <h4 class="row">
        <span class="q-ml-lg">Nombre:</span>
        <p class="text-bold q-mx-lg">{{ apiData.name }}</p>
      </h4>

      <div class="flex flex-center q-pb-xl">
        <q-img :src="url" spinner-color="white" style="height: 70px; max-width: 150px" />
      </div>

      <span class="row">
        <span class="q-ml-lg">Tipo:</span>
        <p class="text-bold q-mx-lg">{{ apiData.types?.[0].type.name }}</p>
      </span>

      <span class="q-ml-lg">Habilidades</span>
      <div class="q-mb-lg">
        <q-list
          v-for="(item, index) in apiData.abilities"
          :key="index"
          bordered
          separator
          class="q-mx-lg"
        >
          <q-item clickable v-ripple>
            <q-item-section>{{ item.ability.name }}</q-item-section>
          </q-item>
        </q-list>
      </div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent, ref, onMounted } from 'vue'
import { api } from 'src/boot/axios'

export default defineComponent({
  name: 'IndexPage',
  setup() {
    const apiData = ref({})
    const url = ref('')

    const fetchApi = async () => {
      try {
        const response = await api.get('/pika-data')
        apiData.value = response.data
        url.value = response.data.sprites.front_default
        console.log('~ fetchApi ~ response:', response.data)
      } catch (error) {
        console.log('~ fetchApi ~ error:', error)
      }
    }

    onMounted(() => {
      fetchApi()
    })

    return {
      apiData,
      fetchApi,
      url,
    }
  },
})
</script>
<style lang="scss" scoped>
.container {
  background: rgb(255, 218, 9);
}
</style>
