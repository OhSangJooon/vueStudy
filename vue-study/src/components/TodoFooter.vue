<template>
  <div class="clearAllContainer">
    <span v-show="propMode == 'R'" class="clearAllBtn" @click="clearTodo">Clear All</span>
    <span v-show="propMode == 'U'" class="modeCancelBtn" @click="modeCancel">수정 취소</span>
  </div>
</template>

<script>
export default {
  props: ['propMode'],
  methods: {
    clearTodo() {
      this.$emit('removeAll');    // 클릭 이벤트를 removeAll 이라는 이벤트명으로 상위 컴포넌트에 전달
    },
    modeCancel() {
      this.$emit('modeCancel');
    }
  }
}
</script>

<style>
.clearAllContainer {
  width: 8.5rem;
  height: 50px;
  line-height: 50px;
  background-color: white;
  border-radius: 5px;
  margin: 0 auto;
}
.clearAllBtn {
  color: #e20303;
  display: block;
}
.modeCancelBtn {
  color: #e20303;
  display: block;
}
</style>
