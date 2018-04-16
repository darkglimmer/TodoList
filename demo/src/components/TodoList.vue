<template>
  <div id="todoList">
    <h1>Todo List</h1>
    <div id="newTodo">
    <input
      type="text"
      v-model="todo"
      @keyup.enter="newTodo"
      style ="width:200px"
    >
    <button
      type="button"
      name="button"
      @click="addTodo"
    >
      add
    </button>
  </div>
    <ul class="todos">
      <li v-for="todo in todos" :key="todo.id" >
        <input
          type="checkbox"
          :checked="todo.isCompleted"
          @click="completed(index)"
        >
        <span
          :class="todo.isCompleted ? 'completed' : ''",
          :class="todo.editing ? 'editing' : ''",
          @click="completed(index)"
        >
        <em>{{ index }}.</em>{{ todo.text }}
        </span>
        <input 
          type="text"
          v-model="todo"
          @keyup.enter="changeTodo"
          style ="width:150px">
          <button class ="delbut" @click = "delTodo">delete</button>
      </li>
    </ul>
    <button @click = "completeall">complete all</button>
    <button @click = "delall">clear</button>
    <div>
      <p v-show="todos.length === 0">
        congratulate! All finish!
      </p>
      <p v-show="todos.length !== 0">
        things: <strong>{{ todos.length }}</strong>   {{ completedCounts }} have been finished,   {{ notCompletedCounts }} need to be done。
      </p>
    </div>
  </div>
</template>
<script>
export default {
  name: 'TodoList',
  components: {
    TodoAdd
  },
  data: () => ({
    todos: []
  }),
  computed: {
    completedCounts () {
      return this.todos.filter(item => item.isCompleted).length
    },
    notCompletedCounts () {
      return this.todos.filter(item => !item.isCompleted).length
    }
  },
  methods: {
    completed (index) {
      this.todos[index].isCompleted = !this.todos[index].isCompleted
    },
    addTodo (todo) {
      this.todos.push({
        text: todo,
        isCompleted: false,
        editing:false
      })
    },
    newTodo () {
      if (this.todo) {
        this.$emit('add', this.todo)
        this.todo = ''
      } else {
        alert('内容不能为空')
      }
    },
    changeTodo(todo){
      todo.text = this.edit;
      todo.editing = !todo.editing;
      this.edit = '';
    },
    editTodo(todo){
      todo.editing = !todo.editing;
    },
    delTodo(todo){
      var index = this.todos.indexOf(todo);
      this.todos.splice(index,1);
    },
    completeall(){
      for(let i = 0; i < this.todos.length; i++){
        this.todos[i].isCompleted = true;
      }
    },
    delall(){
      this.todos = [];
    }
  }
}
</script>
<style scoped>
#todoList {
  margin: 0 auto;
  max-width: 350px;
}
.todos li {
  list-style: none;
}
.todo {
  text-align: left;
  cursor: pointer;
}
.completed {
  text-decoration: line-through;
}
.delbut{
  float: right;
}
</style>



