<template>
  <div>
    <input type="text" class="todo-input" placeholder="What needs to be done" v-model="newTodo" @keyup.enter="addTodo">
    <div v-for="(todo, index) in todos" :key="todo.id" class="todo-item">
      <div class="todo-item-left">
        <div v-if="!todo.editing" @dblclick="editTodo(todo)" class="todo-item-lavel">{{ todo.title }}</div>

        <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" v-focus>

        </div>
        <div class="remove-item" @click="removeTodo(index)">
          &times;
        </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'todo-list',
  data () {
    return {
      newTodo: '',
      idForTodo: 3,
      todos: [
        {
          'id': 1,
          'title': 'Finish Vue Screencast',
          'completed': false,
          'editing': false,
        },
        {
          'id': 2,
          'title': 'Take over world',
          'completed': false,
          'editing': false,
        },
      ]
    }
  },
  directives: {// `v-focus` というローカルディレクティブに登録
  focus: {
    // ディレクティブ定義
    inserted: function (el) {// 要素にフォーカスを当てる
      el.focus()
    }
  }
},
  methods: {
    addTodo() {
      if (this.newTodo.trim().length == 0) {
        return
      }

      this.todos.push({
        id: this.idForTodo,
        title: this.newTodo,
        completed: false,
      })

      this.newTodo = ''
      this.idForTodo++
    },

    editTodo(todo) {
      alert('doble clicked')
      todo.editing = true
    },

    doneEdit(todo) {
      todo.editing = false
    },

    removeTodo(index) {
      this.todos.splice(index, 1)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">

.todo-input {
    width: 100%;
    padding: 10px 18px;
    font-size: 18px;
    margin-bottom: 16px;

    &:focus {
      outline: 0;
    }
  }

  .todo-item {
    margin-bottom: 12px;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .remove-item {
    cursor: pointer;
    margin-left: 14px;
    &:hover {
      color: black;
    }
  }

.todo-item-edit {
    font-size: 24px;
    color: #2c3e50;
    margin-left: 12px;
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc; //override defaults
    font-family: 'Avenir', Helvetica, Arial, sans-serif;

      &:focus {
        outline: none;
    }

  }

</style>
