<template>
  <App :cartoes-listados="cartoesListados"></App>
</template>

<script>
import App from './app.vue';
import URLS from '../utils/urls'

export default {
    name: "IndexPage",
    components: { App },
    async asyncData({$axios}) {
      const cartoesListados = await $axios.$get(URLS.CARTOES_LISTAR);
      return { cartoesListados }
    },
}
</script>
