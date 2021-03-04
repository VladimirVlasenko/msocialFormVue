<template>
  <div class="city inputField">
    <label for="city">Ваш город<span>*</span></label>
    <div class="input_wrapper">
      <input class="hidden" type="text" 
      autocomplete="off"
      form="form"
      v-model="selectValue" 
      name="city">
      <div class="select" @click="showOptions">
        {{selectValue}}
      </div>
      <div class="options hidden">
        <div class="option" @click="selectInput" v-for="(key, name) in cities" :key="key">{{ name }}</div>
      </div>
    </div>
  </div>

</template>
<script>
import json from '../assets/cities.json'
export default {
  name: 'Inputcity',
  data(){
    return {
      selectValue: '',
      citiesJson: json, 
      maxPopulation: 0,
      maxCity: '',
      cities: {},
    }
  },
  methods: {
    showOptions() {
      let options = document.querySelector('.options');
      options.classList.remove("hidden");
      let selectLang = document.querySelector(".select");
      selectLang.classList.add("active");
    },
    selectInput(event){
      this.selectValue = event.target.textContent
      let options = document.querySelector('.options');
      options.classList.add("hidden");
      let selectLang = document.querySelector('.select');
      selectLang.classList.remove('active');
    },
  },
  beforeMount(){
    for(let i=0; i< this.citiesJson.length; i++) {
      if(+this.citiesJson[i].population >= 50000) {
        this.cities[this.citiesJson[i].city] = this.citiesJson[i].population;
      }
      
    }
    for(let key in this.cities) {
      if((this.cities[key] - this.maxPopulation) > 0 ) {
        this.maxPopulation = this.cities[key];
        this.maxCity = key;
        this.selectValue = this.maxCity;
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
    input{
      &.hidden {
        display: none;
      }
    }
    .select {
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
      display: flex;
      align-items: center;
      justify-content: flex-start;
      position: relative;
      &::after {
        content: '';
        position: absolute;
        right: 10px;
        width: 14px;
        height: 9px;
        top: 50%;
        transform: translateY(-50%);
        background: url('../assets/img/arrow.svg') center center no-repeat;
      }
    }
    .options {
      position: absolute;
      top: 31px;
      width: 95%;
      height: 200px;
      overflow-y: scroll;
      background-color: #ffffff;
      box-sizing: border-box;
      cursor: pointer;
      border: 1px solid #999999;
      z-index: 2;
      &.hidden {
        display: none;
      }
      .option {
        line-height: 25px;
        &:hover {
          background-color: rgba(202, 202, 202, 0.5);
        }
      }
    }
  }
  
}
</style>
