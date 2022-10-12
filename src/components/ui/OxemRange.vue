<template>
  <div class="range">
    <p class="title">{{ title }}</p>
    <div class="slider">
      <div class="slider__input-wrapper">
        <input
          class="slider__input"
          type="tel"
          v-model="inputValue"
          @blur="updateValue"
          @input="updateInput"
        />
        <slot></slot>
      </div>
      <input
        class="slider__range"
        name="range"
        type="range"
        :min="min"
        :max="max"
        :step="step"
        v-model="rangeValue"
        @input="updateProgress"
        :style="{
          background: `linear-gradient(90deg, #FF9514 ${this.progress}%, #E1E1E1 ${this.progress}%)`,
        }"
      />
    </div>
  </div>
</template>

<script>
export default {
  name: "OxRange",
  props: {
    title: {
      type: String,
      default: "",
    },
    value: {
      type: Number,
      default: 0,
    },
    min: {
      type: Number,
      default: 0,
    },
    max: {
      type: Number,
      default: 100,
    },
    step: {
      type: Number,
      default: 1,
    },
  },
  data() {
    return {
      inputValue: this.formatNumbers(this.min),
      rangeValue: this.min,
      onChange: this.min,
      progress: 0,
    };
  },
  methods: {
    formatNumbers(num) {
      return new Intl.NumberFormat("ru-RU").format(num);
    },

    updateProgress() {
      this.progress =
        ((this.rangeValue - this.min) / (this.max - this.min)) * 100;
      this.inputValue = this.formatNumbers(this.rangeValue);
    },

    updateValue() {
      if (this.onChange !== this.rangeValue) {
        if (this.rangeValue > this.max) {
          this.inputValue = this.formatNumbers(this.max);
        } else if (this.rangeValue < this.min) {
          this.inputValue = this.formatNumbers(this.min);
        }
      }
      this.updateInput();
      this.roundValue();
    },

    roundValue() {
      let remainder = this.rangeValue % this.step;
      if (remainder < this.step) {
        this.rangeValue = this.rangeValue - remainder + +this.step;
        this.inputValue = this.formatNumbers(this.rangeValue);
      }
    },

    updateInput() {
      this.rangeValue = Number(
        this.inputValue.replace(/\s/g, "").replace(/\D/g, "")
      );
      this.updateProgress();
    },
  },
};
</script>

<style lang="scss" scoped>
@import "../../assets/scss/variables";

.range {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.title {
  font-family: "Gilroy", serif;
  font-style: normal;
  font-weight: 400;
  font-size: 16px;
  line-height: 20px;
  margin-bottom: 24px;
  margin-top: 32px;
}

.slider {
  position: relative;
  display: flex;
  flex-direction: column;
  min-width: 280px;
  padding: 20px 20px 0 20px;
  max-width: 300px;
  margin-bottom: 8px;

  &:after {
    position: absolute;
    top: 0;
    left: 0;
    display: block;
    content: "";
    width: 100%;
    height: 97%;
    border-radius: 16px;
    background-color: $background-color;
    z-index: -1;
  }

  .slider__input-wrapper {
    position: relative;
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 11px;
    overflow: hidden;

    .slider__input {
      display: inline-block;
      outline: none;
      border: none;
      padding: 0 5px 0 0;
      min-width: 200px;
      width: auto;
      font-family: "Nekst", sans-serif;
      font-style: normal;
      font-weight: 900;
      font-size: 22px;
      line-height: 20px;
      background-color: transparent;

      -moz-appearance: textfield;
      &::-webkit-outer-spin-button,
      &::-webkit-inner-spin-button {
        -webkit-appearance: none;
        margin: 0;
      }
    }

    //.slider__label {
    //  font-style: normal;
    //  font-weight: 900;
    //  font-size: 30px;
    //  //line-height: 36px;
    //  overflow: hidden;
    //  white-space: nowrap;
    //  text-overflow: ellipsis;
    //}
  }

  .slider__range {
    -webkit-appearance: none;
    -webkit-transition: 0.2s;
    width: 100%;

    &:focus {
      outline: none;
    }

    &::-webkit-slider-runnable-track {
      width: 100%;
      max-height: 2px;
      cursor: pointer;
      accent-color: red;
      //transition: max-height .3s ease;

      &:hover {
        //max-height: 1px;
      }
    }

    &::-webkit-slider-thumb {
      -webkit-appearance: none;
      border: none;
      height: 20px;
      width: 20px;
      border-radius: 100%;
      background: $primary-color;
      cursor: pointer;
      margin-top: -10px;
      transition: transform 0.3s ease;

      &:hover {
        transform: scale(1.2);
      }
    }
  }
}
</style>
