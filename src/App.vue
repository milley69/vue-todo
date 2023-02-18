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
        >
          Add task
        </button>
        <button
          v-else
          @click.prevent="editTodoBtn()"
          class="btn"
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
          <!-- <TodoItem
            v-for="(todo, index) in todos"
            :key="index"
            :todo="todo"
            :index="index"
            @remove="removeTodo"
            @done="doneTodo"
            @edit="editTodo"
          >
          </TodoItem> -->
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
        <hr />
        <h5>
          Total tasks: <span class="primary">{{ todos.length }} </span> | Completed tasks:
          <span class="primary">{{ todosDone }}</span>
        </h5>
        <h5
          @click="deleteAlltodos"
          class="del-btn"
        >
          Delete all tasks
        </h5>
      </div>
      <h4 v-else>There are no tasks!</h4>
    </div>
  </div>
</template>

<script>
import TodoItem from './components/TodoItem.vue'
export default {
  components: {
    TodoItem,
  },
  data() {
    return {
      title: 'task list',
      inputTask: '',
      edit: { editable: false, index: 0 },
      todos: [{ id: 0, task: 'Learn JavaScript', done: false }],
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
      this.todos[index].edit = true
      this.edit.editable = true
      this.edit.index = index
      this.inputTask = this.todos[index].task
      // console.log(this.todos)
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

<style scoped></style>
