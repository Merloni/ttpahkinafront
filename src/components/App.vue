<template>
  <div id="app">
    <div class="banner">
      <h1>Terveystalopähkinä 123</h1>
    </div>
    <div class="bottom">
      <label for="primeresult">Onko alkuluku?</label>
      <div class="resultbox">
        <p id="primeresult"></p>
      </div>
      <label for="sumresult">Summa:</label>
      <div class="resultbox">
        <p id="sumresult"></p>
      </div>
      <label for="numberinputs">Kirjoita halutut numerot pilkulla erotellen tai yksi numero jos haluat selvittää vain alkuluvun.</label><br>
      <input type="text" id="numberinputs" placeholder="1,223,341"></input>
      <input type="submit" class="button" value="Lähetä" v-on:click="sendData()"></input>

    </div>
  </div>
</template>

<script>
import axios from 'axios';

  export default {
    name: 'app',
    methods: {
      sendData(){
        var self = this;
        var numbers = document.getElementById("numberinputs").value.split(",");

        if(numbers.length == 1){
          if(numbers == ""){
            console.log("Ei parametreja");
            return;
          }
          axios.get('http://localhost:3000/api/checkprime', {
            params: {
              number: numbers
            }
          })
          .then(function(response){
            console.log("Success");
            self.setResults(response.data);
          })
          .catch(function (error) {
            console.log("Error: ", error);
          });
        }else{
          axios.get('http://localhost:3000/api/add', {
            params: {
              numbers: document.getElementById("numberinputs").value
            }
          })
          .then(function(response){
            console.log("Success");
            self.setResults(response.data);
          })
          .catch(function (error) {
            console.log("Error: ", error);
          });
        }
      },
      setResults(response){
        var sumresult = document.getElementById("sumresult");
        var primeresult = document.getElementById("primeresult");

        sumresult.innerHTML = response.result ? response.result : "";
        primeresult.innerHTML = response.isPrime ? "On alkuluku" : "Ei ole alkuluku";

      }
    }

  }

</script>

<!-- CSS libraries -->
<style src="normalize.css/normalize.css"></style>

<!-- Global CSS -->
<style>
  code {
    font-family: Menlo, Monaco, Lucida Console, Liberation Mono, DejaVu Sans Mono, Bitstream Vera Sans Mono, Courier New, monospace, serif;
    font-size: 0.9em;
    white-space: pre-wrap;
    color: #2c3e50;
  }


</style>

<!-- Scoped component css -->
<!-- It only affect current component -->
<style scoped>
  #app {
    text-align: center;
  }

  #app h1 {
    color: #2c3e50;
    font-weight: 300;
    margin: 0;
  }

  .banner {
    height: 200px;
    background-color: #f6f6f6;
    padding: 50px 10px;
  }

  .bottom {
    padding: 80px 10px;
    font-size: 24px;
    font-weight: 300;
  }

  .resultbox{
    background-color: RGBA(0,0,0,0.1);
    border: 1px solid black;
    border-radius: 2px;
    min-height: 20px;
    width: 20%;
    height: 50px;
    min-width: 200px;
    margin: 10px auto;
    padding: 5px;
  }

  .button{
    margin: 10px 5px;
    width: 100px;
    font-size : 20px;

  }

  #primeresult, #sumresult{

  }
</style>
