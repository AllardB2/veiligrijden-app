<script setup>
defineProps({
  variant: {
    type: String,
    default: "primary", // 'primary' | 'secondary' | 'success'
  },
  disabled: Boolean,
  loading: Boolean,
});
</script>

<template>
  <button
    class="base-button"
    :class="[`base-button--${variant}`]"
    :disabled="disabled || loading"
  >
    <span v-if="loading" class="base-button__loader" />
    <slot v-else />

    <div class="icon-box">
      <svg
        width="12"
        height="10"
        viewBox="0 0 12 10"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path
          d="M6.43597 0.189841C6.68909 -0.0632794 7.09938 -0.0632794 7.3525 0.189841L11.4769 4.31423C11.73 4.56735 11.73 4.97764 11.4769 5.23076L7.3525 9.35515C7.09938 9.60827 6.68909 9.60827 6.43597 9.35515C6.18285 9.10203 6.18285 8.69174 6.43597 8.43862L9.45394 5.42065L0.648154 5.42065C0.290188 5.42065 -4.48519e-07 5.13046 -4.17225e-07 4.7725C-3.8593e-07 4.41453 0.290188 4.12434 0.648154 4.12434L9.45394 4.12434L6.43597 1.10637C6.18285 0.853252 6.18285 0.442961 6.43597 0.189841Z"
          fill="#012856"
        />
      </svg>
    </div>
  </button>
</template>

<style scoped>
.base-button {
  width: 100%;
  padding: 16px 24px;
  border-radius: var(--radius-sm);
  border: none;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: var(--transition-soft);
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  position: relative;
}

.base-button--primary {
  background-color: var(--color-gold);
  color: var(--color-navy);
}

.base-button .icon-box {
  width: 24px;
  height: 24px;
  background-color: var(--color-white);
  border-radius: var(--radius-full);
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  right: 16px;
  top: 50%;
  transform: translateY(-50%);
}

.base-button--secondary {
  background-color: transparent;
  color: var(--color-white);
  border: 2px solid var(--color-white);
}

.base-button--success {
  background-color: var(--color-success);
  color: var(--color-white);
}

.base-button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.base-button:active:not(:disabled) {
  transform: scale(0.98);
}

.base-button__loader {
  width: 20px;
  height: 20px;
  border: 2px solid currentColor;
  border-top-color: transparent;
  border-radius: 50%;
  animation: spin 0.8s linear infinite;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}
</style>
