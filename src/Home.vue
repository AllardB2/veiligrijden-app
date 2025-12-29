<script setup>
import { ref } from "vue";
import AppShell from "./components/shared/AppShell.vue";
import Mascot from "./components/shared/Mascot.vue";
import BaseButton from "./components/shared/BaseButton.vue";

// Swiper imports
import { Swiper, SwiperSlide } from "swiper/vue";
import { Pagination } from "swiper/modules";
import "swiper/css";
import "swiper/css/pagination";

const modules = [Pagination];

const user = ref({
  name: "Johan",
  points: "41.147",
  score: 57,
  isBeaconConnected: true,
});

const feedbackCards = [
  {
    id: "accel",
    title: "Optrekken",
    sub: "Goed gedaan!",
    icon: "🏎️",
    status: "success",
  },
  {
    id: "brake",
    title: "Remmen",
    sub: "Goed gedaan!",
    icon: "⚠️",
    status: "success",
  },
  {
    id: "speed",
    title: "Snelheid",
    sub: "Probeer rekening te houden met je snelheid",
    icon: "🕒",
    status: "warning",
  },
  {
    id: "turns",
    title: "Bochten",
    sub: "Probeer rekening te houden met je bochten",
    icon: "📍",
    status: "warning",
  },
];

const quickActions = [
  { id: "trips", title: "Ritten", icon: "🛣️" },
  { id: "challenges", title: "Uitdagingen", icon: "🏆" },
  { id: "beacon", title: "Beacon", icon: "📡" },
  { id: "map", title: "Kaart", icon: "🗺️" },
  { id: "discount", title: "Korting", icon: "🏷️" },
  { id: "profile", title: "Profiel", icon: "👤" },
];
</script>

<template>
  <AppShell>
    <div class="home-view pb-5 sticky-bottom-padding">
      <!-- A. Header Section -->
      <header class="home-header px-3 pt-4">
        <div class="d-flex justify-content-between align-items-end">
          <div>
            <h1 class="h3 fw-bold text-white mb-0">Goedemorgen,</h1>
            <p class="h4 text-white opacity-75">{{ user.name }}</p>
            <div class="points-badge mt-2 shadow-sm">
              <span class="points-badge__icon">🪙</span>
              <span class="points-badge__text">{{ user.points }} punten</span>
            </div>
          </div>
          <div class="header-right position-relative">
            <Mascot emotion="neutral" class="mascot-sm" />
            <span class="notif-badge">!</span>
          </div>
        </div>
      </header>

      <!-- B. Rijscores Section -->
      <section class="px-3 mb-5 section-header-scores">
        <div
          class="score-card shadow-sm p-4 bg-white rounded-4 overflow-hidden position-relative"
        >
          <div class="row align-items-center">
            <div class="col-5">
              <div class="score-circle">
                <svg viewBox="0 0 100 100">
                  <circle
                    cx="50"
                    cy="50"
                    r="45"
                    fill="none"
                    stroke="#f0f0f0"
                    stroke-width="8"
                  />
                  <circle
                    cx="50"
                    cy="50"
                    r="45"
                    fill="none"
                    stroke="#003580"
                    stroke-width="8"
                    stroke-dasharray="280"
                    stroke-dashoffset="120"
                    stroke-linecap="round"
                  />
                </svg>
                <div class="score-number">
                  <span class="fs-2 fw-bold">57</span>
                  <span class="small opacity-50 d-block">van 100</span>
                </div>
              </div>
            </div>
            <div class="col-7">
              <div class="d-flex align-items-center gap-2 mb-1">
                <span class="dot-indicator bg-danger"></span>
                <span class="text-danger small fw-bold">Onveilig</span>
              </div>
              <h2 class="h5 fw-bold mb-1">Jouw rijscore</h2>
              <p class="small text-muted mb-0">19 nov - 18 dec</p>
            </div>
          </div>
          <div
            class="beacon-status mt-4 p-3 rounded-3 d-flex align-items-center justify-content-between light-blue-section"
          >
            <div class="d-flex align-items-center gap-3">
              <div class="beacon-icon-box bg-navy rounded-circle">
                <span class="text-white">✓</span>
              </div>
              <div>
                <p class="mb-0 fw-bold small">Beacon verbonden</p>
                <p class="mb-0 x-small text-muted">Laatst actief: vandaag</p>
              </div>
            </div>
            <span class="text-muted">›</span>
          </div>
        </div>
      </section>

      <!-- C. Afgelopen Rit -->
      <section class="px-3 mb-5">
        <h3 class="h2 fw-bold mb-3">Afgelopen rit</h3>
        <div class="white-section p-4 rounded-4 bg-white shadow-sm">
          <div
            class="rewind-promo p-2 px-3 rounded-pill d-inline-flex align-items-center gap-2 mb-3"
          >
            <span class="x-small fw-bold"
              >🔗 Bekijk je rit met: Rit Rewind</span
            >
          </div>

          <div class="row g-3 mb-4">
            <div v-for="card in feedbackCards" :key="card.id" class="col-6">
              <div class="feedback-card p-3 text-center rounded-4 border">
                <div class="feedback-icon fs-2 mb-2">{{ card.icon }}</div>
                <p class="fw-bold mb-1 small">{{ card.title }}</p>
                <p class="x-small text-muted mb-0 line-clamp2">
                  {{ card.sub }}
                </p>
              </div>
            </div>
          </div>

          <BaseButton variant="primary">Deze rit bekijken</BaseButton>
        </div>
      </section>

      <!-- D. Doelstellingen -->
      <section class="mb-5 bg-navy py-4">
        <div
          class="d-flex justify-content-between align-items-center px-3 mb-3"
        >
          <h3 class="h2 fw-bold text-white mb-0">Doelstellingen</h3>
          <button
            class="btn btn-sm btn-light rounded-pill px-3 shadow-sm py-1 x-small fw-bold"
          >
            Bekijk alles ›
          </button>
        </div>

        <swiper
          :slides-per-view="'auto'"
          :space-between="16"
          :pagination="{ clickable: true }"
          :modules="modules"
          class="goals-swiper px-3 pb-5"
        >
          <swiper-slide class="goal-slide">
            <div class="goal-widget p-4 bg-white rounded-4 shadow-sm h-100">
              <div class="goal-gauge text-center position-relative">
                <svg viewBox="0 0 100 55">
                  <path
                    d="M10 50 A 40 40 0 0 1 90 50"
                    fill="none"
                    stroke="#f0f0f0"
                    stroke-width="12"
                    stroke-linecap="round"
                  />
                  <path
                    d="M10 50 A 40 40 0 0 1 80 25"
                    fill="none"
                    stroke="#012856"
                    stroke-width="12"
                    stroke-linecap="round"
                  />
                </svg>
                <div class="gauge-value">
                  <div class="fw-bold h4 mb-0">85/100</div>
                  <div class="x-small opacity-50">Bochtenkoning</div>
                </div>
              </div>
              <p class="x-small text-center mt-3 text-muted mb-0">
                Haal jij het beste uit je bochten?
              </p>
            </div>
          </swiper-slide>

          <swiper-slide class="goal-slide">
            <div class="goal-widget p-4 bg-white rounded-4 shadow-sm h-100">
              <div
                class="d-flex h-100 align-items-end justify-content-center gap-2 pt-2"
              >
                <div class="bar bg-navy" style="height: 40%"></div>
                <div class="bar bg-navy opacity-50" style="height: 60%"></div>
                <div class="bar bg-navy" style="height: 80%"></div>
                <div class="bar bg-navy" style="height: 50%"></div>
              </div>
              <p class="x-small text-center mt-3 text-muted mb-0">
                Gemiddelde score per week
              </p>
            </div>
          </swiper-slide>
        </swiper>
      </section>

      <!-- E. Snelle Acties -->
      <section class="px-3 mb-5">
        <h3 class="h2 fw-bold mb-3">Snelle acties</h3>
        <div class="actions-container bg-white rounded-5 p-4 shadow-sm">
          <div class="row g-3 text-center mb-4">
            <div v-for="action in quickActions" :key="action.id" class="col-4">
              <div class="action-item p-3 border rounded-4">
                <div class="fs-3 mb-1">{{ action.icon }}</div>
                <div class="x-small fw-bold opacity-50">{{ action.title }}</div>
              </div>
            </div>
          </div>
          <BaseButton variant="primary">Bekijk alles</BaseButton>
        </div>
      </section>

      <!-- F. Nieuws -->
      <section class="mb-5">
        <h3 class="px-3 h2 fw-bold mb-3">Nieuws</h3>
        <swiper
          :slides-per-view="'auto'"
          :space-between="16"
          class="news-swiper px-3"
        >
          <swiper-slide class="news-slide">
            <div
              class="news-card bg-white rounded-5 overflow-hidden shadow-sm h-100"
            >
              <div class="news-image bg-light">
                <img
                  src="https://images.unsplash.com/photo-1541899481282-d53bffe3c35d?auto=format&fit=crop&w=300&q=80"
                  alt="News"
                  class="w-100 h-100 object-fit-cover"
                />
              </div>
              <div class="p-4">
                <h4 class="h5 fw-bold mb-2">Autocomfort voor je hond</h4>
                <p class="x-small text-muted mb-3">
                  10 tips om je hond tevreden te houden voor korte en langere
                  ritjes!
                </p>
                <div class="d-flex justify-content-between align-items-center">
                  <div class="d-flex gap-1">
                    <span class="badge bg-gold text-dark x-small py-1 px-2"
                      >Actueel</span
                    >
                    <span class="badge bg-gold text-dark x-small py-1 px-2"
                      >Dierendag</span
                    >
                  </div>
                  <button
                    class="btn btn-navy p-0 rounded-circle d-flex align-items-center justify-content-center shadow-sm"
                    style="width: 32px; height: 32px"
                  >
                    <span class="text-white x-small">→</span>
                  </button>
                </div>
                <p class="x-small text-muted mt-3 mb-0">Leestijd: 5 min.</p>
              </div>
            </div>
          </swiper-slide>
          <!-- Duplicate for testing scroll -->
          <swiper-slide class="news-slide">
            <div
              class="news-card bg-white rounded-5 overflow-hidden shadow-sm h-100"
            >
              <div class="news-image bg-light">
                <img
                  src="https://images.unsplash.com/photo-1449965408869-eaa3f722e40d?auto=format&fit=crop&w=300&q=80"
                  alt="News"
                  class="w-100 h-100 object-fit-cover"
                />
              </div>
              <div class="p-4">
                <h4 class="h5 fw-bold mb-2">Veilig de winter door</h4>
                <p class="x-small text-muted mb-3">
                  Bereid je auto voor op koude dagen met deze handige tips.
                </p>
                <div class="d-flex justify-content-between align-items-center">
                  <div class="d-flex gap-1">
                    <span class="badge bg-gold text-dark x-small py-1 px-2"
                      >Actueel</span
                    >
                    <span class="badge bg-gold text-dark x-small py-1 px-2"
                      >Onderhoud</span
                    >
                  </div>
                  <button
                    class="btn btn-navy p-0 rounded-circle d-flex align-items-center justify-content-center shadow-sm"
                    style="width: 32px; height: 32px"
                  >
                    <span class="text-white x-small">→</span>
                  </button>
                </div>
                <p class="x-small text-muted mt-3 mb-0">Leestijd: 3 min.</p>
              </div>
            </div>
          </swiper-slide>
        </swiper>
      </section>

      <!-- G. Bottom Navigation -->
      <nav
        class="bottom-nav fixed-bottom bg-white shadow-lg d-flex justify-content-around align-items-center py-3"
      >
        <div class="nav-item text-center active">
          <div class="nav-icon mb-1">🏠</div>
          <div class="x-small fw-bold">Home</div>
        </div>
        <div class="nav-item text-center">
          <div class="nav-icon mb-1">🛣️</div>
          <div class="x-small fw-bold">Ritten</div>
        </div>
        <div class="nav-item text-center">
          <div class="nav-icon mb-1">🗺️</div>
          <div class="x-small fw-bold">Kaart</div>
        </div>
        <div class="nav-item text-center">
          <div class="nav-icon mb-1">🏷️</div>
          <div class="x-small fw-bold">Korting</div>
        </div>
        <div class="nav-item text-center">
          <div class="nav-icon mb-1">⚙️</div>
          <div class="x-small fw-bold">Meer</div>
        </div>
      </nav>
    </div>
  </AppShell>
</template>

<style scoped>
.home-view {
  background-color: var(--color-gray-light);
  color: #000;
}

.sticky-bottom-padding {
  padding-bottom: 100px !important;
}

.home-header {
  background-color: var(--color-navy);
  border-bottom-left-radius: 20px;
  border-bottom-right-radius: 20px;
  padding-bottom: 40px;
}

.points-badge {
  background: rgba(255, 255, 255, 0.1);
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 6px 14px;
  border-radius: 99px;
  color: var(--color-gold);
}

.points-badge__text {
  font-size: 13px;
  font-weight: 600;
}

.mascot-sm {
  width: 72px;
}

.notif-badge {
  position: absolute;
  top: 0;
  left: 35px;
  background: #ff4d4d;
  color: white;
  width: 18px;
  height: 18px;
  border-radius: 50%;
  font-size: 11px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: bold;
  border: 2px solid var(--color-navy);
}

/* Score Section */
.score-circle {
  position: relative;
  width: 100%;
}

.score-number {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
  line-height: 1;
}

.dot-indicator {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  display: inline-block;
}

.beacon-icon-box {
  width: 32px;
  height: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.section-header-scores {
  margin-top: -16px;
}

.bg-navy {
  background-color: var(--color-navy);
}

.btn-navy {
  background-color: var(--color-navy);
}

/* Trip Section */
.rewind-promo {
  border: 1px solid #eee;
}

.feedback-card {
  transition: all 0.3s ease;
}

.feedback-card:active {
  background: #fdfdfd;
}

.line-clamp2 {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

/* Swiper Styles */
.goal-slide {
  width: 280px;
  height: auto;
}

.news-slide {
  width: 300px;
  height: auto;
}

.goals-swiper :deep(.swiper-pagination-bullet) {
  background: rgba(255, 255, 255, 0.4);
  opacity: 1;
}

.goals-swiper :deep(.swiper-pagination-bullet-active) {
  background: var(--color-gold);
  width: 20px;
  border-radius: 4px;
}

/* Goals Section Elements */
.goal-gauge {
  height: 80px;
}

.gauge-value {
  position: absolute;
  bottom: 0px;
  left: 50%;
  transform: translateX(-50%);
}

.bar {
  width: 12px;
  border-radius: 4px;
}

/* Actions Section */
.action-item {
  transition: transform 0.2s;
}

.action-item:active {
  transform: scale(0.95);
}

/* News Section */
.news-image {
  height: 180px;
}

/* Bottom Nav */
.bottom-nav {
  z-index: 1000;
  border-top: 1px solid #eee;
  padding-bottom: env(safe-area-inset-bottom);
}

.nav-item {
  color: #888;
  cursor: pointer;
}

.nav-item.active {
  color: var(--color-gold);
}

.nav-icon {
  font-size: 22px;
}

.x-small {
  font-size: 10px;
}

/* Helpers */
.rounded-5 {
  border-radius: 2.5rem !important;
}

.bg-gold {
  background-color: var(--color-gold);
}
</style>
