<template>
  <div>
    <input type="text" v-model="newTodoItem">   <!-- v-model은 폼에서 주로 사용되는 속성. 폼에 입력한 값을 뷰 인스턴스의 데이터와 즉시 동기화함 -->
    <button>추가</button>
  </div>
</template>

<script>
export default {
  /* 컴포넌트를 export 하는 부분으로 v-model의 값의 변화를 data로 받고 그 값을 newTodoItem에 담아 리턴해준다. */
  data() {  // ES6 문법으로 data : 가 아닌 data()로 지정
    return {
      newTodoItem: ''
    }
  }
}
</script>

<style scoped>

</style>
