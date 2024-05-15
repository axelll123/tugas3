<template>
  <div class="todo-app">
    <h1>Todays Activities</h1>
    <form @submit.prevent="addTodo">
      <input type="text" v-model="newTodo" placeholder="Add Something!" />
      <button type="submit" class="primary">Add</button>
    </form>
    <h2>List Items</h2>
    <div class="filter-section">
      <button @click="filterTodos('all')" class="filter-btn">All</button>
      <button @click="filterTodos('active')" class="filter-btn">Not Completed</button>
      <button @click="filterTodos('completed')" class="filter-btn">Completed</button>
    </div>
    <ul class="todo-list">
      <li v-for="(todo, index) in filteredTodos" :key="index">
        <template v-if="editIndex !== index">
          <input type="checkbox" v-model="todo.done" />
          <span :class="{ 'done': todo.done }">{{ todo.text }}</span>
          <button @click="editIndex = index" class="primary">Edit</button>
          <button @click="removeTodo(index)" class="danger">Remove</button>
        </template>
        <template v-else>
          <input type="text" v-model="editedTodo" @keyup.enter="saveEditedTodo" @keyup.esc="cancelEdit" />
          <button @click="saveEditedTodo" class="primary">Save</button>
          <button @click="cancelEdit" class="danger">Cancel</button>
        </template>
      </li>
    </ul>
    <footer>
      <p>Created by Sulthon</p>
    </footer>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: '',
      editedTodo: '',
      todos: [],
      filter: 'all',
      editIndex: -1,
    };
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim().length === 0) {
        return;
      }
      this.todos.push({
        text: this.newTodo,
        done: false,
      });
      this.newTodo = '';
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
    },
    filterTodos(filterType) {
      this.filter = filterType;
    },
    editTodo(index) {
      this.editedTodo = this.todos[index].text;
      this.editIndex = index;
    },
    saveEditedTodo() {
      if (this.editedTodo.trim().length === 0) {
        this.cancelEdit();
        return;
      }
      this.todos[this.editIndex].text = this.editedTodo;
      this.cancelEdit();
    },
    cancelEdit() {
      this.editedTodo = '';
      this.editIndex = -1;
    },
  },
  computed: {
    filteredTodos() {
      switch (this.filter) {
        case 'active':
          return this.todos.filter(todo => !todo.done);
        case 'completed':
          return this.todos.filter(todo => todo.done);
        default:
          return this.todos;
      }
    },
  },
};
</script>

<style>
body {
  color: rgb(252, 248, 248);
  font-family: "Dancing Script", cursive;
  margin: 0;
  background-image: url(/src/assets/3401742.jpg);
  background-size: cover;
  background-repeat: no-repeat;
}


.todo-app {
  font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
  max-width: 600px;
  margin: 40px auto;
  padding: 40px;
  border-radius: 10px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  background-color: #82e6d5; 
  color: rgb(2, 2, 9); 
}

.todo-app h1 {
  text-align: center;
  color: #4d6aff;
  font-size: 2rem;
  margin-bottom: 20px;
}

.todo-app form {
  display: flex;
  margin-bottom: 20px;
}

.todo-app input[type=text] {
  flex: 1;
  padding: 15px 20px;
  font-size: 1.2rem;
  border: 1px solid #ddd;
  border-radius: 5px;
  outline: none;
  box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.1);
}

.todo-app button[type=submit] {
  margin-left: 10px;
  padding: 10px 20px;
  font-size: 1.2rem;
  background-color: rgb(107, 62, 255);
  color: #fff;
}

.done{
  text-decoration: line-through;
  color: #747171;
}

.filter-section {
  text-align: center;
  margin-bottom: 20px;
}

.filter-btn {
  padding: 10px 20px;
  margin: 0 10px; 
  font-size: 1rem;
  background-color: rgb(107, 62, 255); 
  color: #fff; 
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.filter-btn:hover {
  background-color: #0056b3; 
}

.todo-list {
  list-style: none;
  padding: 0;
}

.todo-list li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px;
  margin-bottom: 10px;
  background-color: #000; 
  color: #fff; 
  border-radius: 5px;
}

.todo-list li input[type="checkbox"] {
  margin-right: 10px;
}

.todo-list li .done {
  text-decoration: line-through;
}

.todo-list li .danger {
  padding: 5px 10px;
  font-size: 0.8rem;
  background-color: #dc3545; 
  color: #fff; 
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.todo-list li .danger:hover {
  background-color: #c82333; 
}
</style>
