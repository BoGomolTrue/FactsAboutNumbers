<template>
  <div class="home">
     <div class="app__jimbo">
      <b-spinner class="app__jimbo__spinner" v-if="loading == true"></b-spinner>
      <div class="content" v-if="loading == false">
      <b-jumbotron header="Факты и числа" lead="Выберите число и тип факта">
        <div class="wrapper">
          <b-form-input class="app__jimbo__input" v-model.trim="number" :class="{invalid: ($v.number.$dirty && !$v.number.required) || ($v.number.$dirty && !$v.number.number)}" placeholder="Число"></b-form-input>
          <b-form-select class="app__jimbo__select form-control" v-model="selected" :options="options" :class="{invalid: ($v.selected.$dirty && !$v.selected.required) || ($v.selected.$dirty && !$v.selected.selected)}"></b-form-select>
        </div>
        <span v-if="($v.selected.$dirty && !$v.selected.required) || ($v.number.$dirty && !$v.number.required)" class="helper-text invalid">Необходимо исправить следующие ошибки:</span>
        <span v-if="($v.selected.$dirty && !$v.selected.required)" class="helper-text invalid">* Тип факта не выбран</span>
        <span v-if="($v.number.$dirty && !$v.number.required)" class="helper-text invalid">* Число не введено</span>
        <b-button class="app__jimbo__button" variant="primary" href="#" @click="getFact">Получить факт</b-button>
      </b-jumbotron>
       <div class="container-fluid">
        <div class="row">
       <b-jumbotron class="fact__jimbo" v-if="factsTranslate != ''">
        <div class="wrapper">
          <span> {{factsTranslate}} </span>
         </div>
      </b-jumbotron>
        </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import { required, between } from 'vuelidate/lib/validators'
export default {
  name: 'Home',
  components: {
  },
  props: ['language'],
  data () {
    return {
      number: null,
      selected: null,
      facts: '',
      factsTranslate: '',
      loading: false,
      options: [
        { value: null, text: 'Выберите нужный тип' },
        { value: 'trivia', text: 'Факт из жизни' },
        { value: 'math', text: 'Математический факт' }
      ]
    }
  },
  validations: {
    number: {
      required,
      between: between(1, 999999999999)
    },
    selected: {
      required
    }
  },
  methods: {
    getFact () {
      if (this.$v.$invalid) {
        console.log(this.$v)
        this.$v.$touch()
        return
      }
      this.loading = true
      axios
        .get('http://numbersapi.com/' + this.number + '/' + this.selected + '')
        .then((response) => {
          this.facts = response.data
          axios
            .get('https://amm-api-translate.herokuapp.com/translate?engine=google&text=' + this.facts + '&to=ru')
            .then((response) => {
              this.factsTranslate = response.data.data.result.replace(/[0-9]/g, '').replace(/-/g, ' ')
              this.loading = false
            })
        }
        )
    }
  }
}
</script>
<style lang="scss" scoped>
  .app__jimbo {
    color: white;
    position: absolute;
    top: 25%;
    width: 100%;
    &__spinner{
      position: absolute;
      width: 10rem;
      height: 10rem;
      top: 20%;
      right: 45%;
    }
    &__input {
      display: inline;
      margin: 0 auto;
      text-align: center;
      width:100px;
      margin-bottom: 40px;
    }
    &__button {
      background: black;
      border-color: gray;
      &:hover{
        background: black;
        border-color: gray;
      }
      &:active {
        background: black;
        border-color: gray;
      }
      &:focus {
        background: black;
        border-color: gray;
      }
    }
    &__select{
      width: 200px;
      display: inline;
      height: 38px;
      border: 1px solid #ced4da;
      border-radius: 0.25rem;
    }
  }
  .helper-text {
    display: block;
    padding-bottom: 20px;
    color: red;
  }
  .fact__jimbo{
    text-align: cemter;
    width: 300px;
    height: auto;
    margin: 50px auto;
    border: 1px solid gray;
    & span{
      font-size: 20px;
    }
  }
</style>
