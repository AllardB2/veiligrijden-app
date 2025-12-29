<script setup>
defineProps({
  variant: {
    type: String,
    default: "white", // 'white' | 'glass'
  },
});
</script>

<template>
  <div class="base-card" :class="[`base-card--${variant}`]">
    <slot />
  </div>
</template>

<style scoped>
.base-card {
  padding: var(--spacing-lg);
  border-radius: var(--radius-lg);
  transition: var(--transition-soft);
}

.base-card--white {
  background-color: var(--color-white);
  box-shadow: var(--shadow-md);
}

.base-card--glass {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  color: var(--color-white);
}
</style>
