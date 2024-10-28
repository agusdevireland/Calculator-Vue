<template>
    <div class="flex justify-center items-center min-h-screen">
      <div class="bg-white py-8 px-8 mt-14 rounded-xl shadow-lg">
  
        <div>
          <input 
            type="text" 
            :value="display"
            readonly
            class="bg-black text-white p-4 mb-8 rounded-sm w-full input" 
          />
        </div>
  
        <div class="grid grid-cols-4 gap-4">
          <!-- Numbers -->
          <div class="grid grid-cols-3 gap-4 col-span-3">
            <button v-for="n in [9, 8, 7, 6, 5, 4, 3, 2, 1]" 
                    class="bg-indigo-500 text-white p-4 rounded-sm border border-black"
                    @click="typeNumber(n)">
              {{ n }}
            </button>
            <button class="bg-indigo-500 text-white p-4 rounded-sm border border-black"
                     @click="typeNumber(0)">0</button>
            <button class="bg-indigo-500 text-white p-4 rounded-sm border border-black"
                     @click="typeNumber('.')">.</button>
            <button class="bg-indigo-500 text-white p-4 rounded-sm border border-black"
                     @click="performCalculation">=</button>
          </div>
  
          <!-- Operators Column -->
          <div class="flex flex-col gap-4">
            <button class="bg-indigo-500 text-white p-4 rounded-sm border border-black"
                    @click="typeOperator('/')">/</button>
            <button class="bg-indigo-500 text-white p-4 rounded-sm border border-black"
                    @click="typeOperator('x')">x</button>
            <button class="bg-indigo-500 text-white p-4 rounded-sm border border-black"
                    @click="typeOperator('-')">-</button>
            <button class="bg-indigo-500 text-white p-4 rounded-sm border border-black"
                    @click="typeOperator('+')">+</button>
          </div>
        </div>
  
        <div class="flex flex-col mt-4">
          <button class="bg-indigo-500 text-white p-2 rounded-sm border border-black"
                  @click="clear">Clear</button> 
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        display: '0', 
        currentNumber: '', 
        operator: null, 
        previousNumber: null 
      };
    },
    methods: {
      typeNumber(n) {
        if (this.currentNumber === '0') {
          this.currentNumber = '';
        }
        this.currentNumber += n;
        this.display = this.currentNumber;                      
      },
      typeOperator(operator) {
        if (this.currentNumber !== '') {
          this.previousNumber = this.currentNumber; 
          this.operator = operator; 
          this.currentNumber = ''; 
          this.display = `${this.previousNumber} ${operator}`; 
        }
      },
      performCalculation() {
        if (this.operator !== null && this.currentNumber !== '') {
          this.calculate();
        } else {
          console.log('Cannot perform calculation: Operator or current number is missing');
        }
      },
      calculate() {
        const previous = parseFloat(this.previousNumber);
        const current = parseFloat(this.currentNumber);
        let result = '';
  
        if (isNaN(previous) || isNaN(current)) {
          console.log('Invalid calculation parameters');
          return;
        }
  
        switch (this.operator) {
          case "+":
            result = previous + current;
            break;
          case "-":
            result = previous - current;
            break;
          case "x":
            result = previous * current;
            break;
          case "/":
            if (current === 0) {
              this.display = 'Error'; 
              this.currentNumber = '';
              return;
            }
            result = previous / current;
            break;
          default:
            return;
        }
  
        this.currentNumber = result.toString();
        this.display = result.toString(); 
        this.previousNumber = null; 
        this.operator = null; 
      },
      clear() {
        this.currentNumber = '';
        this.operator = null;
        this.previousNumber = null;
        this.display = '0'; 
      },
    }
  }
  </script>
  
  <style scoped>
  .input {
    border: thick solid grey;
    border-radius: 10px;
    padding: 15px;
  }
  </style>
  