<template>
  <div class="surname inputField">
    <label for="surname">Фамилия<span>*</span></label>
    <div class="input_wrapper">
      <input type="text" id="surname" placeholder="Введите фамилию" 
      autocomplete="off"
      form="form"
      name="surname"
      v-model.trim="value" 
      @input="validateField" 
      @blur="checkLength">
      <div class="error_message">{{ errorMessage }}</div>
    </div>
    <div class="description">Должен содержать не менее 2 символов и только кириллицу</div>
  </div>
</template>

<script>
export default {
  name: 'Inputsurname',
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
      let surnameField = document.querySelector('.surname');
      let inputsurname = surnameField.querySelector('#surname');
      let errorMessageNode = surnameField.querySelector('.error_message');
      let surnameInputArr = this.value.split('');
      // Проверяем первое условие
      for(let i=0;i<surnameInputArr.length; i++) {
        if(surnameInputArr[i].match(/[А-я -]/i) == null) {
          surnameInputArr.pop();
          let newsurnameInput = surnameInputArr.join('');
          this.value = newsurnameInput;
          inputsurname.classList.add('wrong');
          this.errorMessage = "Фамилия может состоять из букв и пробелов"
          errorMessageNode.classList.add('visible');
          this.validContent = false;
          
        }
        else {
          errorMessageNode.classList.remove('visible');
          this.validContent = true;
          this.errorMessage = "Фамилия может состоять из букв и пробелов"
          inputsurname.classList.remove('wrong');
        }
      }
// Проверяем второе условие
    },
    checkLength(){
      let surnameField = document.querySelector('.surname');
      let inputsurname = surnameField.querySelector('#surname');
      let errorMessageNode = surnameField.querySelector('.error_message');
      if(this.value.length <= 2) {
        inputsurname.classList.add('wrong');
        this.errorMessage="Слишком короткая фамилия"
        errorMessageNode.classList.add('visible');
      } else {
        inputsurname.classList.remove('wrong');
        this.errorMessage=""
        errorMessageNode.classList.remove('visible');
        this.longEnough = true;
      }

      if(this.longEnough) {
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
