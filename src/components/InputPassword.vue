<template>
  <div class="password inputField">
    <label for="password">Пароль<span>*</span></label>
    <div class="input_wrapper">
      <input type="password" id="password"
      autocomplete="off"
      name="password"
      placeholder="Введите пароль" 
      form="form"
      v-model="value" 
      @input="validateField" 
      @blur="checkLength">
      <div class="error_message">{{ errorMessage }}</div>
    </div>
    <div class="description">Должен содержать не менее 6 символов и только латинские буквы</div>
  </div>
</template>

<script>
export default {
  name: 'InputPassword',
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
      let passwordField = document.querySelector('.password');
      let inputpassword = passwordField.querySelector('#password');
      let errorMessageNode = passwordField.querySelector('.error_message');
      let passwordInputArr = this.value.split('');
      // Проверяем первое условие
      for(let i=0;i<passwordInputArr.length; i++) {
        if(passwordInputArr[i].match(/[A-z]/i) == null) {
          passwordInputArr.pop();
          let newpasswordInput = passwordInputArr.join('');
          this.value = newpasswordInput;
          inputpassword.classList.add('wrong');
          this.errorMessage = "Пароль может состоять из латинских букв"
          errorMessageNode.classList.add('visible');
          this.validContent = false;
        }
        else {
          errorMessageNode.classList.remove('visible');
          this.validContent = true;
          this.errorMessage = "Пароль должен состоять из латинских букв"
          inputpassword.classList.remove('wrong');
        }
      }
// Проверяем второе условие
    },
    checkLength(){
      let passwordField = document.querySelector('.password');
      let inputpassword = passwordField.querySelector('#password');
      let errorMessageNode = passwordField.querySelector('.error_message');
      if(this.value.length < 6) {
        inputpassword.classList.add('wrong');
        this.errorMessage="Слишком короткий пароль"
        errorMessageNode.classList.add('visible');
        this.isValid = false;
        this.$emit("validateField", this.isValid, this.value)
      } else {
        inputpassword.classList.remove('wrong');
        this.errorMessage=""
        errorMessageNode.classList.remove('visible');
        this.longEnough = true;
        this.isValid = true;
        this.$emit("validateField", this.isValid, this.value)
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
