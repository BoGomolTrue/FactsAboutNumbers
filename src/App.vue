<template>
  <div id="app">
    <div class="language__switch">
    <div class="EN" :class="{active: language == 'EN'}" @click="switchLanguage('EN')">EN</div>
    <div class="RU" :class="{active: language == 'RU'}" @click="switchLanguage('RU')">RU</div>
    </div>
    <HomeRu v-if="language == 'RU'" />
    <HomeEn v-if="language == 'EN'"/>
  </div>
</template>
<script>
import HomeRu from './views/HomeRu.vue'
import HomeEn from './views/HomeEn.vue'
export default {
  name: 'App',
  components: {
    HomeRu,
    HomeEn
  },
  data: function () {
    return {
      language: 'EN'
    }
  },
  methods: {
    switchLanguage (event) {
      if (event === 'EN') {
        this.language = 'EN'
      } else {
        this.language = 'RU'
      }
    }
  }
}
</script>

<style lang="scss">
body{
  background: black!important;
}
#app{
    text-align: center;
}
.language__switch{
  float: right;
  margin-right: 50px;
  margin-top: 25px;
}
.EN{
  cursor: pointer;
  padding-right: 20px;
  display: inline;
  font-size: 24px;
  color: white;
}
.RU{
  cursor: pointer;
  display: inline;
  font-size: 24px;
  color: white;
}
.active{
  text-decoration: underline;
}
</style>
