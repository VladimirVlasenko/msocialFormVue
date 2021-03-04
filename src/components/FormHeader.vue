<template>
  <div class="hello">
    Здравствуйте, <span>{{name}}</span>
  </div>
</template>

<script>
export default {
  name: 'FormHeader',
  props: {
    name: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.hello {
  font-family: Arial, 'Arial', sans-serif;
  font-style: normal;
  font-weight: bold;
  font-size: 30px;
  line-height: 34px;
  height: 34px;
  color: #666666;
  text-align: left;
  margin-bottom: 45px;
  span {
    color: #000000;
  }
}
</style>
