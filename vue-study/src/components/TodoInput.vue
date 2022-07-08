<template>
    <div class="inputBox shadow">
      <!-- v-model은 폼에서 주로 사용되는 속성. 폼에 입력한 값을 뷰 인스턴스의 데이터와 즉시 동기화함
           v-model의 경우 v-bind와 v-on의 기능의 조합으로 동작하는데 입력시 직정 지정하지 않아도 되는 이점
           v-bind 속성 : 뷰 인스턴스의 데이터 속성을 해당 HTML 요소에 연결할 때 사용
           v-on 속성 : 해당 HTML 요소의 이벤트를 뷰 인스턴스의 로직과 연결할 때 사용
           HTML 입력 요소의 종류에 따라 `v-model` 속성 구성 방법 (각각 v-bind / v-on)
           (1) input 태그에는 `value / input`
           (2) checkbox 태그에는 `checked / change`
           (3) select 태그에는 `value / change`

           v-model은 즉시 동기화 되기 때문에 사용하기 용이하지만, 한글을 입력할때는 한글자가 끝나야 반응함 (영어는 즉시 반응)
           그래서 바로 반응을 원하는 경우 v-bind:value와 v-on:input를 직접 연결해서 사용
           한국어 입력 처리 (https://joshua1988.github.io/web-development/vuejs/v-model-usage/)
           -->
      <input type="text" v-model="newTodoItem" placeholder="Type what you have to do" v-on:keypress.enter="addTodo">
      <span class="addContainer" v-on:click="addTodo">
      <i class="addBtn fas fa-plus" aria-hidden="true"></i>
    </span>

      <modal v-if="showModal" @close="showModal = false">     <!-- flag값이 true인 경우 동작 닫기 버튼을 누르면 flag값 false로 변경-->
        <h3 slot="header">경고</h3>
        <span slot="footer" @click="showModal = false">할 일을 입력하세요.
        <i class="closeModalBtn fas fa-times" aria-hidden="true"></i>
      </span>
      </modal>
    </div>
</template>

<script>
import Modal from "./common/Modal";

export default {
  /* 컴포넌트를 export 하는 부분으로 v-model의 값의 변화를 data로 받고 그 값을 newTodoItem에 담아 리턴해준다. */
  data() {  // ES6 문법으로 data : 가 아닌 data()로 지정
    return {
      newTodoItem: '',      // 인풋에 입력받은 값을 저장
      showModal: false
    }
  },
  methods: {
    addTodo() {
      if (this.newTodoItem !== "") {    // this.newTodoItem은 현재 컴포넌트를 의미 / 인풋 박스에 입력된 데이터가 없을 경우 예외 처리
        var value = this.newTodoItem && this.newTodoItem.trim();
        this.$emit('addTodo', value)  // 입력받은 데이터 부모 컴포넌트로 전달
        this.clearInput();            // 현재 메소드에서 this 를 사용하여 clearInput에 접근
      } else {
        // 인풋 박스에 값이 없으면 flag값 true로 변경
        this.showModal = !this.showModal;
      }
    },
    clearInput() {    // 인풋 박스의 데이터 초기화
      this.newTodoItem = '';
    }
  },
  components: {
    Modal: Modal
  }
}
</script>

<style>
input:focus {
  outline: none;
}
.inputBox {
  background: white;
  height: 50px;
  line-height: 50px;
  border-radius: 5px;
}
.inputBox input {
  border-style: none;
  font-size: 0.9rem;
}
.addContainer {
  float: right;
  background: linear-gradient(to right, #6478FB, #8763FB);
  display: inline-block;
  width: 3rem;
  border-radius: 0 5px 5px 0;
}
.addBtn {
  color: white;
  vertical-align: middle;
}
</style>
