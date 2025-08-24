<template>
  <img
    v-if="!visible"
    src="@/assets/icones/eye.png"
    alt=""
    class="btn"
    @click="toggle"
  />
  <img
    v-else
    src="@/assets/icones/eye-crossed.png"
    alt=""
    class="btn"
    @click="toggle"
  />
</template>

<script>
export default {
  props: ['input'],
  data() {
    return { visible: false }
  },
  methods: {
    toggle() {
      this.visible = !this.visible;
      this.input.type = this.visible ? 'text' : 'password';
    }
  }
}
</script>

<style>
.btn {
 max-width: 24px;
  max-height: 24px;
  cursor: pointer;
}
</style>
