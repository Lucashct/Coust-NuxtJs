<template>
  <b-container class="container-geral">
    <div class="titulo-container">
      <span class="titulo-font">COUST</span>
    </div>

    <div class="cartoes-container">
      <b-card sub-title="Cartões">
        
      </b-card>
      <div class="botoes-cartoes">
        <b-button @click="showCreateNewCardDialog" variant="success" size="sm">Adicionar</b-button>
        <b-button variant="danger" size="sm">Remover</b-button>
      </div>
    </div>

    <div style="margin-top: 90px;">
      <b-card sub-title="Contas do mês">

      </b-card>
    </div>
    <cadastro-cartao-dialog :visibility="visibilidadeNewCardModal" @close="closeCreateNewCarDialog()"/>
  </b-container>
</template>

<script>
  import CadastroCartaoDialog from '../components/cadastroCartaoDialog.vue'

  export default {
  components: {
    CadastroCartaoDialog
  },

  data() {
    return {
      visibilidadeNewCardModal: false
    }
  },

  computed: {
    testString() { return this.$store.state.cartoesStore.testeString }
  },

  methods: {
    showCreateNewCardDialog() {
      this.visibilidadeNewCardModal = true
    },
    closeCreateNewCarDialog() {
      this.visibilidadeNewCardModal = false
    }
  }
 }
</script>

<style>
  .container-geral {
    padding: -5px;
  }

  .titulo-container {
    text-align: center;
  }

  .titulo-font {
    font-size: 30px;
    font-weight: 600;
  }

  .botoes-cartoes {
    padding-top: 5px;
    float: right;
  }
</style>