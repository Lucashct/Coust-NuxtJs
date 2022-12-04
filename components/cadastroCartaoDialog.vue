<template>
  <b-modal
    title="Novo Cartão"
    :visible="visibility"
    @hide="fechar()"
  >
    <b-form-group>
      <b-form-input
        label="Nome"
      >

      </b-form-input>
    </b-form-group>

    <template #modal-footer>
      <b-button @click="fechar()">Fechar</b-button>
    </template>
  </b-modal>
</template>

<script>
  export default {
    props: {
      visibility: Boolean
    },

    methods: {
      fechar() {
        this.$emit('close')
      }
    }
  }
</script>