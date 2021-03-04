<template>
  <div class="agree inputField">
    <label for="agree">Я согласен</label>
    <div class="input_wrapper">
      <input type="checkbox" 
      id="agree" 
      name="agree"
      form="form"
      placeholder="Введите имя" 
      v-model.trim="value"
       @click="checkPushed">
      <label for="agree" class="checkbox">Принимать актуальную информацию на емейл</label>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Inputagree',
  data(){
    return {
      isValid: false,
      value: '',
      errorMessage: '',
      validContent: false,
      longEnough: false
    }
  },
  methods: {
    checkPushed(){
      let agreeField = document.querySelector('.agree');
      let inputAgree = agreeField.querySelector('#agree');
      if(inputAgree.checked !== true) {
        this.isValid = false;
        this.$emit("validateField", this.isValid)
      } else {
        this.isValid = true;
        this.$emit("validateField", this.isValid)
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.inputField {
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  align-items: center;
  margin-bottom: 40px;
  label {
    font-family: Arial;
    font-style: normal;
    font-weight: normal;
    font-size: 21px;
    line-height: 19px;
    width: 224px;
    text-align: left;
    span {
      color: #FF0000;
    }
  }
  .input_wrapper {
    position: relative;
    input {
      display: none;
      width: 335px;
      height: 15px;
      border: 1px solid #999999;
      box-sizing: border-box;
      font-style: normal;
      font-weight: normal;
      font-size: 16px;
      line-height: 19px;
      padding: 0 12px;
      margin-right: 18px;
      background-color: #037BB8;
      &.wrong {
        border: 1px solid #FF0000;
      }
    }
    label {
      display: block;
      width: 375px;
      height: 20px;
      background: url('../assets/img/checkbox.svg') center left no-repeat;
      font-family: 'Arial', sans-serif;
      font-style: normal;
      font-weight: normal;
      font-size: 16px;
      line-height: 19px;
      color: #000000;
      padding-left: 15px;
      margin-right: 18px;
      box-sizing: border-box;
    }
    input:checked+label {
      background: url('../assets/img/checkboxChecked.svg') center left no-repeat;
    }
    .error_message {
      position: absolute;
      bottom:-23px;
      left: 0px;
      font-size: 12px;
      line-height: 20px;
      color: #FF0000;
      visibility: hidden;
      &.visible {
        visibility: visible;
      }
    }

  }
  
  .description {
    font-family: Arial;
    font-style: normal;
    font-weight: normal;
    font-size: 12px;
    line-height: 20px;
    color: #999999;
  }
}
</style>
