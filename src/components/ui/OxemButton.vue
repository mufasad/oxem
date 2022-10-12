<template>
  <button
    type="button"
    class="button"
    :class="{ load: load }"
    :disabled="disabled"
  >
    <transition name="fade">
      <span class="button__load" v-if="load"></span>
    </transition>
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: "OxenButton",
  props: {
    load: Boolean,
    disabled: Boolean,
  },
};
</script>

<style lang="scss" scoped>
@import "../../assets/scss/variables";

.button {
  font-family: "Gilroy", sans-serif;
  font-style: normal;
  font-weight: 700;
  font-size: 16px;
  line-height: 24px;
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 12px 24px;
  border-radius: 40px;
  border: none;
  color: $white-color;
  cursor: pointer;
  transition: background-color 0.3s ease, opacity 0.3s ease, color 0.3s ease;
  background-color: $primary-color;

  &:hover {
    background-color: $secondary-color;
  }

  &:active {
    background-color: $tertiary-color;
  }

  &.load {
    //font-size: 0;
    color: transparent;
  }

  &[disabled] {
    //cursor: not-allowed;
    opacity: 0.4;
    cursor: default;

    &:hover,
    &:active {
      background-color: $primary-color;
    }
  }

  .button__load {
    position: absolute;
    width: 21px;
    height: 21px;
    margin: 1.5px 17px;
    animation: circle 1s linear infinite;
    background-image: url("data:image/svg+xml,%3Csvg width='21' height='21' viewBox='0 0 21 21' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M13.8691 19.3825C11.9455 20.1121 9.83718 20.1976 7.86093 19.6261C5.88468 19.0546 4.14734 17.857 2.91009 16.2134C1.67285 14.5698 1.00256 12.569 1.00001 10.5118C0.99745 8.45459 1.66276 6.45212 2.89591 4.80546C4.12906 3.15879 5.86342 1.95691 7.83825 1.38051C9.81307 0.804109 11.9216 0.88433 13.847 1.60911C15.7723 2.3339 17.4103 3.66408 18.5148 5.39968C19.6193 7.13528 20.1305 9.18251 19.9716 11.2336' stroke='white' stroke-width='2' stroke-linecap='round'/%3E%3C/svg%3E%0A");
  }
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}

@keyframes circle {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>
