<template>
  <div>
    <input type="text" class="todo-input" placeholder="What needs to be done" v-model="newTodo" @keyup.enter="addTodo">
    <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">
    <div v-for="(todo, index) in todosFiltered" :key="todo.id" class="todo-item">
      <div class="todo-item-left">

        <input type="checkbox" v-model="todo.completed">

        <div v-if="!todo.editing" @dblclick="editTodo(todo)" class="todo-item-label" :class="{ completed : todo.completed }">{{ todo.title }}</div>

        <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus>
        </div>
        <div class="remove-item" @click="removeTodo(index)">
          &times;
        </div>
    </div>
    </transition-group>

      <!-- 37:00 -->
      <div class="extra-container">
      <div><label><input type="checkbox" :checked="!anyRemaining" @change="checkAllTodos"> Check All</label></div>
      <div>{{ remaining }} items left</div>
      </div>
      <!-- 37:00 -->

      <!-- 43:00 -->
      <div class="extra-container">
      <div>
        <button :class="{ active: filter == 'all' }" @click="filter = 'all'">All</button>
        <button :class="{ active: filter == 'active' }" @click="filter = 'active'">Active</button>
        <button :class="{ active: filter == 'completed' }" @click="filter = 'completed'">Completed</button>
      </div>
      <!-- 43:00 -->

      <!-- 48:28 -->
      <div>
        <transition name="fade">
        <button v-if="showClearCompletedButton" @click="clearCompleted">Clear Completed</button>
        </transition>
      </div>
      <!-- 48:28 -->

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
      beforeEditCache: '',
      filter: 'all',
      todos: [
        {
          'id': 1,
          'title': '朝食をきちんととる',
          'completed': false,
          'editing': false,
        },
        {
          'id': 2,
          'title': '筋トレをする',
          'completed': false,
          'editing': false,
        },
      ]
    }
  },

  //38:24
  computed: {
    remaining() { //アイテム数をカウント
      return this.todos.filter(todo => !todo.completed).length
  },
  //40:00
  anyRemaining() { //0itemsなら自動でCheck All有効化
    return this.remaining != 0
    },
  //45:30
  todosFiltered() {
    if (this.filter == 'all') {
      return this.todos
    } else if (this.filter == 'active') {
      return this.todos.filter(todo => !todo.completed)
    } else if (this.filter == 'completed') {
      return this.todos.filter(todo => todo.completed)
    }

    return this.todos
  },

  showClearCompletedButton() {
    return this.todos.filter(todo => todo.completed).length > 0
    }
  },
  directives: {
  focus: {
    // ディレクティブ定義
    inserted: function (el) {
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
      // alert('doble clicked')
      this.beforeEditCache = todo.title
      todo.editing = true
    },

    doneEdit(todo) {
      // 33:00
      if (todo.title.trim() == '') {
        todo.title = this.beforeEditCache
      }
      todo.editing = false
    },

    //31:30
    cancelEdit(todo) {
      todo.title = this.beforeEditCache
      todo.editing = false
    },

    removeTodo(index) {
      this.todos.splice(index, 1)
    },

  //41:50
    checkAllTodos() { //Check Allを有効化
    this.todos.forEach((todo) => todo.completed =
    event.target.checked)
    },
    //48:50
    clearCompleted() {
      this.todos = this.todos.filter(todo => !todo.completed)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

<style lang="scss">
// Animate.cssのCDN
@import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.7.0/animate.min.css");

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
    animation-duration: 0.3s; //速度
  }

  .remove-item {
    cursor: pointer;
    margin-left: 14px;
    &:hover {
      color: black;
    }
  }
  .todo-item-left { // later
    display: flex;
    align-items: center;
  }
  .todo-item-label {
    padding: 10px;
    border: 1px solid white;
    margin-left: 12px;
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

  //34:50 打ち消し線
  .completed {
    text-decoration: line-through;
    color: grey;
  }

  // 37:40 Check All
  .extra-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 16px;
    border-top: 1px solid lightgrey;
    padding-top: 14px;
    margin-bottom: 14px;
  }
    button {
    font-size: 14px;
    background-color: white;
    appearance: none;
    &:hover {
      background: lightgreen;
    }
    &:focus {
      outline: none;
    }
  }
  .active {
    background: lightgreen;
  }

  //50:25 CSS Transitions
  .fade-enter-active, .fade-leave-active {
    transition: opacity .2s;
  }
  .fade-enter, .fade-leave-to {
    opacity: 0;
  }

</style>
