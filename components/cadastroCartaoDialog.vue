<template>
  <b-modal  
    :visible="visibility"
    @hide="fechar()"
  >
    <template #modal-header>
      <b-row>
        <b-col>
          <span style="font-weight: 500; font-size: large;">Novo Cartão</span>     
          <b-icon icon="credit-card" variant="info"/>
        </b-col>
      </b-row>

    </template>
    <b-form-group>
      <label for="nomeCartao">Selecione o nome do cartão:</label>
      <b-form-select
        v-model="cartaoForm.nomeCartao"
        id="nomeCartao"
        size="small"
        :options="cartoes"
        value-field="data"
        text-field="label"
      ></b-form-select>
    </b-form-group>
    
    <b-form-group>
      <label for="limiteCartao">Limite</label>
      <money
        id="limiteCartao"
        v-model="cartaoForm.limite"
        v-bind="money"
        class="form-control"
        size="sm"
      />
    </b-form-group>

    <template #modal-footer>
      <b-button variant="primary" size="sm" @click="recordNewCreditCard()">Cadastrar</b-button>
      <b-button variant="danger" size="sm" @click="fechar()">Fechar</b-button>
    </template>
  </b-modal>
</template>

<script>
  import { cartoes } from '~/utils/constantes';
  import URLS from '../utils/urls'

  export default {
    data() {
      return {
        cartaoForm: {
          nomeCartao: null,
          limite: 0.0
        },
        money: {
          decimal: ',',
          thousands: '.',
          prefix: 'R$ ',
          precision: 2,
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
      async recordNewCreditCard() {
        debugger
        const payload = {
          ...this.cartaoForm
        }

        await this.$axios.$post(URLS.CARTOES_GRAVAR, payload);
      },

      reset() {
        this.cartaoSelecionado = {
          nomeCartao: null,
          limite: 0.0
        }
      },

      fechar() {
        this.reset()
        this.$emit('close')
      }
    }
  }
</script>