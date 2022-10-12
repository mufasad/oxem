<template>
  <div class="container">
    <h1>Рассчитайте стоимость автомобиля в лизинг</h1>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
};
</script>

<style scoped lang="scss">
@import "src/assets/scss/mixins";

.container {
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 44px 20px;
}

h1 {
  font-style: normal;
  font-weight: 900;
  font-size: 34px;
  line-height: 90%;

  @include sm-up {
    font-size: 54px;
  }
}
</style>
