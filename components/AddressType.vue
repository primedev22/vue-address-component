<template>
  <div class="address-type" tabindex="0" @focusout="focusout">
    <div class="selected" @click="toggle">{{ options[value] }}</div>
    <div v-if="isOpen" class="options">
      <div
        v-for="(option, index) in options"
        :key="index"
        class="option"
        @click="select(index)"
      >
        {{ option }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    value: {
      type: Number,
      default: 0,
    },
    options: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      isOpen: false,
    }
  },
  methods: {
    toggle() {
      this.isOpen = !this.isOpen
    },
    select(index) {
      this.$emit('input', index)
      this.isOpen = !this.isOpen
    },
    focusout() {
      this.isOpen = false
    },
  },
}
</script>

<style scoped>
.address-type {
  cursor: pointer;
  position: relative;
  min-width: 140px;
  height: 30px;
  border-bottom: 1px solid white;
  color: white;
}
.address-type:focus {
  outline: none;
}
.selected {
  position: relative;
  padding: 0 10px;
}
.selected:after {
  content: '';
  top: 5px;
  right: 10px;
  position: absolute;
  width: 10px;
  height: 10px;
  background: transparent;
  border-top: 2px solid #bfbfbf;
  border-left: 2px solid #bfbfbf;
  transition: all 250ms ease-in-out;
  text-decoration: none;
  color: transparent;
  transform: rotate(225deg);
}
.options {
  position: absolute;
  left: 0;
  top: 100%;
  z-index: 1;
  width: 100%;
  margin-top: 10px;
  padding: 10px 0;
  background: #333333;
  border-bottom-left-radius: 10px;
  border-bottom-right-radius: 10px;
}
.option {
  padding: 0 10px;
}
.option:hover {
  opacity: 50%;
}
</style>
