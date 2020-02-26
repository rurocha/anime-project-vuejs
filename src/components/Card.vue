<template>
  <div class="card"
  @click="emitClickItem"
  >
    <picture class="card__img">
      <slot name="anime-image"></slot>
    </picture>
    <div class="card__content">
      <div class="card__content-name">
        <slot name="anime-name"></slot>
      </div>
      <div class="card__content-synopsis">
        <slot name="anime-synopsis"></slot>
      </div>
      <div class="card__content-seemore">
        <slot name="anime-seemore"></slot>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    emitClickItem() {
      this.$emit('click', event.currentTarget)
    }
  }
};
</script>