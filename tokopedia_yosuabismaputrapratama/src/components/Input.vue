<template>
  <div id="input-nominal">
    <input class="input-style" type="text" v-model="message" placeholder="Your Nominal...">
    <!-- <p>{{message}}</p> -->
    <div class="submit-button" v-on:click="calculateAmount()">Submit</div>
    <table>
      <tr>
        <th class="cell-header">Fraction</th>
        <th class="cell-header">Amount</th>
      </tr>
      <tr v-for="fr in fractions">
        <td class="cell">Rp {{fr.nominal}}</td>
        <td class="cell">{{fr.amount}}</td>
      </tr>
    </table>
  </div>
</template>
<script>
  export default {
    components: {

    },
    data() {
      return {
        message:null,
        fractions : [
          {nominal:100000, amount:0},
          {nominal:50000, amount:0},
          {nominal:20000, amount:0},
          {nominal:10000, amount:0},
          {nominal:5000, amount:0},
          {nominal:1000, amount:0},
          {nominal:500, amount:0},
          {nominal:100, amount:0},
          {nominal:50, amount:0}
        ]
      }
    },
    methods:{
      calculateAmount(){
        var nominalInput = this.message;
        var tempFraction = 0;
        for(var i=0; i<this.fractions.length;i++){
          var tempAmount = nominalInput/this.fractions[i].nominal;
          if(tempAmount >= 1){
            this.fractions[i].amount= parseInt(tempAmount);
          }else{
            this.fractions[i].amount= 0;
          }
        }
      }
    }
  }
</script>
<style scoped>
  #input-nominal{
    background-color: #ffffff;
    width: 500px;
    height: 500px;
    padding: 40px 40px;
    border-radius: 16px;
    box-shadow: 0 0 40px #ececec;
    border: 1px solid #e0e0e0;
  }
  .input-style{
    border: 1px solid #e0e0e0;
    border-radius: 8px;
    width: 400px;
    padding: 10px 10px;
    color: #26dc46;
    font-size: 18px;
  }
  .input-style:focus{
    border: 1px solid #26dc46;
    outline: none !important;
  }
  ::placeholder {
    color: #e0e0e0;
  }
  .submit-button{
    background-color:#26dc46;
    color: #ffffff;
    cursor: pointer;
  }
  /* td,th{
    padding: 5px 20px;
    width: 150px;
    text-align: left;
    font-size: 16px;
    border-radius: 8px;

  }
  .cell-header{
    background-color:#26dc46;
    color: #ffffff;
  }
  .cell{
    background-color: #ececec;
  } */
</style>
