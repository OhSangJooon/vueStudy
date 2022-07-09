<template>
  <section>
    <transition-group name="list" tag="ul">   <!--transition-group 애니메이션 추가하기 위한 태그-->
      <!--부모컴포넌트(propsdata)에서 받은 배열을 v-for로 돌림 :key로 value값(todoItem)을 저장  >> transition-group 애니메이션에 사용하기 위함.
        Vue가 각 노드의 ID를 추적하고, 기존 엘리먼트를 재사용하고 재정렬할 수 있도록 힌트를 제공하기 위해서는 Key 속성 제공 필요(:key를 사용하며 유니크하게 작성).
        소스에서는 todoItem 이라는 배열을 key로 잡았지만, 객체가 넘어온 경우 todoItem.요소이름 으로 key를 잡으면 됨
        ps. v-for에서 key값만 가지고 오고 싶을땐 {{key}} 입력. 이경우 위의 :key와 다름
      -->
      <li v-for="(todoItem, index) in propsdata" :key="todoItem" class="shadow">
        <i class="checkBtn fas fa-check" aria-hidden="true" v-show="propMode == 'R'" @click="modeChange(index,todoItem)"></i>
        <span class="content" v-bind:id="`ctnt${index}`" >{{ todoItem }}</span>
        <input class="inputUdtBox" v-show="idx == index && propMode == 'U'" type="text" v-model="udtTodoItem" :placeholder="placeholder" v-on:keypress.enter="updateTodo(todoItem, index)">
        <span class="removeBtn" type="button"  >
          <i class="far fa-trash-alt" aria-hidden="true" @click="removeTodo(todoItem, index)" v-show="propMode == 'R'" ></i>
          <i class="fas fa-plus" aria-hidden="true" @click="updateTodo(todoItem, index)" v-show="idx == index && propMode == 'U'"></i>
        </span>
      </li>
    </transition-group>
    <modal v-if="showModal" @close="showModal = false">     <!-- flag값이 true인 경우 동작 닫기 버튼을 누르면 flag값 false로 변경-->
      <h3 slot="header">경고</h3>
      <span slot="footer" @click="showModal = false">할 일을 입력하세요.
        <i class="closeModalBtn fas fa-times" aria-hidden="true"></i>
      </span>
    </modal>
  </section>
</template>

<script>
import Modal from "./common/Modal";

export default {
  data() {
    return {
      udtTodoItem: '',
      showModal: false,
      placeholder: ''
    }
  },
  props: ['propsdata', 'propMode', 'idx'],   // 부모 컴포넌트인 App.vue에서 데이터 전달
  methods: {
    removeTodo(todoItem, index) {
      this.$emit('removeTodo', todoItem, index);    /* 부모 컴포넌트로 이벤트 값 전달 */
    },
    modeChange(index,todoItem) {        /* 업데이트 모드로 변경 (인풋박스, placeholder 초기화)*/
      this.$emit('modeChange', index);
      this.placeholder = todoItem;
      this.udtTodoItem = '';
    },
    updateTodo(todoItem, index) {
      if (this.udtTodoItem !== "") {    // this.newTodoItem은 현재 컴포넌트를 의미 / 인풋 박스에 입력된 데이터가 없을 경우 예외 처리
        var value = this.udtTodoItem && this.udtTodoItem.trim();
        this.$emit('updateTodo', todoItem, index, value);    /* 부모 컴포넌트로 이벤트 값 전달 */
        this.clearInput();            // 현재 메소드에서 this 를 사용하여 clearInput에 접근
      } else {
        // 인풋 박스에 값이 없으면 flag값 true로 변경
        this.showModal = !this.showModal;
      }
    },
    clearInput() {    // 인풋 박스의 데이터 초기화
      this.udtTodoItem = '';
    }
  },
  components: {
    Modal: Modal
  }
}
</script>

<style>
ul {
  list-style-type: none;
  padding-left: 0px;
  margin-top: 0;
  text-align: left;
}
li {
  display: flex;
  min-height: 50px;
  height: 50px;
  line-height: 50px;
  margin: 0.5rem 0;
  padding: 0 0.9rem;
  background: white;
  border-radius: 5px;
}
.checkBtn {
  line-height: 45px;
  color: #62acde;
  margin-right: 5px;
}
.removeBtn {
  margin-left: auto;
  color: #de4343;
}
.list-enter-active, .list-leave-active {
  transition: all 1s;
}
.list-enter, .list-leave-to {
  opacity: 0;
  transform: translateY(30px);
}
input:focus {
  outline: none;
}
.inputUdtBox {
  background: white;
  height: 35px;
  line-height: 55px;
  border-radius: 5px;
}
.inputBox input {
  border-style: none;
  font-size: 0.9rem;
}
</style>
