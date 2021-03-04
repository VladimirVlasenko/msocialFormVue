<template>
  <div class="phone inputField">
    <label for="phone">Номер телефона</label>
    <div class="input_wrapper">
      <input type="text" 
      autocomplete="off"
      id="phone" 
      name="phone"
      form="form"
      placeholder="+7(xxx)xxx-xx-xx" 
      v-model.trim="value"
       @blur="validateField" 
       @input="maskPhone"
       @focus="focusInput"
      >
      <div class="error_message">{{ errorMessage }}</div>
    </div>
    <div class="description">Маска с международным форматом “+7(999)999-99-99”.</div>
  </div>
</template>

<script>
export default {
  name: 'InputPhone',
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
    validateField(){
      let phoneField = document.querySelector('.phone');
      let inputphone = phoneField.querySelector('#phone');
      let errorMessageNode = phoneField.querySelector('.error_message');
      let phoneInputArr = this.value.split('');
      // Проверяем первое условие
      for(let i=0;i<phoneInputArr.length; i++) {
        if(phoneInputArr[i].match(/[0-9+()-]/) == null) {
          inputphone.classList.add('wrong');
          this.errorMessage = 'Должен содержать только цифры и знаки "+", "-", "(", ")" ';
          errorMessageNode.classList.add('visible');
          this.validContent = false;
        }
        else {
          errorMessageNode.classList.remove('visible');
          this.validContent = true;
          this.errorMessage = ""
          inputphone.classList.remove('wrong');
          this.$emit("validateField", this.isValid)
        }
      }
    },
    maskPhone(){
      let phoneField = document.querySelector('.phone');
      let inputphone = phoneField.querySelector('#phone');
      let errorMessageNode = phoneField.querySelector('.error_message');
      let phoneInputArr = this.value.split('');
      console.log(phoneInputArr)
      for(let i=0;i<phoneInputArr.length; i++) {
        if(phoneInputArr[i].match(/[0-9+()-]/) == null) {
          inputphone.classList.add('wrong');
          this.errorMessage = 'Должен содержать только цифры и знаки "+", "-", "(", ")" ';
          errorMessageNode.classList.add('visible');
          this.validContent = false;
          phoneInputArr.pop("-")
          this.value = phoneInputArr.join("");
        }
        if(i == phoneInputArr.length-1 && i == 5) {
          phoneInputArr.push(")")
          console.log("Условие выполнено")
          this.value = phoneInputArr.join("");
          console.log(this.value)
        }
        if(i == phoneInputArr.length-1 && i == 9) {
          phoneInputArr.push("-")
          console.log("Условие выполнено")
          this.value = phoneInputArr.join("");
          console.log(this.value)
        }
        if(i == phoneInputArr.length-1 && i == 12) {
          phoneInputArr.push("-")
          console.log("Условие выполнено")
          this.value = phoneInputArr.join("");
          console.log(this.value)
        }
        if(i== phoneInputArr.length-1 && i >= 16) {
          phoneInputArr.pop("-")
          this.value = phoneInputArr.join("");
        }
      }
      
    },
    focusInput(){
      let phoneField = document.querySelector('.phone');
      let inputPhone = phoneField.querySelector('#phone');
      inputPhone.value = "+7("
    },


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
      width: 335px;
      height: 31px;
      border: 1px solid #999999;
      box-sizing: border-box;
      font-style: normal;
      font-weight: normal;
      font-size: 16px;
      line-height: 19px;
      padding: 0 12px;
      margin-right: 18px;
      &.wrong {
        border: 1px solid #FF0000;
      }
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
