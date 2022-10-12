<template>
  <div class="container">
    <h1>Рассчитайте стоимость автомобиля в лизинг</h1>
    <form action="">
      <oxem-range
        title="Стоимость автомобиля"
        value="1000000"
        min="1000000"
        max="6000000"
        step="50000"
      ></oxem-range>
      <oxem-button>Оставить заявку</oxem-button>
    </form>
  </div>
</template>

<script>
import OxemRange from "@/components/ui/OxemRange";
import OxemButton from "@/components/ui/OxemButton";
export default {
  name: "HelloWorld",
  components: {
    OxemRange,
    OxemButton,
  },
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
