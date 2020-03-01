<template>
  <form class="search">
    <input 
      type="text"
      @keyup="sendValue"
      class="search__input" 
      :placeholder="placeholder"
    />
    <button class="search__btn">
      <i class="fas fa-search"></i>
    </button>
  </form>
</template>

<script>
export default {
  name: "Input",

  props: {
    placeholder: String
  },

  methods: {
    sendValue() {
      this.$emit('input', event.target.value)
    }
  },
};
</script>