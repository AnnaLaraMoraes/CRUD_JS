<template>
  <div id='pessoas'>
    <div v-bind:key="pessoa.id" v-for="pessoa in pessoas">
      <PessoaP v-bind:pessoa="pessoa" v-on:del-pessoa="$emit('del-pessoa', pessoa.id)" />
    </div>
  </div>
</template>

<script>
import PessoaP from './PessoaP.vue';
export default {
  name: "pessoas",
  components: {
    PessoaP
  },
  props: ["pessoas"]
}
</script>

<style scoped>
</style>