<script setup>
import { ref } from "vue";
import AppShell from "./components/shared/AppShell.vue";
import Mascot from "./components/shared/Mascot.vue";
import BaseButton from "./components/shared/BaseButton.vue";
import Wrapped from "./Wrapped.vue";

// Swiper imports
import { Swiper, SwiperSlide } from "swiper/vue";
import { Pagination } from "swiper/modules";
import "swiper/css";
import "swiper/css/pagination";

import bochten from "./components/icons/last_rit/Bochten.png";
import optrekken from "./components/icons/last_rit/Optrekken.png";
import remmen from "./components/icons/last_rit/Remmen.png";
import snelheid from "./components/icons/last_rit/Snelheid.png";

import beacon from "./components/icons/actions/Beacon.png";
import discount from "./components/icons/actions/Discount.png";
import map from "./components/icons/actions/Map.png";
import profile from "./components/icons/actions/Profile.png";
import road from "./components/icons/actions/Road.png";
import trophy from "./components/icons/actions/Trophy.png";

import battery from "./components/icons/popup/Battery.png";
import firmware from "./components/icons/popup/Firmware.png";
import reload from "./components/icons/popup/Reload.png";

import homeIcon from "./components/icons/menu/home.png";
import tripsIcon from "./components/icons/menu/rit.png";
import mapIcon from "./components/icons/menu/map.png";
import discountIcon from "./components/icons/menu/discount.png";
import moreIcon from "./components/icons/menu/settings.png";

const navItems = [
  { label: 'Home', icon: homeIcon, active: true },
  { label: 'Ritten', icon: tripsIcon, active: false },
  { label: 'Kaart', icon: mapIcon, active: false },
  { label: 'Korting', icon: discountIcon, active: false },
  { label: 'Meer', icon: moreIcon, active: false }
]

const showBeaconModal = ref(false);

const showWrapped = ref(false);

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
    icon: optrekken,
    status: "success",
  },
  {
    id: "brake",
    title: "Remmen",
    sub: "Goed gedaan!",
    icon: remmen,
    status: "success",
  },
  {
    id: "speed",
    title: "Snelheid",
    sub: "Probeer rekening te houden met je snelheid",
    icon: snelheid,
    status: "warning",
  },
  {
    id: "turns",
    title: "Bochten",
    sub: "Probeer rekening te houden met je bochten",
    icon: bochten,
    status: "warning",
  },
];

const quickActions = [
  { id: "trips", title: "Ritten", icon: road },
  { id: "challenges", title: "Uitdagingen", icon: trophy },
  { id: "beacon", title: "Beacon", icon: beacon },
  { id: "map", title: "Kaart", icon: map },
  { id: "discount", title: "Korting", icon: discount },
  { id: "profile", title: "Profiel", icon: profile },
];

</script>

<template>
  <AppShell>
    <Wrapped v-if="showWrapped" @close="showWrapped = false" />
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
          <div
            class="header-right position-relative"
            @click="showWrapped = true"
            style="cursor: pointer;"
          >
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
                    stroke="#E18835"
                    stroke-width="10"
                    stroke-dasharray="130"
                    stroke-dashoffset="300"
                    stroke-linecap="round"
                  />
                </svg>
                <div class="score-number">
                  <span class="score text-main fw-bold">57</span>
                  <span class="max-score small opacity-50 d-block">van 100</span>
                </div>
              </div>
            </div>
            <div class="col-7 dateScore">
              <div class="d-flex align-items-center gap-2 mb-1">
                <span class="dot-indicator bg-danger"></span>
                <span class="text-danger small fw-bold">Onveilig</span>
              </div>
              <h2 class="h5 text-main fw-medium mb-0">Jouw rijscore</h2>
              <p class="small text-muted mb-0">19 nov - 18 dec</p>
            </div>
          </div>
          <div
            class="beacon-status mt-4 p-2 rounded-3 d-flex align-items-center justify-content-between light-blue-section"
            @click="showBeaconModal = true"
            style="cursor: pointer;"
          >
            <div class="d-flex align-items-center gap-3">
              <div class="beacon-icon-box rounded-circle">
                <span class="text-white">✓</span>
              </div>
              <div>
                <p class="connected mb-0 fw-medium">Beacon verbonden</p>
                <p class="lastActive mb-0 x-small text-muted">
                  Laatst actief: vandaag
                </p>
              </div>
            </div>
            <span class="viewMore">›</span>
          </div>
        </div>
      </section>

      <!-- C. Afgelopen Rit -->
      <section class="px-3 mb-5">
        <h3 class="mb-3 latest_rit">Afgelopen rit</h3>
        <div class="white-section p-4 rounded-4 bg-white shadow-sm">
          <div
            class="rewind-promo p-1 px-3 rounded-pill d-inline-flex align-items-center gap-2 mb-3"
          >
            <span class="x-small fw-bold"
              >🔗 Bekijk je rit met: Rit Rewind</span
            >
          </div>

          <div class="row g-3 mb-4">
            <div v-for="card in feedbackCards" :key="card.id" class="col-6">
              <div class="feedback-card p-3 text-center rounded-4 border">
                <div
                  class="feedback-icon fs-2 mb-2"
                  :class="`icon-bg--${card.status}`"
                >
                  <img
                    :src="card.icon"
                    alt=""
                    class="feedback-img"
                  />

                </div>
                <p class="title mb-1 small">{{ card.title }}</p>
                <p class="sub x-small text-muted mb-0 line-clamp3">
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
          <h3 class="goals h2 fw-medium text-white mb-0">Doelstellingen</h3>
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
                  <div class="x-medium">Bochtenkoning</div>
                </div>
              </div>
              <div class="gauge-text">
                <p class="x-medium text-main text-center mt-3 mb-0">
                  Haal jij het beste uit je bochten?
                </p>
              </div>
            </div>
          </swiper-slide>

          <swiper-slide class="goal-slide">
            <div class="goal-widget p-4 bg-white rounded-4 shadow-sm h-100 graph">
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
        <h3 class="fastActions mb-3">Snelle acties</h3>
        <div class="actions-container bg-white p-4 shadow-sm">
          <div class="row g-3 text-center mb-4">
            <div v-for="action in quickActions" :key="action.id" class="col-4">
              <div class="action-item p-3 border rounded-4">
                <div class="action-icon mb-2 bg-gradient">
                  <img
                    :src="action.icon"
                    :alt="action.title"
                    class="action-icon-img"
                  />
                </div>
                <div class="x-small fw-medium text-main">
                  {{ action.title }}
                </div>
              </div>
            </div>
          </div>
          <BaseButton variant="primary">Bekijk alles</BaseButton>
        </div>
      </section>

      <!-- F. Nieuws -->
      <section class="mb-5">
        <h3 class="news px-3 mb-3">Nieuws</h3>
        <swiper
          :slides-per-view="'auto'"
          :space-between="16"
          class="news-swiper px-3"
        >
          <swiper-slide class="news-slide">
            <div class="news-card bg-white overflow-hidden shadow-sm h-100 d-flex">
              <div class="news-image">
                <img
                  src="https://images.unsplash.com/photo-1541899481282-d53bffe3c35d?auto=format&fit=crop&w=300&q=80"
                  alt="News"
                />
              </div>

              <div class="news-content p-3 d-flex flex-column">
                <div>
                  <h4 class="title fw-medium mb-3">Autocomfort voor je hond</h4>
                  <p class="text x-small mb-2">
                    10 tips om je hond tevreden te houden voor korte en langere ritjes!
                  </p>
                </div>

                <p class="x-small text-muted mb-3">Leestijd: 5 min.</p>

                <div>
                  <div class="d-flex justify-content-between align-items-center mb-2">
                    <div class="d-flex gap-1">
                      <span class="badge bg-gold text-dark x-small py-2 px-2">Actueel</span>
                      <span class="badge bg-gold text-dark x-small py-2 px-2">Dierendag</span>
                    </div>
                    <button
                      class="btn btn-navy d-flex align-items-center justify-content-center shadow-sm"
                      style="width: 32px; height: 32px; border-radius: 16px 0 16px 0;"
                    >
                      <span class="text-white x-medium">→</span>
                    </button>
                  </div>
                </div>
              </div>
            </div>
          </swiper-slide>

          <swiper-slide class="news-slide">
            <div class="news-card bg-white overflow-hidden shadow-sm h-100 d-flex">
              <div class="news-image">
                <img
                  src="https://images.unsplash.com/photo-1449965408869-eaa3f722e40d?auto=format&fit=crop&w=300&q=80"
                  alt="News"
                />
              </div>

              <div class="news-content p-3 d-flex flex-column">
                <div>
                  <h4 class="title fw-medium mb-2">Veilig de winter door</h4>
                  <p class="text x-small mb-2">
                    Bereid je auto voor op koude dagen met deze handige tips.
                  </p>
                </div>

                <p class="x-small text-muted mb-3">Leestijd: 5 min.</p>

                <div>
                  <div class="d-flex justify-content-between align-items-center mb-2">
                    <div class="d-flex gap-1">
                      <span class="badge bg-gold text-dark x-small py-2 px-2">Actueel</span>
                      <span class="badge bg-gold text-dark x-small py-2 px-2">Onderhoud</span>
                    </div>
                    <button
                      class="btn btn-navy d-flex align-items-center justify-content-center shadow-sm"
                      style="width: 32px; height: 32px; border-radius: 16px 0 16px 0;"
                    >
                      <span class="text-white x-small">→</span>
                    </button>
                  </div>
                </div>
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
          <div class="nav-icon mb-1">
            <img :src="homeIcon" alt="Home" class="png-icon" />
          </div>
          <div class="x-small">Home</div>
        </div>
        <div class="nav-item text-center">
          <div class="nav-icon mb-1">
            <img :src="tripsIcon" alt="Ritten" class="png-icon" />
          </div>
          <div class="x-small">Ritten</div>
        </div>
        <div class="nav-item text-center">
          <div class="nav-icon mb-1">
            <img :src="mapIcon" alt="Kaart" class="png-icon" />
          </div>
          <div class="x-small">Kaart</div>
        </div>
        <div class="nav-item text-center">
          <div class="nav-icon mb-1">
            <img :src="discountIcon" alt="Korting" class="png-icon" />
          </div>
          <div class="x-small">Korting</div>
        </div>
        <div class="nav-item text-center">
          <div class="nav-icon mb-1">
            <img :src="moreIcon" alt="Meer" class="png-icon" />
          </div>
          <div class="x-small">Meer</div>
        </div>
      </nav>
    </div>

    <div v-if="showBeaconModal" class="modal-backdrop" @click.self="showBeaconModal = false">
      <div class="modal-card beacon-modal">
        <button class="modal-close-btn" @click="showBeaconModal = false">X</button>
        <h2 class="modal-title mb-3">Beacon status</h2>

        <div class="beacon-status-card mb-4">
          <div class="d-flex align-items-center gap-3">
            <div class="beacon-bluetooth">
              <span class="beacon-icon-box">ᛒ</span>
            </div>
            <div class="flex-grow-1">
              <p class="fw-bold mb-0">Beacon verbonden</p>
              <p class="text-muted small mb-0">
                Signaalsterkte: Uitstekend
              </p>
            </div>
            <span class="status-dot"></span>
          </div>
        </div>

        <div class="row g-3 mb-4">
          <div class="col-6">
            <div class="info-card text-center">
              <img :src="battery" alt="Batterij" class="popup-icon mb-2" />
              <p class="text-main fw-medium mb-1">Batterij</p>
              <p class="text-muted mb-0">85%</p>
            </div>
          </div>

          <div class="col-6">
            <div class="info-card text-center">
              <img :src="reload" alt="Laatste sync" class="popup-icon mb-2" />
              <p class="text-main fw-medium mb-1">Laatste sync</p>
              <p class="text-muted mb-0">Zojuist</p>
            </div>
          </div>
        </div>


        <div class="firmware-card mb-4">
          <div class="d-flex gap-2">
            <img :src="firmware" alt="firmware" class="popup-icon mb-2" />
            <p class="text-main fw-medium mb-1">Firmware</p>
          </div>
          <p class="text-muted mb-0">Versie 2.4.1 (Up-to-date)</p>
        </div>

        <BaseButton variant="primary" class="w-100">
          Test verbinding
        </BaseButton>
      </div>
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
  border-style: solid; 
  border-width: 2px;
  border-color:rgba(255, 255, 255, 0.1);
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
  width: 22px;
  height: 22px;
  border-radius: 50%;
  font-size: 11px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: bold;
  border: 2px solid var(--color-navy);
  top: 30px;
  animation: badgeBounce 1s ease-in-out infinite;
}

@keyframes badgeBounce {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-5px);
  }
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
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
  background: linear-gradient(to bottom, #023F88, #012856);
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
  border: 1px solid var(--color-navy);
}

.feedback-card {
  transition: all 0.3s ease;
  font-family: 'Poppins', sans-serif;
}

.feedback-card:active {
  background: #fdfdfd;
}

.line-clamp3 {
  display: -webkit-box;
  -webkit-line-clamp: 3;
  line-clamp: 3;
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
  top: 65px;
  left: 50%;
  transform: translateX(-50%);
  color: var(--color-text-main);
}

.gauge-text {
  margin-top: 60px;
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

.nav-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 54px;
  height: 54px;
  transition: all 0.2s ease;
  color: var(--color-navy);
}

.nav-item.active {
  background-color: var(--color-gold);
  border-radius: 12px;
  color: #000;
}

.nav-icon {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 24px;
  margin-bottom: 2px;
}

.png-icon {
  width: 22px;
  height: 22px;
  object-fit: contain;
}

.x-small {
  font-size: 12px;
}

.x-medium {
  font-size: 14px;
}

/* Helpers */
.rounded-5 {
  border-radius: 2.5rem !important;
}

.bg-gold {
  background-color: var(--color-gold);
}

.feedback-icon {
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
  margin: 0 auto;
  color: #fff;
}

.text-main {
  color: var(--color-text-main);
}

.icon-bg--success {
  background-color: var(--color-success);
}

.icon-bg--warning {
  background-color: #E18835;
}

.icon-bg--stop {
  background-color: #dc3545;
}

h3.latest_rit {
  color: var(--color-text-main);
  font-size: 20px;
  font-weight: 500;
  font-family: 'Poppins', sans-serif; 

}

.rewind-promo span {
  color: var(--color-text-main);
  font-size: 15px;
  font-family: 'Poppins', sans-serif;
}

.feedback-card .title {
  font-size: 16px;
  font-weight: 400;
}

.beacon-status p.connected {
  color: var(--color-text-main);
  font-size: 16px;
}

.beacon-status .viewMore {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  font-size: 32px;
  color: var(--color-text-main);
  margin-bottom: 4px;
}

.score-number .score {
  font-size: 32px;
}

.dataScore {
  font-family: 'Poppins', sans-serif; 
}

.header-right {
  top: 80px;
  z-index: 5;
}

.goal-widget.graph {
  padding-top: 16px;
}

h3.goals {
  font-size: 20px;
}

.action-icon {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto;
  background: #012856;
}

.action-icon-img {
  width: 18px;
  height: 18px;
  object-fit: contain;
  z-index: 5;
}

.actions-container {
  border-radius: var(--radius-md);
}

.news-card {
  border-radius: var(--radius-md);
}

.news-card h4 {
  font-size: 14px;
}

.news-card button {
  padding: 20px;
  position: absolute;
  bottom: 0;
  right: 0;
}

h3.fastActions {
  font-family: 'Poppins', sans-serif; 
  color: var(--color-text-main);
  font-weight: 500;
  font-size: 20px;
}

h3.news {
  font-family: 'Poppins', sans-serif; 
  color: var(--color-text-main);
  font-weight: 500;
  font-size: 20px;
}

.news-card {
  height: 100%;
}

.news-image {
  width: 25%;
  min-width: 100px;
  min-height: 220px;
}

.news-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.news-content {
  width: 75%;
}

.news-content .title {
  color: var(--color-navy);
}

.modal-backdrop {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.55);
  z-index: 3000;
  display: flex;
  align-items: flex-end;
  justify-content: center;
}

.modal-card {
  position: relative;
  background: #fff;
  width: 100%;
  max-width: 420px;
  border-radius: 24px 24px 0 0;
  padding: 24px;
  animation: slideUp 0.25s ease-out;
}

@keyframes slideUp {
  from {
    transform: translateY(100%);
  }
  to {
    transform: translateY(0);
  }
}

.modal-title {
  font-family: 'Poppins', sans-serif;
  font-size: 22px;
  font-weight: 700;
  color: var(--color-navy);
}

.beacon-status-card {
  background: #eef6ff;
  border-radius: 16px;
  padding: 16px;
}

.beacon-status-card .flex-grow-1 {
  color: var(--color-navy);
  font-family: 'Poppins', sans-serif;
} 

.beacon-bluetooth {
  width: 54px;
  height: 54px;
  color: white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 20px;
}

.status-dot {
  width: 10px;
  height: 10px;
  background: #4caf50;
  border-radius: 50%;
}

.info-card {
  background: #fff;
  border-radius: 16px;
  padding: 16px;
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.08);
}

.info-icon {
  font-size: 22px;
  margin-bottom: 6px;
}

.firmware-card {
  background: #fff;
  border-radius: 16px;
  padding: 16px;
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.08);
}

.popup-icon {
  width: 22px;
  height: 22px;
  object-fit: contain;
}

.modal-close-btn {
  position: absolute;
  top: auto;
  right: 24px;
  background: transparent;
  border: none;
  font-size: 20px;
  cursor: pointer;
  color: var(--color-navy);
  z-index: 15;
}

</style>
