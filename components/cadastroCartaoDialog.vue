<template>
  <b-modal
    title="Novo Cartão"
    :visible="visibility"
    @hide="fechar()"
  >
    <b-form-group>
      <label for="nomeCartao">Selecione o nome do cartão:</label>
      <b-form-select
        v-model="cartaoSelecionado.nomeCartao"
        id="nomeCartao"
        size="small"
        :options="cartoes"
        value-field="data"
        text-field="label"
      ></b-form-select>
    </b-form-group>
    
    <b-form-group>
      <label for="limiteCartao">Limite</label>
      <b-form-input
        id="limiteCartao"
        v-model="cartaoSelecionado.limite"
        type="number"
        min="0.00"
      >

      </b-form-input>
    </b-form-group>

    <template #modal-footer>
      <b-button variant="primary" size="sm">Cadastrar</b-button>
      <b-button variant="danger" size="sm" @click="fechar()">Fechar</b-button>
    </template>
  </b-modal>
</template>

<script>
  import { cartoes } from '~/utils/constantes';

  export default {
    data() {
      return {
        cartaoSelecionado: {
          nomeCartao: null,
          limite: 0
        }
      }
    },

    props: {
      visibility: Boolean
    },

    computed: {
      cartoes() {
        return cartoes
      }
    },

    methods: {
      reset() {
        this.cartaoSelecionado = {
          nomeCartao: null,
          limite: 0
        }
      },

      fechar() {
        this.reset()
        this.$emit('close')
      }
    }
  }
</script>