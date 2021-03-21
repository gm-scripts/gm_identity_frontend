<template>
  <div
    class="radio"
    :class="{
      active: active,
    }"
  >
    <div class="radio-outer">
      <div class="radio-inner"></div>
    </div>
    <span class="radio-label">{{ label }}</span>
  </div>
</template>
<script>
export default {
  name: "RadioBtn",
  props: {
    label: String,
    active: Boolean,
  },
};
</script>
<style lang="scss">
.radio {
  line-height: calc(3vh * var(--scale));
  display: inline-grid;
  grid-template-columns: calc(4vh * var(--scale)) auto;
  place-items: right;
  margin-bottom: calc(1.5vh * var(--scale));
  .radio-outer {
    height: calc(3vh * var(--scale));
    width: calc(3vh * var(--scale));
    border-radius: 50%;

    border: calc(3vh * var(--scale) / 6) solid var(--color-secondary);

    transform: scale(1.001) translateY(0);
    transition: linear transform 0.1s, border 0.1s;
    display: inline-block;
    position: relative;
    top: calc(1vh * var(--scale));
    margin-right: calc(0.6vh * var(--scale));
    margin-bottom: calc(0.4vh * var(--scale));
    .radio-inner {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      height: 0;
      width: 0;
      background-color: var(--color-primary);
      transform: all 0.2s;
    }
  }
  .radio-outer:hover {
    transform: scale(1.1) translateY(calc(-0.01vh * var(--scale)));
    transition: transform 0.1s;
    cursor: pointer;
  }
  .radio-label {
    display: inline;
    font-size: calc(2.2vh * var(--scale));
    color: var(--text-secondary);
    font-family: Helvetica, Arial, sans-serif;

    position: relative;
    top: calc(1.2vh * var(--scale));
  }
}
.radio:hover {
  .radio-outer {
    transform: scale(1.1) translateY(calc(-0.01vh * var(--scale)));
    transition: transform 0.1s;
    cursor: pointer;
  }
  .radio-label {
    cursor: pointer;
  }
}
.radio.active {
  .radio-outer {
    border: calc(0.5vh * var(--scale)) solid var(--color-primary);
    animation: pulse 0.25s;
    .radio-inner {
      height: calc(3vh * var(--scale) * 0.5);
      width: calc(3vh * var(--scale) * 0.5);
      border-radius: calc(3vh * var(--scale) * 0.25);
    }
  }
}
@keyframes pulse {
  0% {
    transform: scale(1.1);
  }
  50% {
    transform: scale(1.2);
  }
  100% {
    transform: scale(1.1);
  }
}
</style>
