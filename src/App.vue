<template>
  <div
    class="card container center"
    id="todo"
  >
    <div v-cloak>
      <h1>{{ title }}</h1>
      <form class="form-control">
        <input
          v-if="!edit.editable"
          type="text"
          maxlength="25"
          v-model.trim="inputTask"
          @keypress.enter.prevent="addTodo()"
          aria-label="Search"
        />
        <input
          v-else
          type="text"
          maxlength="25"
          v-model.trim="inputTask"
          @keypress.enter.prevent="editTodoBtn()"
        />
        <button
          v-if="!edit.editable"
          @click.prevent="addTodo()"
          class="btn"
          type="button"
        >
          Add task
        </button>
        <button
          v-else
          @click.prevent="editTodoBtn()"
          class="btn"
          type="button"
        >
          Edit task
        </button>
      </form>
      <hr />
      <div
        class="list-todos"
        v-if="todos.length"
      >
        <ul class="list">
          <TodoItem
            v-for="(todo, index) in todos"
            :key="index"
            :todo="todo"
            :index="index"
            @remove="removeTodo"
            @done="doneTodo"
            @edit="editTodo"
          />
        </ul>
        <Total
          :todos="todos.length"
          :todosDone="todosDone"
          @deleteAlltodos="deleteAlltodos"
        />
      </div>
      <h4 v-else>There are no tasks!</h4>
    </div>
    <span class="made"
      >Made by
      <a
        href="http://github.com/milley69"
        target="_blank"
        rel="noopener noreferrer"
        >milley</a
      ></span
    >
  </div>
</template>

<script>
import TodoItem from './components/TodoItem.vue'
import Total from './components/Total.vue'
export default {
  components: {
    TodoItem,
    Total,
  },
  data() {
    return {
      title: 'task list',
      inputTask: '',
      edit: { editable: false, index: 0 },
      todos: [],
      // todos: [
      //   { id: 0, task: 'Learn JavaScript', done: true },
      //   { id: 1, task: 'Learn Script', done: false },
      //   { id: 2, task: 'Learn Java', done: false },
      //   { id: 3, task: 'Learn JavaScript', done: true },
      //   { id: 4, task: 'Learn Script', done: false },
      //   { id: 5, task: 'Learn Java', done: false },
      // ],
    }
  },
  methods: {
    setLocalStorage() {
      localStorage.setItem('todos', JSON.stringify(this.todos))
    },
    editTodoBtn() {
      if (this.inputTask == '') {
        return
      }
      this.todos[this.edit.index].task = this.inputTask
      this.edit.editable = false
      this.inputTask = ''
      delete this.todos[this.edit.index].edit
      this.setLocalStorage()
    },
    addTodo() {
      if (this.inputTask != '') {
        this.todos.push({
          id: this.todos.length + 1,
          task: this.inputTask,
          done: false,
        })
        this.inputTask = ''
        this.setLocalStorage()
      }
    },
    removeTodo(index) {
      this.todos.splice(index, 1)
      this.setLocalStorage()
    },
    doneTodo(index) {
      this.todos[index].done = !this.todos[index].done
      this.setLocalStorage()
    },
    editTodo(index) {
      if (!this.edit.editable) {
        this.todos[index].edit = true
        this.edit.editable = true
        this.edit.index = index
        this.inputTask = this.todos[index].task
      }
    },
    deleteAlltodos() {
      this.todos = []
      this.setLocalStorage()
    },
  },
  computed: {
    todosDone() {
      let done = 0

      this.todos.forEach((item) => {
        item.done === true ? done++ : null
      })
      return done
    },
  },
  async mounted() {
    this.todos = (await JSON.parse(localStorage.getItem('todos'))) || []
  },
}
</script>
<style scoped>
.made {
  font-size: 11px;
  opacity: 0.8;
}
</style>
