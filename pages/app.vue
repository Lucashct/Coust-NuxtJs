<template>
  <b-container class="container-geral">
    <div class="titulo-container">
      <span class="titulo-font">COUST</span>
    </div>

    <div class="cartoes-container">
      <b-card sub-title="Cartões">
        <b-row style="padding: 5px;" cols="4">
          <!-- INFO SOBRE OS CARTÕES -->
          <b-col>
            <b-row style="margin-left: 20px;">
              <span style="font-weight:600; font-size: 15px">Nome: </span> <span style="font-size: 15px;">{{ ' Teste' }}</span>
            </b-row>

            <b-row style="margin-left: 20px; margin-top: 20px;">
              <span style="font-weight:600; font-size: 15px">Limite: </span> <span style="font-size: 15px;">{{ ' R$ 0.00' }}</span>
            </b-row>

            <b-row style="margin-left: 20px; margin-top: 20px;">
              <span style="font-weight:600; font-size: 15px">Limite Disponível: </span> <span style="font-size: 15px;">{{ ' R$ 0.00' }}</span>
            </b-row>
          </b-col>
          
          <!-- IMAGEM DO CARTÃO -->
          <b-col cols="6">

          </b-col>
        </b-row>
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
  import URLS from '../utils/urls'

  export default {
    props: {
      cartoesListados: Object
    },

    components: {
      CadastroCartaoDialog
    },

    data() {
      return {
        visibilidadeNewCardModal: false
      }
    },

    async asyncData({$axios}) {
      debugger
      const cartoesListados = await $axios.$get(URLS.CARTOES_LISTAR);
      return { cartoesListados }
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