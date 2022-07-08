<template>
  <header>
    <h1>TODO it!</h1>   <!-- 애플리케이션 제목 추가 -->
  </header>
</template>

<script>
export default {
  name: "TodoHeader"
}
</script>

<!-- style에서 scoped를 사용하면 오직 특정 컴포넌트(현재)에서만 고유의 스타일을 선언하여 적용할 수 있다.
    vue-loader이 제공함
     scoped를 사용한 뒤 컴포넌트가 생성되면 태그에 data- 로 시작하는 고유의 속성값을 가지게 된다 Ex) data-a31h23f -->
<style>
  h1 {
    color : #2F3B52;
    font-weight: 900;
    margin: 2.5rem 0 1.5rem;
  }
</style>
