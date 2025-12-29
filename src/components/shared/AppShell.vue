<script setup></script>

<template>
  <div class="app-shell">
    <div class="app-shell__background">
      <svg
        width="402"
        height="889"
        viewBox="0 0 402 889"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path
          d="M19.2412 -23.0508C197.445 5.60068 563.036 129.934 599.766 398.057"
          stroke="#DCA926"
          stroke-width="49"
        />
        <path
          d="M515.643 295.101C446.169 458.272 241.174 777.506 -23.0172 749.079"
          stroke="#DCA926"
          stroke-width="49"
        />
      </svg>
    </div>
    <div class="app-shell__content">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.app-shell {
  position: relative;
  min-height: 100dvh;
  width: 100%;
  background-color: var(--color-navy);
  overflow: hidden;
  display: flex;
  flex-direction: column;
}

.app-shell__background {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 0;
  pointer-events: none;
}

.app-shell__content {
  position: relative;
  z-index: 1;
  flex: 1;
  display: flex;
  flex-direction: column;
}
</style>
