<template>
  <div id="input-nominal">
    <div id="input-section">
      <div id="logo">
        <img src="./../assets/tokopedia-white-logo.png" id="logo-tokopedia">
      </div>
      <input @keyup.enter="inputValidation()" v-on:click="refresh()" v-if="invalid==false" class="inputs input-style" type="text" v-model="message" placeholder="Ketikkan nominal uang...">
      <input @keyup.enter="inputValidation()" v-on:click="refresh()" v-if="invalid==true" class="inputs input-style-invalid" type="text" v-model="message" placeholder="Ketikkan nominal uang...">
      <div class="submit-button" v-on:click="inputValidation()">Submit</div>
    </div>
    <div id="invalid-section" v-if="invalid==true">
      <font>Invalid Input Format!</font>
    </div>
    <div id="results-section">
      <div id="waiting" v-if="results==false">
        <div class="mascot">
          <img src="./../assets/Tokopedia_Mascot.png" class="mascot-tokopedia bounce">
        </div>
        <div class="note" v-if="message==null">
          <font class="note-title"><b>Halo!</b></font><br/>
          <font>Silahkan masukkan nominal uang</font>
        </div>
        <div class="note" v-if="message!=null&&invalid==false">
          <font>Tekan <b>Enter</b> atau klik tombol <b>Submit</b> untuk melihat hasil.</font>
        </div>
        <div class="note" v-if="invalid==true">
          <font class="note-title"><b>Ups!</b></font><br/>
          <font>Ada <b>kesalahan input</b>, periksa kembali</font>
        </div>
      </div>
      <div id="details" class="fade-in" v-if="results==true">
        <h3>Results</h3>
        <div class="table-results">
          <table>
            <tr>
              <th colspan="2" class="cell-header"></th>
              <th class="cell-header">Fraction</th>
              <th class="cell-header">Amount</th>
            </tr>
            <tr v-for="fr in fractions" v-if="fr.amount>0">
              <td colspan="2">Rp</td>
              <td class="cell">{{fr.nominal}}</td>
              <td class="cell">{{fr.amount}}</td>
            </tr>
          </table>
          <div id="left" v-if="left>0">
            <font>Left: Rp {{left}} (no available fraction)</font>
          </div>
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
        results:false,
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
        this.results=true;
      },
      refresh(){
        this.left=0;
        this.results=false;
        this.invalid=false;
        for(var i=0; i<this.fractions.length;i++){
          this.fractions[i].amount=0;
        }
      }
    }
  }
</script>
<style scoped>
  #input-section{
    background-image: linear-gradient(to bottom right,#1dbc60,#26dc46,#26dc46,#52ff0d);
    padding: 20px;
    position: fixed;
    margin: auto;
    top:0;
    left: 0;
    right: 0;
  }
  #logo{
    padding: 20px;
  }
  #logo-tokopedia{
    height: 50px;
  }
  .inputs{
    border-radius: 16px;
    width: 250px;
    padding: 10px 10px;
    font-size: 18px;
    font-weight: bold;
    text-align: center;
  }
  .inputs:focus{
    border: 2px solid #52ff0d;
    outline: none !important;
    color: #26dc46;
  }
  .input-style{
    border: 2px solid #e0e0e0;
    color: #26dc46;
  }
  .input-style-invalid{
    border: 2px solid #d64541;
    color: #d64541;
  }
  ::placeholder {
    color: #e0e0e0;
  }
  .submit-button{
    margin-top: 16px;
    background-color:#ffcd02;
    color: #555555;
    cursor: pointer;
    padding: 10px 10px;
    width: 100px;
    border-radius: 16px;
    font-size: 16px;
    font-weight: bold;
  }
  .submit-button:hover{
    background-color:#ffa800;
  }
  #invalid-section{
    background-color:#d64541;
    color:#ffffff;
    padding: 10px;
    position: fixed;
    margin: auto;
    margin-top: -20px;
    left: 0;
    right: 0;
    font-size: 12px;
  }
  #results-section{
    margin-top: 250px;
    background-color: #ffffff;
    height: 100%;
    overflow: auto;
    padding:18px;
  }
  .mascot{
    padding-top: 60px;
    position: fixed;
    left: 0;
    right: 0;
    margin: auto;
  }
  .mascot-tokopedia{
    height: 100px;
    align-self: flex-end;
    animation-duration: 2s;
    animation-iteration-count: infinite;
    margin: 0 auto 0 auto;
    transform-origin: bottom;
  }
  .bounce{
    animation-name: bounce;
    animation-timing-function: cubic-bezier(0.280, 0.840, 0.420, 1);
  }
  @keyframes bounce {
    0%   { transform: scale(1,1)      translateY(0); }
    10%  { transform: scale(1.1,.9)   translateY(0); }
    25%  { transform: scale(.9,1)   translateY(-60px); }
    56%  { transform: scale(1,.65) translateY(0); }
    64%  { transform: scale(1,1)      translateY(-3px); }
    73%  { transform: scale(1,1)      translateY(0); }
    80%  { transform: scale(1,1)      translateY(0); }
    100% { transform: scale(1,1)      translateY(0); }
  }
  .note{
    color:#555555;
    margin-top: 160px;
    font-size: 14px;
    position: fixed;
    left: 0;
    right: 0;
  }
  .note-title{
    font-size: 30px;
    font-weight: bold;
  }
  h3{
    color:#555555;
    border-bottom: 3px solid #26dc46;
    font-size: 26px;
    width: 100px;
  }
  table{
    width:350px;
    color: #555555;
  }
  td,th{
    padding: 4px 16px;
    width: 175px;
    text-align: center;
    font-weight: bold;
  }
  .cell-header{
    color: #26dc46;
    font-size: 12px;
    padding-bottom: 4px;
    border: none;
  }
  .cell{
    font-size: 14px;
  }
  #left{
    margin-top: 16px;
    background-color: #26dc46;
    color: #ffffff;
    padding: 4px;
    border-radius: 8px;
    width: 250px;
    font-weight: bold;
    font-size: 14px
  }
  .fade-in {
    animation: fadeIn ease 1s;
    -webkit-animation: fadeIn ease 1s;
  }
  @keyframes fadeIn{
    0% {
      opacity:0;
    }
    100% {
      opacity:1;
    }
  }
</style>
