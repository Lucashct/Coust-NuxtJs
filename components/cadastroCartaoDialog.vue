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
      <b-spinner variant="info" v-if="showLoading"></b-spinner>
      <b-button variant="primary" size="sm" @click="recordNewCreditCard()" v-if="confirmButtonVisibility">Cadastrar</b-button>
      <b-button variant="danger" size="sm" @click="fechar()">Fechar</b-button>
    </template>

    <b-alert
      :show="dismissCountDown"
      dismissible
      :variant="variantAlert"
      @dismissed="dismissCountDown=0"
      @dismiss-count-down="countDownChanged"
    >
      <p>{{ alertMessage }}</p>
      <b-progress
        :variant="variantAlert"
        :max="dismissSecs"
        :value="dismissCountDown"
        height="4px"
      ></b-progress>
    </b-alert>
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
        },

        showLoading: false,
        confirmButtonVisibility: true,

        dismissSecs: 5,
        dismissCountDown: 0,
        variantAlert: '',
        alertMessage: ''
      };
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
        this.showLoading = true;
        this.confirmButtonVisibility = false;

        const payload = {
          ...this.cartaoForm
        }

        const response = await this.$axios.$post(URLS.CARTOES_GRAVAR, payload)
        
        switch (response.status) {
          case 'Sucesso':
            this.showLoading = false;
            this.confirmButtonVisibility = true;
            this.showAlert('success', 'Cartão criado com sucesso.');
            break;
          case 'Erro':
            this.showLoading = false;
            this.confirmButtonVisibility = true;
            this.showAlert('warning', 'Erro ao criar cartão');
          default:
            this.showLoading = false;
            this.confirmButtonVisibility = true;
        }
      },

      countDownChanged(dismissCountDown) {
        this.dismissCountDown = dismissCountDown
      },
      showAlert(variant, message) {
        this.variantAlert = variant;
        this.alertMessage = message;
        this.dismissCountDown = this.dismissSecs
      },

      reset() {
        this.cartaoForm.limite = 0.0;
        this.cartaoForm.nomeCartao = null;
      },

      fechar() {
        this.reset()
        this.$emit('close')
      }
    }
  }
</script>