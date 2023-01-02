<template>
  <b-container class="container-geral">
    <div class="titulo-container">
      <span class="titulo-font">COUST</span>
    </div>

    <div class="cartoes-container">
      <b-card sub-title="Cartões">
        <b-row style="padding: 5px;">
          <!-- INFO SOBRE OS CARTÕES -->
          <b-col cols="5">
            <b-row style="margin-left: 20px; margin-top: 25px;">
              <span style="font-weight:600; font-size: 15px">Nome: {{ cartaoSelecionado ? cartaoSelecionado.nomeCartao : '' }}</span>
            </b-row>

            <b-row style="margin-left: 20px; margin-top: 20px;">
              <span style="font-weight:600; font-size: 15px">Limite: {{ cartaoSelecionado ? cartaoSelecionado.limite : '' }}</span> 
            </b-row>

            <b-row style="margin-left: 20px; margin-top: 20px;">
              <span style="font-weight:600; font-size: 15px">Limite Disponível: </span> <span style="font-size: 15px;">{{ ' R$ 0.00' }}</span>
            </b-row>
          </b-col>
          
          <!-- IMAGEM DO CARTÃO -->
          <b-col cols="5">
            
            <b-row>
                <div>
                  <b-button @click="previousCard()" variant="outline" style="font-size: 30px; margin-top: 50px;" size="sm" 
                    :disabled="disablePreviousButton">
                    <b-icon icon="arrow-left-square-fill" :variant="disablePreviousButton ? 'secondary' : 'info'"/>
                  </b-button>
                </div>

                <img :src="require(`~/assets/img/${imgCardSelect}.png`)" width="280" height="177" 
                  style="margin: 0px 20px 0px 20px"
                />
                
                <div>
                  <b-button @click="nextCard()" variant="outline" style="font-size: 30px; margin-top: 50px;" size="sm"
                    :disabled="disableNextButton">
                    <b-icon icon="arrow-right-square-fill" :variant="disableNextButton ? 'secondary' : 'info'"/>
                  </b-button>
                </div>
            </b-row>
            
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

  export default {
    props: {
      cartoesListados: Object
    },

    components: {
      CadastroCartaoDialog
    },

    data() {
      return {
        visibilidadeNewCardModal: false,

        indexCartao: 0,

        cartaoSelecionado: undefined,

        imgCardSelect: undefined
      }
    },

    created() {
      this.fillCartaoSelecionado();
    },

    computed: {
      disablePreviousButton() {
        return this.indexCartao === 0;
      },

      disableNextButton() {
        return (this.indexCartao + 1) === this.cartoesListados.lista.length
      }
    },

    methods: {
      showCreateNewCardDialog() {
        this.visibilidadeNewCardModal = true
      },
      closeCreateNewCarDialog() {
        this.visibilidadeNewCardModal = false
      },
      fillCartaoSelecionado(index) {
        const realIndex = index ? index : 0

        this.cartaoSelecionado = this.cartoesListados.lista[realIndex]

        switch (this.cartaoSelecionado.nomeCartao) {
          case 'NEON':
            this.imgCardSelect = 'credit-card-neon'
            break;
          case 'ITAU_CLICK':
            this.imgCardSelect = 'credit-card-click'
            break;
          case 'NU_BANK':
            this.imgCardSelect = 'credit-card-nubank'
            break;
        }
      },

      nextCard() {
        const totalCartoes = this.cartoesListados.lista.length

        this.indexCartao = this.indexCartao + 1

        if((this.indexCartao + 1) <= totalCartoes) {
          this.fillCartaoSelecionado(this.indexCartao)
        }
      },

      previousCard() {
        const totalCartoes = this.cartoesListados.lista.length
        
        this.indexCartao = this.indexCartao - 1

        if((this.indexCartao + 1) < totalCartoes && (this.indexCartao > 0)) {
          this.fillCartaoSelecionado(this.indexCartao)
        }
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