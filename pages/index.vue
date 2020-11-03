<template>
  <div class="m-0 min-h-screen flex items-center justify-center app">
    <div class="flex">
      <AddressType v-model="type" :options="typeArray" />
      <div class="m-2"></div>
      <div>
        <AddressInput v-for="index in count" :key="index" @plus="count++" />
      </div>
    </div>
  </div>
</template>

<script>
import AddressType from '../components/AddressType'
import AddressInput from '../components/AddressInput'

export default {
  components: {
    AddressInput,
    AddressType,
  },
  data() {
    return {
      type: 0,
      count: 1,
    }
  },
  computed: {
    typeArray() {
      return ['Residential', 'Domicile', 'Legal', 'Operational']
    },
  },
}
</script>
<style scoped>
.app {
  background-color: #181818;
  font-family: Arial, Helvetica, sans-serif;
}
</style>
