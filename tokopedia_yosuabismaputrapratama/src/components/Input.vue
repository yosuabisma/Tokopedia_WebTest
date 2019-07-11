<template>
  <div id="input-nominal">
    <div id="input-section">
      <div id="logo">
        <img src="./../assets/tokopedia-white-logo.png" id="logo-tokopedia">
      </div>
      <input v-if="invalid==false" class="inputs input-style" type="text" v-model="message" placeholder="Your Nominal...">
      <input v-if="invalid==true" class="inputs input-style-invalid" type="text" v-model="message" placeholder="Your Nominal...">
      <div class="submit-button" v-on:click="inputValidation()">Submit</div>
    </div>
    <div id="results-section">
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
        <div v-if="left>0">
          <font><b>Left: Rp {{left}} (no available fraction)</b></font>
        </div>
      </div>
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
        invalid:false,
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
      inputValidation(){
        this.refresh();
        var validInput = /^(Rp)?( )?(([0-9]{1,3}){1}((\.[0-9]{3})+(\,00)?)|[0-9]+)$/;
        if(this.message.match(validInput)){
          this.invalid=false;
          this.calculateAmount();
        }else{
          this.invalid=true;
        }
      },
      calculateAmount(){
        var removeChar = /[Rp\. ]+/g;
        var nominalInput = parseInt(this.message.replace(removeChar, ""));
        var tempFraction = 0;
        var tempAmount = 0;
        for(var i=0; i<this.fractions.length;i++){
          tempAmount = parseInt(nominalInput/this.fractions[i].nominal);
          if(tempAmount >= 1){
            this.fractions[i].amount= tempAmount;
            tempFraction = this.fractions[i].nominal*tempAmount;
            nominalInput = nominalInput - tempFraction;

          }else{
            this.fractions[i].amount= 0;
          }
        }
        if(nominalInput>0){
          this.left=nominalInput;
        }
      },
      refresh(){
        this.left=0;
        for(var i=0; i<this.fractions.length;i++){
          this.fractions[i].amount=0;
        }
      }
    }
  }
</script>
<style scoped>
  #logo{
    padding: 20px;
  }
  #logo-tokopedia{
    height: 50px;
  }
  #input-section{
    background-color: #26dc46;
    padding: 40px 40px;
  }
  .inputs{
    border-radius: 16px;
    width: 400px;
    padding: 10px 10px;
    font-size: 18px;
    text-align: center;

  }
  .inputs:focus{
    border: 2px solid #26dc46;
    outline: none !important;
    color: #26dc46;
  }
  .input-style{
    border: 2px solid #e0e0e0;
    color: #26dc46;
  }
  .input-style-invalid{
    border: 2px solid #e74c3c;
    color: #e74c3c;
  }
  ::placeholder {
    color: #e0e0e0;
  }
  .submit-button{
    margin-top: 16px;
    background-color:#1dbc60;
    color: #ffffff;
    cursor: pointer;
    padding: 10px 10px;
    width: 150px;
    border-radius: 16px;
    font-size: 18px;
  }
  .submit-button:hover{
    background-color:#52ff0d;
  }
  #results-section{
    padding: 40px 40px;
    background-color: #ffffff;
    height: 100%;
  }
  h3{
    color:#26dc46;
    border-bottom: 2px solid #26dc46;
    width: 100px;
  }
  table{
    width:300px;

  }
  td,th{
    padding: 8px 16px;
    width: 150px;
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
