<template>
  <form>
    <div class="box column">

      <p class="title">Ingrese su número de cuenta bancaria</p>
      <div class="block">
        <div class="field is-grouped">
          <div class="control is-expanded">
            <input class="input" type="number" placeholder="Número de cuenta"
              v-model="accountNum">
          </div>
          <div class="control">
            <button class="button is-primary" v-on:click.prevent="showMsg">Aceptar</button>
          </div>
        </div>
      </div>

      <div class="notification is-success" v-bind:style="'display: ' + successMsgDisplay">
        <button class="delete" type="button" @click.prevent="hideMsg"></button>
        Su regalo ha sido consignado en la cuenta indicada.
      </div>

    </div>
  </form>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      accountNum: null,
      successMsgDisplay: 'none'
    }
  },
  methods: {
    showMsg(event) {
      if (this.accountNum) {
        event.target.disabled = true;
        this.successMsgDisplay = 'block';
        setTimeout(() => document.documentElement.scrollTop = document.documentElement.scrollHeight, 200);
      }
    },
    hideMsg() {
      this.successMsgDisplay = 'none';
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
form {
  position: absolute;
  width: 100%;
  padding: 30px;
  bottom: 240px;
  z-index: 1000;
}
</style>
