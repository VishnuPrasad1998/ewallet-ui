<template>
  <div class="container">
    <h2>Add New Wallet</h2>
    <hr>

    <div class="row">
      <div class="col-md-6">
        <form action=""
          method="post"
          @submit.prevent="submitForm()">

          <div class="form-group">
            <label for="">Name</label>
            <input type="text" class="form-control"
              v-model="name">
          </div>
          <div class="form-group">
            <label for="">Balance</label>
            <input type="text" class="form-control"
              v-model="balance">
          </div>

          <input type="submit" value="Submit" class="btn btn-primary mr-3">

        </form>
      </div>
      <div class="col-md-6">
        <div class="boxed" v-if="wallet_name && wallet_balance">
        <h3 v-if="wallet_name" style="color: red">Wallet: <span style="color: black">{{wallet_name}}</span></h3>
        <h3 v-if="wallet_balance" style="color: red">Balance: <span style="color: black">₹{{wallet_balance}}</span></h3>
        </div>
        <div v-if="wallet_balance && wallet_name">
          <form action=""
          method="post"
          @submit.prevent="submitTransactionForm()">

          <div class="form-group">
            <label for="">Amount</label>
            <input type="text" class="form-control"
              v-model="amount">
          </div>
          <div class="form-group">
            <label for="">Description</label>
            <input type="text" class="form-control"
              v-model="description">
          </div>
          <div class="form-group">
            <input type="radio" id="debit" name="type" value="debit" v-model="type">
            <label for="debit">Debit</label><br>
            <input type="radio" id="credit" name="type" value="credit" v-model="type">
            <label for="credit">Credit</label><br>
          </div>
          <input type="submit" value="Submit" class="btn btn-success mr-3">

        </form>
        </div>
      </div>
    </div>
    <div class="row" style="margin-top: 50px">
      <button @click="onTransactionsClick" class="btn btn-success" :disabled="isDisabled">Click here to see all the transactions in the wallet</button>
    </div>
  </div>
</template>

<script>
export default {
  computed: {
      isDisabled() {
        if(this.amount && this.description){
          return false
        }else{
          return true
        }
  }},
  data(){
    return{
      name:null,
      balance:null,
      amount:null,
      description:null,
      wid: '',
      wallet_balance: '',
      wallet_name: '',
      type: ''
    }
  },
  methods:{
    submitForm(){
      this.$axios.post('https://my-ewallet-demo.herokuapp.com/setup', {
          name: this.name,
          balance: this.balance
        })
        .then((response) => {
          localStorage.setItem('wallet_id', response.data.id);
          localStorage.setItem('wallet_name', response.data.name);
          this.wallet_name = response.data.name
          this.wallet_balance = response.data.balance
          this.wid = localStorage.getItem('wallet_id');
        })
        .catch( (error) => {
          console.log(error)
        });
    },
    submitTransactionForm(){
      console.log(this.type)
      if(this.type === "debit"){
        this.amount = parseInt("-" +this.amount)
      }else{
        this.amount = parseInt(this.amount)
      }
      this.$axios.post('https://my-ewallet-demo.herokuapp.com/transact/'+this.wid, {
          amount: this.amount,
          description: this.description
        })
        .then((response) => {
          this.wallet_balance = response.data.balance
        })
        .catch( (error) => {
          console.log(error)
        });
    },
    onTransactionsClick(){
      this.$router.push('/transactions/' + this.wid)
    }

  }
}
</script>
<style scoped>
.boxed {
  padding: auto;
  margin: auto;
  text-align: center;
  border: 3px solid green;
}
</style>