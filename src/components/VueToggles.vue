<template>
  <button
    role="switch"
    :aria-checked="value ? 'true' : 'false'"
    :aria-readonly="disabled ? 'true' : 'false'"
    :style="bgStyle"
    class="toggles-btn"
    @click="!disabled ? $emit('click', value) : null"
  >
    <span aria-hidden="true" :style="dotStyle" class="dot">
      <span v-show="checkedText && value" :style="textStyle" class="text">
        {{ checkedText }}
      </span>

      <span v-show="uncheckedText && !value" :style="textStyle" class="text">
        {{ uncheckedText }}
      </span>
    </span>
  </button>
</template>

<script>
export default {
  name: 'VueToggles',
  props: {
    value: {
      type: Boolean,
      default: false,
    },
    disabled: {
      type: Boolean,
      default: false,
    },
    reverse: {
      type: Boolean,
      default: false,
    },
    checkedText: {
      type: String,
      default: null,
    },
    uncheckedText: {
      type: String,
      default: null,
    },
    width: {
      type: [Number, String],
      default: 75,
    },
    height: {
      type: [Number, String],
      default: 25,
    },
    uncheckedBg: {
      type: String,
      default: '#939393',
    },
    checkedBg: {
      type: String,
      default: '#5850ec',
    },
    dotColor: {
      type: String,
      default: '#fff',
    },
    fontSize: {
      type: [Number, String],
      default: '12',
    },
    checkedColor: {
      type: String,
      default: '#fff',
    },
    uncheckedColor: {
      type: String,
      default: '#fff',
    },
    fontWeight: {
      type: [Number, String],
      default: 'normal',
    },
  },
  computed: {
    bgStyle() {
      const styles = {
        width: this.width + 'px',
        height: this.height + 'px',
        background: this.value && !this.disabled ? this.checkedBg : this.uncheckedBg,
      };

      return styles;
    },
    dotStyle() {
      const styles = {
        background: this.dotColor,
        width: this.height - 8 + 'px',
        height: this.height - 8 + 'px',
        'min-width': this.height - 8 + 'px',
        'min-height': this.height - 8 + 'px',
        'margin-left': this.value ? this.width - (this.height - 3) + 'px' : '5px',
      };

      if ((!this.value && this.reverse) || (this.value && !this.reverse)) {
        styles.marginLeft = this.width - (this.height - 3) + 'px';
      }

      if ((this.value && this.reverse) || (!this.value && !this.reverse)) {
        styles.marginLeft = '5px';
      }

      return styles;
    },
    textStyle() {
      const styles = {
        'font-weight': this.fontWeight,
        'font-size': this.fontSize + 'px',
        color: this.value && !this.disabled ? this.checkedColor : this.uncheckedColor,
        right: this.value ? this.height - 3 + 'px' : 'auto',
        left: this.value ? 'auto' : this.height - 3 + 'px',
      };

      if (!this.value && this.reverse) {
        styles.right = this.height - 3 + 'px';
        styles.left = 'auto';
      }

      if (this.value && this.reverse) {
        styles.left = this.height - 3 + 'px';
        styles.right = 'auto';
      }

      return styles;
    },
  },
};
</script>

<style scoped lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.toggles-btn {
  cursor: pointer;
  display: flex;
  align-items: center;
  border-radius: 9999px;
  overflow: hidden;
  border: none;
  padding: 0;
  transition: background-color ease 0.2s, width ease 0.2s, height ease 0.2s;

  @media (prefers-reduced-motion: reduce) {
    transition: none;
  }
}

.dot {
  position: relative;
  display: flex;
  align-items: center;
  border-radius: 9999px;
  box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1), 0 1px 2px 0 rgba(0, 0, 0, 0.06);
  transition: margin ease 0.2s;

  @media (prefers-reduced-motion: reduce) {
    transition: none;
  }

  .text {
    position: absolute;
    font-family: inherit;
    user-select: none;

    @media all and (-ms-high-contrast: none) {
      /* IE11 fix */
      top: 50%;
      transform: translateY(-50%);
    }
  }
}
</style>
