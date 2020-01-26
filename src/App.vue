<template>
  <div id="app">
    <header class="page-header"></header>
    <section class="wrapper">
      <form class="new-todo-form">
        <label class="new-todo-label">
          New Todo:
          <input v-model="newTodo" type="text" class="new-todo-input" />
        </label>
        <button type="submit" @click.prevent="addTodo()" class="new-todo-button">Add</button>
      </form>
      <ul class="todo-list">
        <li v-for="todo in todos" :key="todo.id" class="todo-item">
          <label class="todo-item-label" v-if="currentlyEditing !== todo.id">
            <input
              type="checkbox"
              v-model="todo.completed"
              @change="updateTodo(todo)"
              class="todo-item__checkbox"
            />
            {{todo.text}}
          </label>
          <div v-if="currentlyEditing !== todo.id">
            <button @click="editTodo(todo)" class="todo-button1">
              <img src="./assets/pencil.svg" alt="Edit todo" />
            </button>
            <button @click="deleteTodo(todo)" class="todo-button">
              <img src="https://img.icons8.com/android/24/000000/trash.png" />
            </button>
          </div>
          <form v-else class="edit-todo-form">
            <label class="edit-todo-label">
              Edit:
              <input type="text" v-model="todoEditText" class="edit-todo-input" />
            </label>
            <button type="submit" class="edit-todo-button" @click.prevent="updateTodoText()">Save</button>
          </form>
        </li>
      </ul>
    </section>
  </div>
</template>

<script>
import { todosCollection } from "./firebase.js";

export default {
  name: "app",
  data() {
    return {
      newTodo: "",
      todos: [],
      todo: [],
      currentlyEditing: null,
      todoEditText: ""
    };
  },
  firestore() {
    return {
      todos: todosCollection.orderBy("createdAt", "desc")
    };
  },
  methods: {
    addTodo() {
      todosCollection
        .add({
          text: this.newTodo,
          completed: false,
          id: this.todos.length,
          createdAt: new Date()
        })
        .then(docRef => {
          // eslint-disable-next-line no-console
          console.log("Document written with ID: ", docRef.id);
        })
        .catch(error => {
          // eslint-disable-next-line no-console
          console.error("Error adding document:", error);
        });
      this.newTodo = "";
    },
    updateTodo(todo) {
      todosCollection.doc(todo.id).update({ ...todo });
    },
    deleteTodo(todo) {
      todosCollection.doc(todo.id).delete();
    },
    editTodo(todo) {
      this.currentlyEditing = todo.id;
      this.todoEditText = todo.text;
    },
    updateTodoText() {
      todosCollection.doc(this.currentlyEditing).update({
        text: this.todoEditText
      });
      this.currentlyEditing = null;
      this.todoEditText = "";
    }
  }
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 0;
  padding: 0;
}
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
a {
  color: #565759;
}
.page-header {
  padding: 5rem 0;
  width: 100%;
  background: #565759;
}
.wrapper {
  max-width: 500px;
  margin: 0 auto;
  padding: 0 1rem;
}
.new-todo-form {
  display: flex;
  align-items: flex-end;
  justify-content: space-between;
  padding: 1rem;
  border-radius: 3px;
  border: 1px solid #fafafa;
  box-shadow: 5px 5px 15px rgba(0, 0, 0, 0.15);
  margin-top: -3rem;
  background: white;
}

.todo-button {
  display: inline-block;
  position: absolute;
  right: 37%;
  transform: translateY(-17px);
  background: transparent;
  border: 0;
  fill: #565759 !important;
  padding: 0.5rem;
  width: 40px;
  height: 40px;
  border-radius: 3px;
  cursor: pointer;
}

.todo-button img {
  width: 18px;
}

.todo-button1 {
  display: inline-block;
  background: transparent;
  position: absolute;
  transform: translateY(-17px);
  right: 39%;
  border: 0;
  padding: 0.5rem;
  width: 40px;
  height: 40px;
  border-radius: 3px;
  cursor: pointer;
}

@media (max-width: 1400px) {
  .todo-button {
    display: inline-block;
    position: absolute;
    right: 32%;
    transform: translateY(-17px);
    background: transparent;
    border: 0;
    fill: #565759 !important;
    padding: 0.5rem;
    width: 40px;
    height: 40px;
    border-radius: 3px;
    cursor: pointer;
  }
  .todo-button1 {
    display: inline-block;
    background: transparent;
    position: absolute;
    transform: translateY(-17px);
    right: 35%;
    border: 0;
    padding: 0.5rem;
    width: 40px;
    height: 40px;
    border-radius: 3px;
    cursor: pointer;
  }
}

@media (max-width: 1000px) {
  .todo-button {
    display: inline-block;
    position: absolute;
    right: 25%;
    transform: translateY(-17px);
    background: transparent;
    border: 0;
    fill: #565759 !important;
    padding: 0.5rem;
    width: 40px;
    height: 40px;
    border-radius: 3px;
    cursor: pointer;
  }
  .todo-button1 {
    display: inline-block;
    background: transparent;
    position: absolute;
    transform: translateY(-17px);
    right: 28%;
    border: 0;
    padding: 0.5rem;
    width: 40px;
    height: 40px;
    border-radius: 3px;
    cursor: pointer;
  }
}

@media (max-width: 500px) {
  .todo-button {
    display: inline-block;
    position: absolute;
    right: 8%;
    transform: translateY(-17px);
    background: transparent;
    border: 0;
    fill: #565759 !important;
    padding: 0.5rem;
    width: 40px;
    height: 40px;
    border-radius: 3px;
    cursor: pointer;
  }
  .todo-button1 {
    display: inline-block;
    background: transparent;
    position: absolute;
    transform: translateY(-17px);
    right: 15%;
    border: 0;
    padding: 0.5rem;
    width: 40px;
    height: 40px;
    border-radius: 3px;
    cursor: pointer;
  }
}

.new-todo-label {
  display: flex;
  flex-direction: column;
  width: 80%;
  justify-content: flex-start;
  text-align: left;
  font-weight: bold;
}
.new-todo-input {
  padding: 0.5rem;
  border-radius: 3px;
  border: 1px solid lightgrey;
  font-size: 1rem;
  margin-top: 0.5rem;
  font-weight: normal;
}
.new-todo-button {
  font-size: 1rem;
  padding: 0.5rem 0.7rem;
  border-radius: 3px;
  color: white;
  font-weight: bold;
  background: #565759;
  flex: 1;
  margin-left: 1rem;
  border: 1px solid #565759;
}
.todo-item {
  display: flex;
  align-items: center;
  border: 1px solid lightgrey;
}
.todo-item-label {
  cursor: pointer;
  padding: 1rem;
}
.todo-item__checkbox {
  margin-right: 1rem;
}
.todo-list {
  max-width: 100%;
  margin: 2rem auto;
}
.edit-todo-form {
  width: 100%;
  justify-content: space-between;
  display: flex;
  padding: 1rem;
}
.edit-todo-label {
  flex: 1;
  text-align: left;
  display: flex;
  align-items: center;
}
.edit-todo-input {
  padding: 0.5rem;
  border-radius: 3px;
  border: 1px solid lightgrey;
  font-size: 1rem;
  font-weight: normal;
  flex: 1;
  margin-left: 1rem;
}
.edit-todo-button {
  font-size: 1rem;
  padding: 0.5rem 0.7rem;
  border-radius: 3px;
  color: #565759;
  font-weight: bold;
  margin-left: 1rem;
  border: 1px solid #565759;
}
</style>