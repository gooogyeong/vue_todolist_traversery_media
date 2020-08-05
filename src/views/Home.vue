<template>
  <div id="app">
    <!-- <Header /> -->
    <AddTodo v-on:add-todo="addTodo" />
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" />
  </div>
</template>

<script>
import axios from "axios";
//import Header from "../components/layout/Header";
import Todos from "../components/Todos";
import AddTodo from "../components/AddTodo";

export default {
  name: "Home", //"App",
  components: {
    Todos,
    //Header,
    AddTodo,
  },
  //component가 가지는 데이터는 객체를 리턴하는 함수.
  data() {
    return {
      todos: [
        // static, dummy data
        // { id: 1, title: "Crash Vue", completed: false },
        // { id: 2, title: "Work out", completed: true },
        // { id: 3, title: "Read", completed: false },
      ],
    };
  },
  methods: {
    deleteTodo(id) {
      axios
        .delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
        .then(() => (this.todos = this.todos.filter((todo) => todo.id !== id)))
        .catch((err) => console.log(err));
      //this.todos = this.todos.filter((todo) => todo.id !== id);
    },
    addTodo(newTodo) {
      const { title, completed } = newTodo;
      axios
        .post("https://jsonplaceholder.typicode.com/todos", {
          title,
          completed,
        })
        .then((res) => {
          //add new todo UI
          //새로 생성된 todo를 response로 돌려줌
          //네트워크 요청 받고 UI에 추가해주기때문에 약간의 delay 있음
          this.todos = [...this.todos, res.data /*newTodo*/];
        })
        .catch((err) => console.log(err));
      //this.todos = [...this.todos, newTodo];
      //this.todos.push(newTodo);
    },
  },
  created() {
    axios
      .get("https://jsonplaceholder.typicode.com/todos?_limit=5")
      .then((res) => {
        //console.log(res.data[0]);
        //completed: false
        //id: 1
        //title: "delectus aut autem"
        //userId: 1
        this.todos = res.data;
      })
      .catch((err) => console.log(err));
  },
};
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: Arial, Helvetica, sans-serif;
  line-height: 1.4;
}
.btn {
  display: inline-block;
  border: none;
  background: #555;
  color: #fff;
  padding: 7px 20px;
  cursor: pointer;
}
</style>
