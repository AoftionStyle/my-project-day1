<template>
  <div id="app">
    <alertBox>
      <h1 slot="header">Error!!</h1>
      <span slot="sub-header">Fix now!!</span>
    </alertBox>
    <todoList :todos="sortedTodo" @onComplete="completedTask"></todoList>
    <inputBox @onSubmitData="saveData"></inputBox>
  </div>
</template>

<script>
// import todoList from './components/todoList'
import inputBox from './components/inputBox'
import mixins from './mixins.js'
import alertBox from './components/alertBox'

export default {
  components: {
    // 'todoList': todoList,
    'inputBox': inputBox,
    'alertBox': alertBox,
    'todoList': () => import('./components/todoList.vue')
  },
  name: 'app',
  watch: {
    todos: function (newVal, oldVal) {
      this.savePersistant()
    }
  },
  mounted() {
    setTimeout(() => {
      if( localStorage['todos']) {
        this.todos = JSON.parse(localStorage['todos'])
      }
    }, 3000)
  },
  computed: {
    sortedTodo() {
      let todosTemp = this.todos
      return todosTemp.sort((a,b) => b.time - a.time)
    }
  },
  methods: {
    savePersistant () {
      localStorage['todos'] = JSON.stringify(this.todos)
    },
    completedTask (time) {
      this.todos = this.todos.map(i => {
        if(time === i.time) {
          i.completed = true
        }
        return i
      })
    },
    saveData (data) {
      this.todos.push(data)
    }
  },
  data () {
    return {
      todos:[
        {
          text: '1st todo',
          time: 1556253019,
          completed: false
        },
        {
          text: '2st todo',
          time: 1556253043,
          completed: false
        },
        {
          text: '3st todo',
          time: 1556253065,
          completed: true
        }
      ]
    }
  }
}
</script>

<style scoped>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
