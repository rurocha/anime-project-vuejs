<template>
  <input 
    type="search"
    @keyup="sendValue" 
  />
</template>

<script>
export default {
  name: "Input",

  methods: {
    sendValue() {
      this.$emit('keyUpVal', event.target.value)
    }
  },
};
</script>