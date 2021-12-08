<template>           
            <div id="app" class="bg-dark">
              <div class="d-flex justify-content-center align-items-center vh-100">
                <div class="col-sm-5 col-md-4 col-lg-3 border border-dark border-5 p-5 bg-skyblue">
                  <form>
                      <div>
                        <input class="form-control text-right bg-blue text-white" type="text" :value="input" />
                      </div>
                      <div>
                      <input class="form-control text-right bg-blue text-white" type="text" :value="total"/>
                      </div>
                  </form>
                  <div class="d-flex flex-column">
                    <div class="d-flex pt-2 justify-content-end">
                        <button @click="allClear" class="btn btn-outline-dark w-30 bg-clear">AC</button>                       
                    </div>
                    <div class="d-flex flex-row pt-2 justify-content-between">
                        <button @click="inputNum('7')" class="btn btn-outline-dark w-25">7</button>
                        <button @click="inputNum('8')" class="btn btn-outline-dark w-25">8</button>
                        <button @click="inputNum('9')" class="btn btn-outline-dark w-25">9</button>
                        <button @click="inputOp('*')" class="btn btn-outline-dark w-25">*</button>
                    </div>
                    <div class="d-flex flex-row pt-2 justify-content-between">
                        <button @click="inputNum('4')" class="btn btn-outline-dark w-25">4</button>
                        <button @click="inputNum('5')" class="btn btn-outline-dark w-25">5</button>
                        <button @click="inputNum('6')" class="btn btn-outline-dark w-25">6</button>
                        <button @click="inputOp('-')" class="btn btn-outline-dark w-25">-</button>
                    </div>
                    <div class="d-flex flex-row pt-2 justify-content-between">
                        <button @click="inputNum('3')" class="btn btn-outline-dark w-25">3</button>
                        <button @click="inputNum('2')" class="btn btn-outline-dark w-25">2</button>
                        <button @click="inputNum('1')" class="btn btn-outline-dark w-25">1</button>
                        <button @click="inputOp('+')" class="btn btn-outline-dark w-25">+</button>
                    </div>
                    <div class="d-flex flex-row pt-2 justify-content-between">
                        <button @click="inputNum('0')" class="btn btn-outline-dark w-25">0</button>
                        <button @click="inputNum('00')" class="btn btn-outline-dark w-25">00</button> 
                         <button @click="oneClear" class="btn btn-outline-dark w-25">←</button>
                        <button @click="inputOp('/')" class="btn btn-outline-dark w-25">/</button>
                    </div>
                    <div class="d-flex flex-row pt-2 justify-content-end">
                         <button @click="calculation" class="btn btn-outline-dark w-25">=</button>
                    </div>
                  </div>  
                </div>
              </div>  
            </div>  
</template>

<script>
export default {
  name: 'calculator',
  data(){
    return{
      input: "",
      total: "",
    }
  },
  methods: {
    inputNum(num){
      if(this.input === "" && num === '0' || this.input === "" && num == '00')return;
      this.input += num;
    },
    inputOp(op){
      if(this.input === "" && op != "-")return;
      else if(this.input[this.input.length - 1] === " ")this.input = this.input.substring(0, this.input.length - 2) + op + " ";
      else this.input += " " + op + " ";
    },
    calculation(){
      if(this.input === "")return;
      this.total = this.expressionParser(this.input);
      this.input = "";
    },
    expressionParser(expression){
      let numbers = [];
      let ops = [];
      expression = expression.replace(" ", "");
      if(isNaN(expression[expression.length - 2]) && expression.length != 1){
        expression += "0";
      }
      for(let i = 0;i < expression.length;i++){
        if(isNaN(expression[i])){
          let currOp = expression[i];
          while(ops.length > 0 && this.getPriority(currOp) <= this.getPriority(ops[ops.length - 1])){
            this.process(numbers, ops[ops.length - 1]);
            ops.pop();
          }
          ops.push(currOp);
        }
        else{
          let number = "";
          while(i < expression.length && !isNaN(expression[i])){
            number += expression[i];
            i++;
          }
          i--;
          numbers.push(number);
        }
      }

        while(ops.length > 0){
          this.process(numbers, ops[ops.length - 1]);
          ops.pop();
        }
        return numbers[0];
    },
    process(stack, op){
      
      let right = parseInt(stack.pop());
      let left = parseInt(stack.pop());
      let value = 0;

      switch(op){
        case '+': value = left + right;break;
        case '-': value = left - right;break;
        case '*': value = left * right;break;
        case '/': value = Math.floor(left / right);break;
      }
      console.log(value);
      stack.push(value);
    },
    getPriority(op){
      if(op === "+" || op === "-")return 1;
      else if(op === "*" || op === "/")return 2;
    },
    allClear(){
      this.input = "";
      this.total = "";
    },
    oneClear(){
      if(this.input[this.input.length - 1] == " ")this.input = this.input.substring(0, this.input.length - 2);
      if(!isNaN(this.input[this.input.length - 1]))this.input = this.input.substring(0, this.input.length - 1);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.w-25{
  width: 25%;
}
.bg-skyblue{
  background: rgb(22, 139, 185);
}
.bg-blue{
  background-color: rgb(5, 112, 155);
}
.bg-clear{
  background: rgb(32, 194, 113);
}
</style>
