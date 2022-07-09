<template>
<!-- config.js에서 모듈 경로 설정을 한다 (main.js) 이후 서버가 실행되면 index.html이 실행되고 "/dist/build.js의 경로를 가진 (설정을 통해  main.js) js 파일을 호출한다.
      호출 이후 main.js에서 App과 Vue 라는 인스턴스를 생성하고 현재 파일에서 app에 하위(지역) 컴포넌트를 등록한다.-->
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoInput v-on:addTodo="addTodo"></TodoInput>        <!-- 이벤트 전달방식 사용 (하위 이벤트(addTodo) -> 상위 컴포넌트에서 받아 메서드 동작(addTodo)) -->
    <TodoList v-on:updateTodo="updateTodo" v-bind:propsdata="todoItems" :propMode="mode" :idx="idx" @modeChange="modeChange" @removeTodo="removeTodo"></TodoList>
    <!-- TodoList(하위컴포넌트)의 propsdata 속성에 props로 전달 (객체명으로 바인딩)  속성값은 for문을 돌려 생성된 todoItems 배열
          removeTodo 이벤트를 하위에서 전달받아 부모 컴포넌트에서 정의한 removeTodo 메서드 동작 (이벤트 발생)-->
    <TodoFooter :propMode="mode" v-on:removeAll="clearAll" v-on:modeCancel="modeCancel"></TodoFooter>   <!-- 이벤트 전달방식 사용 (하위 이벤트(removeAll) -> 상위 컴포넌트에서 받아 메서드 동작(clearAll)) -->
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
      /* 데이터 변경에 대한 즉각적인 반응을 하기 위해 최상위 컴포넌트에 조회, 추가, 삭제를 등록한다.
      *  하위 컴포넌트 TodoList에 데이터를 전달하기 위해 todoItems 선언 */
      todoItems: [],   // 로컬 스토리지의 데이터를 담기 위해 빈 배열 생성 (v-for 목록 렌더링에 활용하기 위해 객체가 아닌 빈 배열로 선언)
      mode: 'R',
      idx: 0
    }
  },
  methods: {
    clearAll() {      // 하위 컴포넌트에서 전달받은 메서드를 재정의 (로컬스토리지에서 삭제)
      localStorage.clear();
      this.todoItems = [];
    },
    addTodo(todoItem) {   // 하위 컴포넌트 (TodoInput 에서 $emit으로 보낸 값을 파라미터로 받는다.)
      // 로컬 스토리지에 데이터를 추가하기 위한 API 사용 API 형식 : 키, 값 형태
      localStorage.setItem(todoItem, todoItem);
      this.todoItems.push(todoItem);
    },
    removeTodo(todoItem, index) {   // 하위 컴포넌트 (TodoList 에서 $emit으로 보낸 값을 파라미터로 받음 (할일명, 인덱스))
      localStorage.removeItem(todoItem);
      this.todoItems.splice(todoItem, 1);    // splice 메서드는 배열의 index에서부터 1번째까지 삭제하겠다는 의미
    },
    updateTodo(todoItem, index, value) {
      localStorage.removeItem(localStorage.key(index));
      localStorage.setItem(value, value);
      this.todoItems[index] = value;
      this.modeCancel();
    },
    modeChange(index) {
      if(this.mode == 'R') {
        this.mode = 'U';
        document.querySelector(`#ctnt${index}`).style.display = 'none';
      }
      this.idx = index;
    },
    modeCancel() {
      const content = document.querySelectorAll(`.content`);

      if(this.mode == 'U') {
        this.mode = 'R';
        for(let i=0; i < content.length; i++){
          content[i].style.display = 'block';
        }
      }
    }
  },
  created() {   // created 라이프 사이클 훅에 로컬 스토리지의 데이터 개수만큼 반복 > 위에서 생성한 todoItems 배열에 로컬스토리지 값 push
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
