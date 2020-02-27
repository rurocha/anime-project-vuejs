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
      this.$emit('input', event.target.value)
    }
  },
};
</script>