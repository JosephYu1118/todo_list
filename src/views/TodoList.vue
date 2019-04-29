<template lang="pug">
.todo_list
  .title 我的待辦清單
  .input-group.mb-3
    .input-group-prepend
      span.input-group-text 待辦事項
    input.form-control(type="text"
                        placeholder="準備要做的..."
                        v-model="newTodo"
                        @keyup.enter="addTodo()")
    .input-group-append
      button.btn.btn-primary(type="button"
                              @click="addTodo()") 新增
  .card.text-center
    .card-header
      ul.nav.nav-tabs.card-header-tabs
        li.nav-item
          a.nav-link(href="#"
                      :class="{ 'active': mode === 'all' }"
                      @click="mode = 'all'") 全部
        li.nav-item
          a.nav-link(href="#"
                      :class="{ 'active': mode === 'progressing' }"
                      @click="mode = 'progressing'") 進行中
        li.nav-item
          a.nav-link(href="#"
                      :class="{ 'active': mode === 'completed' }"
                      @click="mode = 'completed'") 已完成
    ul.list-group.list-group-flush.text-left
      li.list-group-item(v-for="(todo, t_id) in filterTodos"
                          @dblclick="editTodo(todo)")
        .d-flex.align-items-center(v-if="todo.id !== tempTodo.id")
          .form-check
            input.form-check-input(type="checkbox"
                                    :id="todo.id"
                                    v-model="todo.isComplete")
            label.form-check-label(:for="todo.id"
                                    :class="{ 'completed': todo.isComplete }") {{ todo.title }}
          button.close.ml-auto(type="button"
                                aria-label="Close"
                                @click="removeTodo(todo)")
            span(aria-hidden="true") ×
        .d-flex.align-items-center(v-else)
          input.form-control(type="text"
                              :style="{ width: '90%' }"
                              v-model="tempTitle"
                              @keyup.esc="cancelEdit()"
                              @keyup.enter="saveEdit(todo)")
          button.close.ml-auto(type="button"
                                @click="saveEdit(todo)")
            i.fas.fa-check(:style="{ 'font-size': '15px' }")
    .card-footer.d-flex.justify-content-between
      span 還有 {{ progressingTodos.length }} 筆任務未完成
      a(href="#"
        @click="removeAll()") 清除全部
</template>

<script>
export default {
  data () {
    return {
      todos: [
        {
          id: '12345',
          title: 'Vue Router practice',
          isComplete: false
        }
      ],
      newTodo: '',
      mode: 'all',
      tempTodo: {},
      tempTitle: ''
    }
  },
  methods: {
    addTodo () {
      if (!this.newTodo.trim()) {
        return
      }
      this.todos.push({
        id: Math.floor(Date.now()),
        title: this.newTodo.trim(),
        isComplete: false
      })
      this.newTodo = ''
    },
    removeTodo (todo) {
      this.todos = this.todos.filter(t => t.id !== todo.id)
    },
    editTodo (todo) {
      this.tempTodo = todo
      this.tempTitle = todo.title
    },
    cancelEdit () {
      this.tempTodo = {}
    },
    saveEdit (todo) {
      todo.title = this.tempTitle
      this.tempTitle = ''
      this.tempTodo = {}
    },
    removeAll () {
      this.todos = []
    }
  },
  computed: {
    filterTodos () {
      if (this.mode === 'progressing') {
        return this.todos.filter((todo) => todo.isComplete === false)
      } else if (this.mode === 'completed') {
        return this.todos.filter((todo) => todo.isComplete !== false)
      } else {
        return this.todos
      }
    },
    progressingTodos () {
      return this.todos.filter((todo) => todo.isComplete === false)
    }
  }
}
</script>

<style lang="sass">
@import "@/styles/global.sass"

// ====================
//      MAIN
// ==============================
.todo_list
  color: $c_black
  .title
    margin: 20px 0
    text-align: center
    font-size: 25px
    color: $c_white

.completed
  text-decoration: line-through

</style>
