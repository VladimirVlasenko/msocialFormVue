<template>
  <div id="app">
    <form class="form" @submit.prevent="sendData" id="form">
      <FormHeader name="Человек"/>
      <InputName @validateField="validateName"></InputName>
      <Surname @validateField="validateSurname"></Surname>
      <InputCity></InputCity>
      <Divider></Divider>
      <InputPassword @validateField="validatePassword"></InputPassword>
      <InputPasswordConfirm @validateField="validatePasswordConfirm" ref="childPassword"></InputPasswordConfirm>
      <Divider></Divider>
      <InputPhone @validateField="validatePhone"></InputPhone>
      <InputEmail @validateField="validateEmail"></InputEmail>
      <InputCheckBox @validateField="validateCheckbox"></InputCheckBox>
      <div class="form__input_wrapper">
        <input type="submit" class="submit_button" disabled value="Изменить">
        <div class="lastChange">{{ lastChange }}</div>
      </div>

    </form>
  </div>
</template>

<script>
import FormHeader from './components/FormHeader.vue'
import InputName from './components/NameField.vue'
import Surname from './components/SurnameField.vue'
import InputCity from './components/CityField.vue'
import Divider from './components/Divider.vue'
import InputPassword from './components/InputPassword.vue'
import InputPasswordConfirm from './components/InputPasswordConfirm.vue'
import InputPhone from './components/InputPhone.vue'
import InputEmail from './components/EmailField.vue'
import InputCheckBox from './components/CheckboxAgree.vue'

export default {
  name: 'App',
  data(){
    return {
      nameIsValid: false,
      surnameIsValid: false,
      cityIsValid: true,
      passwordIsValid: false,
      passwordValue: '',
      passwordConfirm: '',
      passwordConfirmIsValid: false,
      phoneIsValid: false,
      emailIsValid: false,
      checkboxIsValid: false,
      lastChange: '',

    }
  },
  computed: {

  },
  methods: {
    validateName(booleanValue){
      this.nameIsValid = booleanValue
    },
    validateSurname(booleanValue){
      this.surnameIsValid = booleanValue
    },
    validatePassword(booleanValue, passwordValue){
      this.passwordIsValid = booleanValue
      this.passwordValue = passwordValue;
    },
    validatePasswordConfirm(confirmValue) {
      if(this.passwordValue == confirmValue) {
        this.passwordConfirmIsValid = true;
        this.$refs.childPassword.checkDifference(this.passwordConfirmIsValid)
      } else {
        this.passwordConfirmIsValid = false;
        this.$refs.childPassword.checkDifference(this.passwordConfirmIsValid)
      }
    },
    validatePhone(booleanValue){
      this.phoneIsValid = booleanValue;
    },
    validateEmail(booleanValue){
      this.emailIsValid = booleanValue;
    },
    validateCheckbox(booleanValue){
      this.checkboxIsValid = booleanValue;
    },
    checkDate(){
      let date = new Date();
      let yy = date.getFullYear();
      let mm = date.getMonth();
      let dd = date.getDate();
      let hh = date.getHours();
      if(hh.length<2) {
        hh = "0"+hh;
      }
      let mins = date.getMinutes();
      if(mins.length<2) {
        mins = "0"+mins;
      }
      let ss = date.getSeconds();
      if(ss.length<2) {
        ss = "0"+ss;
      }
      let months=["Января", "Февраля", "Марта", "Апреля", "Мая", "Июня", "Июля", "Августа", "Сентября", "Октября", "Ноября", "Декабря"];
      let monthName = ''
      for(let i=0;i<months.length; i++) {
        if(i == mm) {
          monthName = months[i];
        }
      }
      this.lastChange = "Последние изменения " + dd + " " + monthName + " " + yy +" года в " + hh + ":" + mins + ":" + ss
    },
    checkForm(){
      let submitButton=document.querySelector('.submit_button')
      if(this.checkboxIsValid) {
        if( this.nameIsValid && this.surnameIsValid && this.passwordIsValid && this.passwordConfirmIsValid && this.emailIsValid) {
          submitButton.disabled = false;
        }
      } else {
          if(this.nameIsValid && this.surnameIsValid && this.passwordIsValid && this.passwordConfirmIsValid) {
            submitButton.disabled = false;
          } else {
            submitButton.disabled = true;
          }
        }
    },

    sendData(){
      const XHR = new XMLHttpRequest();
        let data={}
        let inputs = document.querySelectorAll('input');
        for(let i=0;i<inputs.length; i++) {
          data[inputs[i].name] = inputs[i].value;
        }
        let urlEncodedData = "",
            urlEncodedDataPairs = [],
            name;
        // Turn the data object into an array of URL-encoded key/value pairs.
        for( name in data ) {
          urlEncodedDataPairs.push( encodeURIComponent( name ) + '=' + encodeURIComponent( data[name] ) );
        }
        // Combine the pairs into a single string and replace all %-encoded spaces to
        // the '+' character; matches the behaviour of browser form submissions.
        urlEncodedData = urlEncodedDataPairs.join( '&' ).replace( /%20/g, '+' );

        // Define what happens on successful data submission
        // XHR.addEventListener( 'load', function() {
        //   alert( 'Yeah! Data sent and response loaded.' );
        // } );

        // // Define what happens in case of error
        // XHR.addEventListener( 'error', function() {
        //   alert( 'Oops! Something went wrong.' );
        // } );
        
        
        // Set up our request
        XHR.open( 'POST', 'https://google.com/cors.php' );

        // Add the required HTTP header for form data POST requests
        XHR.setRequestHeader( 'Content-Type', 'application/x-www-form-urlencoded' );

        // Finally, send our data.
        XHR.send( urlEncodedData );

        let json = JSON.stringify(data)
        console.log(json)
    },
  },
  components: {
    FormHeader,
    InputName,
    Surname,
    InputCity,
    Divider,
    InputPassword,
    InputPasswordConfirm,
    InputPhone,
    InputEmail,
    InputCheckBox,
  },
  watch:{
    nameIsValid(){
      this.checkForm()
      this.checkDate()
    },
    surnameIsValid(){
      this.checkForm()
      this.checkDate()
    },
    passwordConfirmIsValid(){
      this.checkForm()
      this.checkDate()
    },
    passwordIsValid(){
      this.checkForm()
      this.checkDate()
    },
    emailIsValid(){
      this.checkForm()
      this.checkDate()
    },
    checkboxIsValid(){
      this.checkForm()
      this.checkDate()
    }
    
  }
}
</script>

<style lang="scss">
@font-face {
  font-family: 'Arial';
  src: url('./assets/fonts/ArialMT.eot');
  src: local('Arial'), local('ArialMT'),
    url('./assets/fonts/ArialMT.eot?#iefix') format('embedded-opentype'),
    url('./assets/fonts/ArialMT.woff2') format('woff2'),
    url('./assets/fonts/ArialMT.woff') format('woff'),
    url('./assets/fonts/ArialMT.ttf') format('truetype');
  font-weight: 400;
  font-style: normal;
}


@font-face {
  font-family: 'Arial';
  src: url('./assets/fonts/Arial-BoldMT.eot');
  src: local('Arial Bold'), local('Arial-BoldMT'),
    url('./assets/fonts/Arial-BoldMT.eot?#iefix') format('embedded-opentype'),
    url('./assets/fonts/Arial-BoldMT.woff2') format('woff2'),
    url('./assets/fonts/Arial-BoldMT.woff') format('woff'),
    url('./assets/fonts/Arial-BoldMT.ttf') format('truetype');
  font-weight: 700;
  font-style: normal;
}
:active, :hover, :focus {
    outline: 0;
    outline-offset: 0;
}
body {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  font-family: 'Arial', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  width: 100vw;
  height: 100vh;
  position: fixed;
  display: block;
  background-color: rgba(2, 2, 2, 0.2);
  overflow-y: scroll;
  .form {
    max-width: 1169px;
    min-height: 969px;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    margin: 0 auto;
    background-color: #ffffff;
    padding: 90px 100px;
    box-sizing: border-box;
    &__input_wrapper {
      width: 100%;
      display: flex;
      justify-content: flex-start;
      align-items: center;
      input[type="submit"] {
        border: none;
        width: 119px;
        height: 40px;
        font-family: Arial;
        font-style: normal;
        font-weight: normal;
        font-size: 18px;
        line-height: 20px;
        color: #FFFFFF;
        text-shadow: 0px -1px 0px rgba(0, 0, 0, 0.25);
        background-color: #71C838;
        margin-left: 224px;
        cursor: pointer;
        margin-right: 12px;
        &:disabled{
          background-color: lightgray;
          position: relative;
        }
      }
      .lastChange {
        font-family: 'Arial', sans-serif;
        font-style: normal;
        font-weight: normal;
        font-size: 12px;
        line-height: 20px;
        color: #999999;
      }
    }
  }
}
@media screen and (max-width: 1200px) {
  #app {
    .form {
      .inputField {
        .description {
          display: none;
        }
      }
    }
  }
  
}
@media screen and (max-width: 822px) {
  #app {
    .form {
    .inputField {
      flex-direction: column;
      justify-content: space-between;
      align-items: flex-start;
      label {
        width: 100%;
        text-align: left;
        margin-bottom: 15px;
      }
      .input_wrapper {
        width: 100%;
        input {
          width: 100%;
        }
        .select {
          width: 100%;
        }
      }
    }
    .form__input_wrapper {
      input[type="submit"] {
        margin: 0 auto;
      }
    }
  }
  }
  
}
@media screen and (max-width: 450px) {
  #app {
    .form {
      padding: 20px 10px;
    }
  }
}


</style>
