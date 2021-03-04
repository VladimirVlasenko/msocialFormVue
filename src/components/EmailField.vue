<template>
  <div class="email inputField">
    <label for="email">Электронная почта</label>
    <div class="input_wrapper">
      <input type="text" 
      autocomplete="off"
      id="email" 
      name="email"
      form="form"
      placeholder="Введите адрес почты" 
      v-model.trim="value"
      @input="validateField" 
      @blur="validateContent">
      <div class="error_message">{{ errorMessage }}</div>
    </div>
    <div class="description">Латинские буквы, цифры и символы "@", "-", "_", "."</div>
  </div>
</template>

<script>
export default {
  name: 'InputEmail',
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
      let emailField = document.querySelector('.email');
      let inputemail = emailField.querySelector('#email');
      let errorMessageNode = emailField.querySelector('.error_message');
      let emailInputArr = this.value.split('');
      // Проверяем первое условие
      for(let i=0;i<emailInputArr.length; i++) {
        if(emailInputArr[i].match(/[A-z 0-9 _ - @ .]/i) == null) {
          emailInputArr.pop();
          let newemailInput = emailInputArr.join('');
          this.value = newemailInput;
          inputemail.classList.add('wrong');
          this.errorMessage = "Введите email вида xxx@xxx.xx"
          errorMessageNode.classList.add('visible');
          this.validContent = false;
          
        }
        else {
          errorMessageNode.classList.remove('visible');
          this.validContent = true;
          this.errorMessage = ""
          inputemail.classList.remove('wrong');
        }
      }
    },
    validateContent(){
      let emailField = document.querySelector('.email');
      let inputemail = emailField.querySelector('#email');
      let errorMessageNode = emailField.querySelector('.error_message');
  
      // Проверяем первое условие
        if(this.value.match(/^([a-z0-9_-]+\.)*[a-z0-9_-]+@[a-z0-9_-]+(\.[a-z0-9_-]+)*\.[a-z]{2,6}$/) == null) {
          inputemail.classList.add('wrong');
          this.errorMessage = "Email должен выглядеть так xxxx@xxxx.xxx"
          errorMessageNode.classList.add('visible');
          this.validContent = false;
          this.isValid = false;
          this.$emit("validateField", this.isValid)
        }
        else {
          errorMessageNode.classList.remove('visible');
          this.validContent = true;
          this.errorMessage = "";
          this.isValid = true;
          inputemail.classList.remove('wrong');
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
