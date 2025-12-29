<script setup>
import { computed } from "vue";

const props = defineProps({
  title: {
    type: String,
    default: "",
  },
  text: {
    type: String,
    required: true,
  },
  pointing: {
    type: String,
    default: "none", // 'up' | 'down' | 'none'
  },
  show: {
    type: Boolean,
    default: true,
  },
});

const bubbleClasses = computed(() => [
  "coach-bubble",
  `coach-bubble--pointing-${props.pointing}`,
  { "coach-bubble--active": props.show },
]);
</script>

<template>
  <Transition name="bubble-pop">
    <div v-if="show" :class="bubbleClasses">
      <div class="coach-bubble__content">
        <strong v-if="title">{{ title }}</strong>
        <p class="coach-bubble__text">{{ text }}</p>
      </div>
      <div v-if="pointing !== 'none'" class="coach-bubble__pointer" />
    </div>
  </Transition>
</template>

<style scoped>
.coach-bubble {
  position: relative;
  background: var(--color-bg-light);
  border-radius: var(--radius-md);
  padding: var(--spacing-md);
  box-shadow: var(--shadow-sm);
  z-index: 10;
}

.coach-bubble__text {
  color: var(--color-navy);
  font-size: 15px;
  line-height: 1.5;
  margin: 0;
  font-weight: 500;
}

.coach-bubble__pointer {
  position: absolute;
  width: 0;
  height: 0;
  border-left: 10px solid transparent;
  border-right: 10px solid transparent;
}

.coach-bubble--pointing-down .coach-bubble__pointer {
  top: -8px;
  left: 50%;
  transform: translateX(-50%) scaleY(-1);
  border-top: 10px solid var(--color-bg-light);
}

.coach-bubble--pointing-up .coach-bubble__pointer {
  top: -8px;
  left: 50%;
  transform: translateX(-50%);
  border-bottom: 10px solid var(--color-bg-light);
}

.bubble-pop-enter-active {
  transition: var(--transition-pop);
}
.bubble-pop-leave-active {
  transition: var(--transition-soft);
}
.bubble-pop-enter-from {
  opacity: 0;
  transform: scale(0.9) translateY(10px);
}
.bubble-pop-leave-to {
  opacity: 0;
  transform: scale(0.9);
}
</style>
