<template>
    <div class="container" style="margin-top: 30px">
    <h3>{{wallet_name}}</h3>
    <table class="table table-bordered">
    <thead>
        <tr>
        <th scope="col">#</th>
        <th scope="col">Description</th>
        <th scope="col">Amount</th>
        <th scope="col">Balance</th>
        <th scope="col">Type</th>
        </tr>
    </thead>
    <tbody>
        <tr v-for="(trans, index) in transactions" :key="trans.id">
            <th scope="row">{{index}}</th>
            <td>{{trans.description}}</td>
            <td>{{trans.amount}}</td>
            <td>{{trans.balance}}</td>
            <td>{{trans.type}}</td>
        </tr>
    </tbody>
    </table>
    <div>
    </div>
    </div>
</template>

<script>
export default {
    async asyncData(context){
    const {data} = await context.$axios.get('https://my-ewallet-demo.herokuapp.com/transactions?walletId='+context.route.params.id)
    const wallet_name = localStorage.getItem('wallet_name');
    return {
      transactions : data,
      wallet_name: wallet_name
    }
  }
}
</script>
<style scoped>

</style>

