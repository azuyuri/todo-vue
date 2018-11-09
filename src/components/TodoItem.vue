// ? 🏃🏻💨 Part2 I'm on it!
<template>
  <div class="todo-item">
    <div class="todo-item-left">
      <input
      type="checkbox"
      v-model="completed"
      @change="doneEdit"
      >
        <div
          v-if="!editing"
          @dblclick="editTodo"
          class="todo-item-label"
          :class="{ completed : completed }"
          >
          {{ title }}
        </div>
      <input
      v-else class="todo-item-edit"
      type="text"
      v-model="title"
      @blur="doneEdit"
      @keyup.enter="doneEdit"
      @keyup.esc="cancelEdit"
      v-focus
      >
    </div>
    <div
      class="remove-item"
      @click="removeTodo(index)"
      >
      &times;
    </div>
  </div>
</template>


<script>
export default {
  name: 'todo-item',
  props: {
    todo: {
      type: Object,
      required: true,
    },
    index: {
      type: Number,
      required: true,
    },
    checkAll: {
      type: Boolean,
      required: true,
    }
  },
  data() { //!クリック時のイベント設定
    return {
      'id': this.todo.id,
      'title': this.todo.title,
      'completed': this.todo.completed,
      'editing': this.todo.editing,
      'beforeEditCache': '',
    }
  },
  methods: {
    removeTodo(index) {
      this.$emit('removedTodo', index)
    },
    editTodo() {
      this.beforeEditCache = this.title
      this.editing = true
    },
    doneEdit() {
      // 33:00
      if (this.title.trim() == '') {
        this.title = this.beforeEditCache
      }
      this.editing = false
      this.$emit('finishedEdit', {
        'index': this.index,
        'todo': {
          'id': this.id,
          'title': this.title,
          'completed': this.completed,
          'editing': this.editing,
        }
      })
    },
    cancelEdit() {
      this.title = this.beforeEditCache
      this.editing = false
    },
  }
}
</script>
