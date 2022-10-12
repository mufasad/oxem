<template>
  <div class="container">
    <h1>Рассчитайте стоимость автомобиля в лизинг</h1>
    <form action="">
      <oxem-range
        :title="credit.title"
        :value="credit.value"
        :min="credit.min"
        :max="credit.max"
        :step="credit.step"
        :key="currentPercent"
        @modification="updatePayment"
        ><span class="rouble">₽</span></oxem-range
      >
      <oxem-range
        :title="payment.title"
        :value="payment.value"
        :min="payment.min"
        :max="payment.max"
        :step="payment.percent.step"
        :key="credit.value"
        @modification="updatePercent"
        ><span class="rouble percent">{{ currentPercent }}%</span></oxem-range
      >
      <oxem-range
        :title="month.title"
        :value="month.value"
        :min="month.min"
        :max="month.max"
        :step="month.step"
        @modification="updateMonth"
        ><span class="rouble">мес.</span></oxem-range
      >
      <div class="total">
        <div class="total__item">
          <p class="total__name"></p>
          <p class="total__value">{{ total.sum }}₽</p>
        </div>
        <div class="total__item">
          <p class="total__name"></p>
          <p class="total__value">{{ total.pay }}₽</p>
        </div>
      </div>
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
  data() {
    return {
      credit: {
        title: "Стоимость автомобиля",
        value: 3000000,
        min: 1000000,
        max: 6000000,
        step: 50000,
      },
      payment: {
        title: "Первоначальный взнос",
        value: 0,
        min: 0,
        max: 1,
        percent: {
          min: 10,
          max: 60,
          step: 1,
        },
      },
      month: {
        title: "Срок лизинга",
        value: 24,
        min: 1,
        max: 60,
        step: 1,
      },
      currentPercent: 10,
      total: {
        sum: 0,
        pay: 0,
      },
    };
  },
  created() {
    this.setPercent();
    this.setTotal();
  },
  methods: {
    setTotal() {
      const fee =
        (this.credit.value - this.payment.value) *
        ((0.035 * Math.pow(1 + 0.035, this.month.value)) /
          (Math.pow(1 + 0.035, this.month.value) - 1));
      this.total.sum = this.formatNumbers(
        Math.round(this.payment.value + this.month.value * fee)
      );
      this.total.pay = this.formatNumbers(Math.round(fee));
    },

    formatNumbers(num) {
      return new Intl.NumberFormat("ru-RU").format(num);
    },

    setPercent() {
      this.payment.value = this.credit.value * (this.payment.percent.min / 100);
      this.payment.min = this.credit.value * (this.payment.percent.min / 100);
      this.payment.max = this.credit.value * (this.payment.percent.max / 100);
      this.payment.percent.step = this.credit.min * (1 / 100);
    },
    updatePayment(data) {
      this.credit.value = Number(data);
      this.setPercent();
      this.payment.value = data * (this.currentPercent / 100);
      this.setTotal();
    },
    updatePercent(data) {
      this.payment.value = Number(data);
      this.currentPercent = Math.round((data / this.credit.value) * 100);
      this.setTotal();
    },
    updateMonth(data) {
      this.month.value = Number(data);
      this.setTotal();
    },
  },
};
</script>

<style scoped lang="scss">
@import "src/assets/scss/mixins";
@import "src/assets/scss/variables";

.container {
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 44px 20px;

  .rouble {
    font-style: normal;
    font-weight: 900;
    font-size: 22px;
    line-height: 20px;
    margin-top: -4px;
  }

  .percent {
    position: relative;
    padding: 14px;
    min-width: 41px;
    border-radius: 12px;
    background-color: $gray-color;
    margin: -19px -14px -14px -14px;
  }
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
