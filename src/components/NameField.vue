<template>
  <div class="name inputField">
    <label for="name">Имя<span>*</span></label>
    <div class="input_wrapper">
      <input type="text" 
      autocomplete="off"
      id="name" 
      name="name"
      form="form"
      placeholder="Введите имя" 
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
  name: 'InputName',
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
      let nameField = document.querySelector('.name');
      let inputName = nameField.querySelector('#name');
      let errorMessageNode = nameField.querySelector('.error_message');
      let nameInputArr = this.value.split('');
      // Проверяем первое условие
      for(let i=0;i<nameInputArr.length; i++) {
        if(nameInputArr[i].match(/[А-я ]/i) == null) {
          nameInputArr.pop();
          let newNameInput = nameInputArr.join('');
          this.value = newNameInput;
          inputName.classList.add('wrong');
          this.errorMessage = "Имя может состоять из букв и пробелов"
          errorMessageNode.classList.add('visible');
          this.validContent = false;
          
        }
        else {
          errorMessageNode.classList.remove('visible');
          this.validContent = true;
          this.errorMessage = "Имя может состоять из букв и пробелов"
          inputName.classList.remove('wrong');
        }
      }
// Проверяем второе условие
    },
    checkLength(){
      let nameField = document.querySelector('.name');
      let inputName = nameField.querySelector('#name');
      let errorMessageNode = nameField.querySelector('.error_message');
      if(this.value.length <= 2) {
        inputName.classList.add('wrong');
        this.errorMessage="Слишком короткое имя"
        errorMessageNode.classList.add('visible');
      } else {
        inputName.classList.remove('wrong');
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
