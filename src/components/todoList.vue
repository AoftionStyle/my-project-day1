<template>
<div id="todoList">
    <span v-if="!todos[0]">Loading.....</span>
    <ul v-if="todos[0]">
      <li
      v-for="todo in todos"
      :key="todo.time"
      :id="prefix + todo.time"
      :class="{
          red: (todo.completed === false),
          green: (todo.completed === true),
      }"
      :style="{
          'font-size':fontSize
      }"
      >
      <button v-if="!todo.completed" @click="$emit('onComplete',todo.time)">x</button>
      {{todo.text | cap }}
      </li>
    </ul>
    <h1> {{ message }} </h1>
</div>

</template>

<script>
export default {
  name:'todoList',
  filters: {
    cap (val) {
      return val.toUpperCase()
    },
  },
  data () {
    return {
      fontSize: '20px',
      prefix: 'item-',
      inputText: 'New todo',
    }
  },
  props: {
    todos: {
      type: Array,
      default: [],
    },
    message: {
      type: String,
      default: () => ""
    }
  }
};
</script>


<style scoped>
li {
  list-style: none;
}
.red {
  color: crimson;
}
.green {
  color:darkgreen;
}
.big{
  font-size: 18px;
}
</style>
