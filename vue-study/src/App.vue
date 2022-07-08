<template>
<!-- config.js에서 모듈 경로 설정을 한다 (main.js) 이후 서버가 실행되면 index.html이 실행되고 "/dist/build.js의 경로를 가진 (설정을 통해  main.js) js 파일을 호출한다.
      호출 이후 main.js에서 App과 Vue 라는 인스턴스를 생성하고 현재 파일에서 app에 하위(지역) 컴포넌트를 등록한다.-->
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoInput v-on:addTodo="addTodo"></TodoInput>
    <TodoList v-bind:propsdata="todoItems" @removeTodo="removeTodo"></TodoList>
    <TodoFooter v-on:removeAll="clearAll"></TodoFooter>
  </div>
</template>

<script>
import TodoHeader from "./components/TodoHeader";
import TodoInput from "./components/TodoInput";
import TodoList from "./components/TodoList";
import TodoFooter from "./components/TodoFooter";
/* import 불러온 파일의 내용이 담길 객체 from 불러올 파일 위치   (여기서 객체명은 자유롭게 작성해도 된다.)*/
export default {
  data() {
    return {
      todoItems: []
    }
  },
  methods: {
    clearAll() {
      localStorage.clear();
      this.todoItems = [];
    },
    addTodo(todoItem) {
      localStorage.setItem(todoItem, todoItem);
      this.todoItems.push(todoItem);
    },
    removeTodo(todoItem, index) {
      localStorage.removeItem(todoItem);
      this.todoItems.splice(index, 1);
    }
  },
  created() {
    if (localStorage.length > 0) {
      for (var i = 0; i < localStorage.length; i++) {
        this.todoItems.push(localStorage.key(i));
      }
    }
  },
  components: {
    'TodoHeader' : TodoHeader,
    'TodoInput'  : TodoInput,
    'TodoList'   : TodoList,
    'TodoFooter' : TodoFooter
  }
}
</script>

<style>
  body {
    text-align: center;
    background-color: #F6F6F8;
  }
  input {
    border-style: groove;
    width: 200px;
  }
  button {
    border-style: groove;
  }
  .shadow {
    box-shadow: 5px 10px 10px rgba(0,0,0,0.03);
  }
</style>
