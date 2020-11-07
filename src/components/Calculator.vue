<template>
  <div class="calculator">
    <Display :value="displayValue" />
    <Button label="AC" @onClick="clearMemory" triple />
    <Button label="/" @onClick="setOperation" operation />
    <Button label="7" @onClick="addDigit" />
    <Button label="8" @onClick="addDigit" />
    <Button label="9" @onClick="addDigit" />
    <Button label="*" @onClick="setOperation" operation />
    <Button label="4" @onClick="addDigit" />
    <Button label="5" @onClick="addDigit" />
    <Button label="6" @onClick="addDigit" />
    <Button label="-" @onClick="setOperation" operation />
    <Button label="1" @onClick="addDigit" />
    <Button label="2" @onClick="addDigit" />
    <Button label="3" @onClick="addDigit" />
    <Button label="+" @onClick="setOperation" operation />
    <Button label="0" @onClick="addDigit" double />
    <Button label="." @onClick="addDigit" />
    <Button label="=" @onClick="setOperation" operation />
  </div>
</template>

<script>
  import Button from './Button.vue'
  import Display from './Display.vue'

  export default {
    name: 'Calculator',
    components: { Button, Display },
    data: function(){
      return {
        displayValue: '0',
        clearDisplay: false,
        operation: null,
        values: [0, 0],
        current: 0
      }
    },
    methods: {
      clearMemory(){
        // retorna ao estado inicial
        Object.assign(this.$data, this.$options.data());
      },
      addDigit(digit){
        // não permite usar dois pontos
        if(digit === '.' && this.displayValue.includes('.')){
          return;
        }
        // limpa o display se tiver apenas zero ou antes de inserir um novo número
        const clearDisplay = this.displayValue === '0' || this.clearDisplay;
        // mostra o novo valor
        const currentValue = clearDisplay ? '' : this.displayValue;
        const displayValue = currentValue + digit;

        this.displayValue = displayValue;
        this.clearDisplay = false;
        // guarda o valor mostrado em this.values
        if(digit !== '.'){
          const i = this.current;
          const newValue = parseFloat(displayValue);
          // const values = [...this.values];
          this.values[i] = newValue;
          
          // this.values = values;
        }
      },
      setOperation(operation){
        if(this.current === 0){
          // guarda a operação e prepara para digitar o segundo valor
          this.operation = operation;
          this.current = 1;
          this.clearDisplay = true;
        }else{
          // calcula o resultado
          const equals = operation === '=';
          const currentOperation = this.operation;
          
          // const values = [...this.values];
          this.values[0] = this.execOperation(this.values, currentOperation);
          this.values[1] = 0;

          this.displayValue = this.values[0];
          this.operation = equals ? null : operation;
          this.current = equals ? 0 : 1;
          this.clearDisplay = !equals;
          // this.values = values;
        }
      },
      execOperation(values, operation){
        switch(operation){
          case '+':
            return values[0] + values[1];
          case '-':
            return values[0] - values[1];
          case '*':
            return values[0] * values[1];
          case '/':
            return values[0] / values[1];
          default:
            return values[0];
        }
      }
    }
  }
</script>

<style>
.calculator {
  height: 320px;
  width: 235px;
  border-radius: 5px;
  overflow: hidden;

  display: grid;
  grid-template-columns: repeat(4, 25%);
  grid-template-rows: 1fr repeat(5, 48px);
}
</style>