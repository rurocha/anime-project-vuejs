<template>
  <div class="modal">
    <div class="modal__overlay">
      <div class="modal__content">
        <button class="modal__close" @click="isModalOpen">
          <i class="fas fa-times"></i>
        </button>
        <slot name="modal-bg"></slot>
        <slot name="modal-info-anime">
        </slot>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Modal",

  methods: {
      isModalOpen() {
        this.$emit('close', event.currentTarget)
      }
  }
};
</script>
