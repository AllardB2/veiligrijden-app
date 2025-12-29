<script setup>
import { ref } from "vue";
import AppShell from "./components/shared/AppShell.vue";
import Mascot from "./components/shared/Mascot.vue";
import CoachBubble from "./components/shared/CoachBubble.vue";
import BaseButton from "./components/shared/BaseButton.vue";
import Home from "./Home.vue";

const currentStep = ref(1);
const isAccepted = ref(false);
const showHome = ref(false);

const nextStep = () => {
  if (currentStep.value < 5) {
    currentStep.value++;
  } else {
    showHome.value = true;
  }
};

const handleAccept = () => {
  isAccepted.value = true;
  nextStep();
};
</script>

<template>
  <Home v-if="showHome" />
  <AppShell v-else>
    <div class="container-fluid h-100 d-flex flex-column onboarding py-4">
      <!-- Shared Mascot Header -->
      <div class="row justify-content-center mb-4 mt-2">
        <div class="col-auto mascot-wrapper">
          <Mascot :emotion="currentStep === 4 ? 'happy' : 'neutral'" />
        </div>
      </div>

      <!-- Main Content Area with Transitions -->
      <div class="flex-grow-1 d-flex flex-column">
        <Transition name="fade-slide" mode="out-in">
          <!-- Step 1: Hoi ik ben rijmaat -->
          <div v-if="currentStep === 1" key="step1" class="step-content">
            <CoachBubble
              pointing="up"
              title="Hoi ik ben rijmaat"
              text="Welkom bij de veilig rijden app van ANWB, ik laat je hier zien hoe alles werkt."
            />
          </div>

          <!-- Step 2: Veilig rijden loont -->
          <div v-else-if="currentStep === 2" key="step2" class="step-content">
            <CoachBubble
              pointing="up"
              title="Veilig rijden loont"
              text="Geen ingewikkelde rapportcijfers, maar helder inzicht in jouw rijstijl. Hoe veiliger je rijdt, hoe hoger je score en hoe lager je premie."
            />
            <div class="row g-3 mt-4 text-center">
              <div class="col-4">
                <div class="icon-box shadow-sm mb-2">🏎️</div>
                <div class="icon-label">Rijgedrag</div>
              </div>
              <div class="col-4">
                <div class="icon-box shadow-sm mb-2">🌿</div>
                <div class="icon-label">Duurzaamheid</div>
              </div>
              <div class="col-4">
                <div class="icon-box shadow-sm mb-2">🏷️</div>
                <div class="icon-label">Kortingen</div>
              </div>
            </div>
          </div>

          <!-- Step 3: Jouw data blijft van jou -->
          <div v-else-if="currentStep === 3" key="step3" class="step-content">
            <CoachBubble
              pointing="up"
              title="Jouw data blijft van jou"
              text="Om je ritten automatisch te herkennen, heb ik toegang nodig tot je locatie en Bluetooth. Ik kijk alleen mee als je rijdt."
            />
          </div>

          <!-- Step 4: Gelukt -->
          <div v-else-if="currentStep === 4" key="step4" class="step-content">
            <CoachBubble
              pointing="up"
              title="Gelukt"
              text="De taak is voltooid"
            />
          </div>

          <!-- Step 5: Verken de homepage -->
          <div v-else-if="currentStep === 5" key="step5" class="step-content">
            <CoachBubble
              pointing="up"
              title="Verken de homepage"
              text="Hier zie je direct je score, je actieve doelen en natuurlijk je huidige korting. Rust en overzicht, precies wat je nodig hebt voor vertrek."
            />
          </div>
        </Transition>
      </div>

      <!-- Persistent Footer Actions -->
      <div class="onboarding__footer mt-4">
        <!-- Step Indicator -->
        <div class="d-flex justify-content-center gap-2 mb-4">
          <div
            v-for="i in 5"
            :key="i"
            class="progress-dot"
            :class="{ 'progress-dot--active': currentStep === i }"
          ></div>
        </div>

        <!-- Buttons Grid -->
        <div class="row justify-content-center g-2 px-3">
          <div class="col-12 col-md-6">
            <!-- Contextual Buttons based on currentStep -->
            <template v-if="currentStep === 3">
              <BaseButton variant="primary" @click="handleAccept" class="mb-2"
                >Accepteren</BaseButton
              >
              <BaseButton variant="secondary" @click="nextStep"
                >Volgende</BaseButton
              >
            </template>

            <template v-else-if="currentStep === 4">
              <div class="status-pill mb-2">
                <span class="status-pill__icon">✅</span>
                <span>Geaccepteerd</span>
              </div>
              <BaseButton variant="primary" @click="nextStep"
                >Volgende</BaseButton
              >
            </template>

            <template v-else-if="currentStep === 5">
              <BaseButton variant="primary" @click="nextStep"
                >Laten we beginnen</BaseButton
              >
            </template>

            <template v-else>
              <BaseButton variant="primary" @click="nextStep"
                >Volgende</BaseButton
              >
            </template>
          </div>
        </div>
      </div>
    </div>
  </AppShell>
</template>

<style>
.onboarding {
  height: 100%;
}

.step-content {
  width: 100%;
  max-width: 420px;
  margin: 0 auto;
}

.icon-box {
  background: white;
  height: 70px;
  width: 70px;
  margin: 0 auto;
  border-radius: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 32px;
}

.icon-label {
  font-size: 11px;
  color: #fff;
  font-weight: 500;
  line-height: 1.2;
}

.progress-dot {
  width: 8px;
  height: 8px;
  background: rgba(255, 255, 255, 0.25);
  border-radius: 50%;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.progress-dot--active {
  background: var(--color-gold);
  width: 24px;
  border-radius: 4px;
}

.status-pill {
  background: var(--color-success);
  color: white;
  padding: 12px;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  font-weight: 700;
  text-transform: uppercase;
  font-size: 14px;
}

/* Animations */
.fade-slide-enter-active,
.fade-slide-leave-active {
  transition: all 0.4s ease-out;
}
.fade-slide-enter-from {
  opacity: 0;
  transform: translateX(20px);
}
.fade-slide-leave-to {
  opacity: 0;
  transform: translateX(-20px);
}

/* Ensure components respect dark theme bg */
.mascot-wrapper {
  position: relative;
  z-index: 2;
}
</style>
