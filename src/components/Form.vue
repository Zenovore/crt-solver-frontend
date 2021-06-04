<template>
    <form>
      <h1>Chinese Remainder Theorem Solver</h1>
      <label>Koefisien X :</label>
      <input type="number" required v-model="xConstant">
      <label>Remainder :</label>
      <input type="number" required v-model="remainder">
      <label>Modulo :</label>
      <input type="number" required v-model="modulo">
      <div class="buttonStyle">
        <Button @click="addEquation()" text ="Add Equation"></Button>
        <Button @click="getAnswer()" text ="Get Answer"></Button>
      </div>
      <div v-for="number in numberArray" :key="number" class="equation">
        <h2>
          {{number[0]}} x ≡ {{number[1]}} mod {{number[2]}}
          <i @click="deleteEquation(number)" class="fas fa-times"></i>
        </h2>
      </div>
    <!-- <h1 :key="answer" v-if="answer">Jawaban x = {{answer[1]}}</h1> -->
    <!-- <div v-if="answer != '' && answer.length != 2 "> -->
    <div v-if="status && answer.length != 0">
      <h2>Setelah Disederhanakan Didapat Persamaan Modulo</h2>
      <h3 v-for="ans in answer[0]" :key="ans">x ≡ {{ans[1]}} mod {{ans[2]}}</h3>
      <h2>Didapat m = {{answer[5]}}</h2>
      <h2 v-for="(ans,index) in answer[2]" :key="ans">M {{index}} = {{answer[1][index]}}</h2>
      <h2>Dari data tersebut didapat y</h2>
      <h2 v-for="(ans,index) in answer[2]" :key="ans">y {{index}} = {{answer[2][index]}} karena {{answer[1][index]}}*{{answer[2][index]}} ≡ 1 mod {{answer[3][index]}}</h2>
      <h2>Sehingga didapat solusi unik dari sistem kekongruenan tersebut adalah</h2>
      <h2>{{test()}}</h2>
      <h2>x = {{answer[6]}}</h2>
      <h2>x ≡ {{answer[6] % answer[5]}} mod {{answer[5]}} </h2>
    </div>
    <div v-else-if="!status">
      <h1>Tidak ada Jawaban</h1>
    </div>
    </form>
</template>

<script>
import Button from "./Button.vue"

export default {
  components: {
    Button
  },
  data(){
    return{
      xConstant : '1',
      remainder : '',
      modulo : '',
      numberArray : [[2,6,11],[3,4,7],[4,1,5]],
      answer : '',
      status: true
    }
  },
  methods:{
    test(){
      let result = 'x ='
      for (let i=0; i<this.answer[0].length;i++){
        result += `${this.answer[4][i]}*${this.answer[1][i]}*${this.answer[2][i]}`
        if (i+1 != this.answer[0].length){
          result += ` + `
        }
      }
      return result
    },
    addEquation(){
      let result = []
      if (this.remainder-0 >= this.modulo-0){
        alert("Persamaan modulo salah")
      }
      else {
        result.push(this.xConstant)
        result.push(this.remainder)
        result.push(this.modulo)
        this.numberArray.push(result)
        this.xConstant = '1'
        this.remainder = ''
        this.modulo = ''
        this.answer =''
        this.status = true
      }
    },
    async getAnswer(){
      const request = new Request("https://crt-solver-backend.azurewebsites.net/",{
        // const request = new Request("http://localhost:5000/",{
          method: "POST",
          mode: "cors",
          cache: "default",
          body: JSON.stringify(this.numberArray),
          // headers:{
          //   'content-type': 'application/json'
          // }
      });
      const res = await fetch(request);
      res.text().then((text) => {
        console.log(text) ; 
        this.answer = JSON.parse(text);
        if (this.answer.length == 0){
          this.status = false
        }
      });
      this.numberArray = []
    },
    checkSame(a,b){
      if (a.length === b.length){
        
        for(let i =0;i<a.length;i++){
          if (a[i] != b[i]){
            return false
          }
        }
        return true
      }
      return false
    },
    deleteEquation(number){
      for(let i =0;i<this.numberArray.length;i++){
        if (this.checkSame(this.numberArray[i],number)){
          this.numberArray.splice(i,1)
        }
      }
    },
    
  }
}
</script>

<style>
  h2{
    font-weight: 800;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  .fas{
    color: red;
  }

  .deleteButton{
    background-color: red;
    padding: 0;
    margin: 0;
  }
  .buttonStyle{
    text-align: center;
  }
  .equation{
    font-family:'Courier New', Courier, monospace;
    font-size: 24px;
    justify-content: space-between;
  }
  h1{
    text-align: center;
  }
  form {
    max-width: 420px;
    margin: 30px auto;
    background: white;
    text-align: left;
    padding: 40px;
    border-radius: 10px;
  }
  label {
    color: #aaa;
    display: inline-block;
    margin: 25px 0 15px;
    font-size: 0.6em;
    text-transform: uppercase;
    letter-spacing: 1px;
    font-weight: bold;
  }
  input, select {
    display: block;
    padding: 10px 6px;
    width: 100%;
    box-sizing: border-box;
    border: none;
    border-bottom: 1px solid #ddd;
    color: #555;
  }
  input[type="checkbox"] {
    display: inline-block;
    width: 16px;
    margin: 0 10px 0 0;
    position: relative;
    top: 2px;
  }
  .pill {
    display: inline-block;
    margin: 20px 10px 0 0;
    padding: 6px 12px;
    background: #eee;
    border-radius: 20px;
    font-size: 12px;
    letter-spacing: 1px;
    font-weight: bold;
    color: #777;
    cursor: pointer;
  }
  button {
    background: #0b6dff;
    border: 0;
    padding: 10px 20px;
    margin-top: 30px;
    color: white;
    border-radius: 20px;
  }
  .submit {
    text-align: center;
  }
  .error {
    color: #ff0062;
    margin-top: 10px;
    font-size: 0.8em;
    font-weight: bold;
  }
</style>