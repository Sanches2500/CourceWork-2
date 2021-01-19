<template>
  <div class="container column">
    <form class="card card-w30" @submit.prevent="addComponent">
      <div class="form-control">
        <label for="type">Тип блока</label>
        <select id="type" v-model="typeComponent">
          <option value="title">Заголовок</option>
          <option value="subtitle">Подзаголовок</option>
          <option value="avatar">Аватар</option>
          <option value="text">Текст</option>
        </select>
      </div>

      <div class="form-control">
        <label for="value">Значение</label>
        <textarea id="value" rows="3" v-model="contentComponent"></textarea>
      </div>

      <app-button mod="primary" :disabled="validateButton">Добавить</app-button>
    </form>
    <div class="card card-w70">
      <app-components v-if="!empty" :components-app="components"></app-components>
      <h3 v-else>Добавьте первый блок, чтобы увидеть результат</h3>
    </div>
  </div>
  <div class="container">
    <p>
      <app-button mod="primary" @action="loadComments">Загрузить комментарии</app-button>
    </p>
    <div class="loader" v-if="loading"></div>
    <app-comments :comments="comments" v-else-if="comments.length !== 0 && !loading"></app-comments>
  </div>
</template>

<script>
import AppButton from '@/AppButton'
import AppComponents from '@/AppComponents';
import AppComments from '@/AppComments';
import axios from 'axios'

export default {
  data () {
    return {
      contentComponent: '',
      typeComponent: 'title',
      components: [],
      empty: true,
      loading: false,
      comments: []
    }
  },
  computed: {
    validateButton () {
      return this.contentComponent.length <= 3;
    }
  },
  components: {
    AppButton, AppComponents, AppComments
  },
  methods: {
    async addComponent () {
      try {
        const {data} = await axios.post('https://vue-cource-work-2-default-rtdb.firebaseio.com/components.json', {
          componentType: this.typeComponent,
          componentContent: this.contentComponent
        })

        this.components.push({
          componentContent: this.contentComponent,
          componentType: this.typeComponent,
          id: data.name
        })

        if (this.components.length !== 0) {
          this.empty = false
        }

        this.contentComponent = ''
        this.typeComponent = 'title'
      } catch (e) {
        throw new Error('Запрос не прошел')
      }
    },
    async loadComponents () {
      try {
        const {data} = await axios.get('https://vue-cource-work-2-default-rtdb.firebaseio.com/components.json')
        if (!data) {
          throw new Error ('Запрос не прошел')
        }
        this.components = Object.keys(data).map(key => {
          return {
            id: key,
            ...data[key]
          }
        })
        this.empty = this.components.length === 0;
      } catch (e) {
        this.empty = true
      }
    },
    async loadComments () {
      try {
        this.loading = true
        const {data} = await axios.get('https://jsonplaceholder.typicode.com/comments?_limit=42')
        this.comments = Object.keys(data).map(key => {
          return {
            id: key,
            ...data[key]
          }
        })
        this.loading = false
      } catch (e) {
        this.loading = false
        throw new Error('Ошибка загрузки комментариев')
      }
    }
  },
  mounted() {
    this.loadComponents()
  },
}
</script>

<style>

</style>
