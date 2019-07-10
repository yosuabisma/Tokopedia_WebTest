<template>
  <div id="input-nominal">
    <input class="input-style" type="text" v-model="message" placeholder="Your Nominal...">
    <div class="submit-button" v-on:click="calculateAmount()">Submit</div>
    <div id="details">
      <h3>Details</h3>
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
      <br/>
      <font><b>Left: Rp {{left}} (no available fraction)</b></font>
    </div>
  </div>
</template>
<script>
  export default {
    components: {

    },
    data() {
      return {
        message:null,
        left:0,
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
        var tempAmount = 0;
        for(var i=0; i<this.fractions.length;i++){
          this.fractions[i].amount=0;
        }
        for(var i=0; i<this.fractions.length;i++){
          tempAmount = parseInt(nominalInput/this.fractions[i].nominal);
          if(tempAmount >= 1){
            this.fractions[i].amount= tempAmount;
            tempFraction = this.fractions[i].nominal*tempAmount;
            nominalInput = nominalInput - tempFraction;
            if(nominalInput>0){
              this.left=nominalInput;
            }
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
    margin-top: 8px;
    background-color:#26dc46;
    color: #ffffff;
    cursor: pointer;
    padding: 10px 10px;
    width: 400px;
    border-radius: 8px;
    font-size: 18px;
  }
  .submit-button:hover{
    background-color:#26dc46;
  }
  #details{
    margin-top: 30px;
  }
  h3{
    color:#26dc46;
    border-bottom: 2px solid #26dc46;
    width: 100px;
  }
  table{
    width:400px;
  }
  td,th{
    padding: 5px 20px;
    width: 200px;
    text-align: left;
    font-size: 16px;
  }
  .cell-header{
    background-color:#26dc46;
    color: #ffffff;
  }
  .cell{
    background-color: #ececec;
  }
</style>
