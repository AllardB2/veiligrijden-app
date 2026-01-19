<script setup>
import { onMounted, onUnmounted, computed } from 'vue';
import { ref, defineEmits } from "vue";
import BaseButton from "./components/shared/BaseButton.vue";

import lines from "./components/images/lines.png";
import chameleon from "./components/images/chameleon.png";
import chameleonOrange from "./components/images/chameleon_orange.png";
import chameleonBlue from "./components/images/chameleon_blue.png";
import trendDown from "./components/icons/shared/trend_down.png";
import leafIcon from "./components/icons/shared/leaf_green.png";
import dropIcon from "./components/icons/shared/drop_blue.png";
import whatsappIcon from "./components/icons/share/whatsapp.png";
import instagramIcon from "./components/icons/share/instagram.png";
import shareIcon from "./components/icons/share/share_arrow.png";
import car from "./components/icons/distance/car.png";
import map2 from "./components/icons/distance/map.png";
import clock from "./components/icons/distance/clock.png";
import logo from "./components/images/ANWB_logo.png";
import swiper from "./components/images/Swiper.png";
import carWhite from "./components/icons/green/car_white.png";
import cloudWarning from "./components/icons/green/cloud_warning.png";
import gas from "./components/icons/green/gas.png";
import clock2 from "./components/icons/green/clock2.png";
import lamp from "./components/icons/green/lamp.png";
import shower from "./components/icons/green/shower.png";
import check from "./components/icons/discount/check.png";
import focus from "./components/icons/discount/focus.png";
import stars from "./components/icons/discount/stars.png";
import turn from "./components/icons/discount/turn.png";
import warning from "./components/icons/discount/warning.png";
import ANWBmap from "./components/icons/highlight/ANWB_map.png";
import checkmarkGreen from "./components/icons/highlight/checkmarkGreen.png";
import star from "./components/icons/highlight/star.png";
import car_on_road from "./components/images/car_on_road.jpg";
import car_sunset from "./components/images/car_sunset.jpg";

const deadline = new Date('2026-01-31T23:59:59');
const now = ref(new Date());

let timer;

onMounted(() => {
  timer = setInterval(() => {
    now.value = new Date();
  }, 1000);
});

onUnmounted(() => {
  clearInterval(timer);
});

const timeLeft = computed(() => {
  const diff = deadline.getTime() - now.value.getTime();
  
  if (diff <= 0) return "Challenge verlopen";

  const days = Math.floor(diff / (1000 * 60 * 60 * 24));
  const hours = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
  const minutes = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60));
  const seconds = Math.floor((diff % (1000 * 60)) / 1000);

  const h = String(hours).padStart(2, '0');
  const m = String(minutes).padStart(2, '0');
  const s = String(seconds).padStart(2, '0');

  return `${days}d ${h}u ${m}m ${s}s`;
});

const emit = defineEmits(["close"]);
const currentIndex = ref(0);
const totalScreens = 14;
const scrollContainer = ref(null); 

const onScroll = (e) => {
  const scrollLeft = e.target.scrollLeft;
  const width = e.target.offsetWidth;
  currentIndex.value = Math.round(scrollLeft / width);
};

const nextPage = () => {
  if (currentIndex.value < totalScreens - 1) {
    currentIndex.value++;
    const targetX = currentIndex.value * window.innerWidth;
    
    if (scrollContainer.value) {
      scrollContainer.value.scrollTo({
        left: targetX,
        behavior: 'smooth'
      });
    }
  }
};

const activeEcoTip = ref(null);

const ecoDetails = {
  boom: {
    title: "CO2 opname door een boom",
    text: "Een gemiddelde boom neemt per maand ongeveer 1,6 tot 2,5 kg CO2 op. Jouw besparing van deze week staat dus gelijk aan wat een boom een volle maand lang uit de lucht filtert!"
  },
  lamp: {
    title: "Lamp laten branden",
    text: "Een moderne LED-lamp verbruikt weinig, maar 3,2kg CO2 staat gelijk aan het non-stop laten branden van een gemiddelde lamp voor 12 uur. Kleine beetjes maken een groot verschil!"
  },
  douche: {
    title: "Warm douchen",
    text: "Water opwarmen kost veel energie. Jouw zuinige rijstijl heeft net zoveel CO2 bespaard als de uitstoot van een warme douche van 5 minuten."
  },

  kilometers: {
    title: "Totaal aantal kilometers",
    text: "Dit zijn alle kilometers die je deze week hebt afgelegd. Dit is gebaseerd op de ritten waarbij de ANWB-app actief was."
  },
  ritten: {
    title: "Aantal ritten",
    text: "Elke keer dat je de auto start en naar een bestemming rijdt, telt dit als één rit. Korte stops van minder dan 5 minuten worden vaak als één rit gezien."
  },
  ritduur: {
    title: "Gemiddelde ritduur",
    text: "Gemiddeld zat je deze week 25 minuten per rit in de auto, dat is 15 minuten vergeleken met vorige week. Dit helpt je inzicht te krijgen in je reisgewoontes."
  },

  punten: {
    title: "Jouw verdiende punten",
    text: `Deze week heb je 37 punten verzameld! <br><br> 
           • <span class="highlight-num">12 punten</span>: Veilig bochtenwerk <br> 
           • <span class="highlight-num">10 punten</span>: Beheerste remacties <br> 
           • <span class="highlight-num">15 punten</span>: Focus (geen telefoongebruik) <br>
           Wissel je punten in bij de ANWB shop en ontvang leuke cadeau's!`,
    titleColor: "#012856"
  },

  ritScore: {
    title: "Jouw ritscore: 100/100",
    text: `Een perfecte score! Deze rit blonk uit op de volgende onderdelen: <br><br>
           • <span class="highlight-num">Snelheid</span>: Je hebt je overal aan de limiet gehouden. <br>
           • <span class="highlight-num">Remmen</span>: Geen enkele keer te hard op de rem getrapt. <br>
           • <span class="highlight-num">Optrekken</span>: Zeer geleidelijk en zuinig weggereden. <br><br>
           Houd dit vol voor een maximale premiekorting aan het eind van de maand!`,
    titleColor: "#3FA261"
  },

  bochten_challenge: {
    title: "Challenge: Goede bochten",
    text: "Een goede bocht betekent dat je met een constante, rustige snelheid de bocht doorgaat zonder abrupt te sturen of te remmen. Haal er 20 deze week!"
  },
  score_challenge: {
    title: "Challenge: Ritscore 80+",
    text: "Probeer twee ritten te rijden waarbij je focus, snelheid en remgedrag samen een score van 80 of hoger opleveren. Een mooie uitdaging voor een veilige rit!"
  },
  remmen_challenge: {
    title: "Challenge: Goed remmen",
    text: "Goed remmen houdt in dat je vroegtijdig gas loslaat en de auto geleidelijk tot stilstand brengt. Vermijd harde remacties om deze challenge te voltooien."
  },

  regio: {
    title: "Jouw Regio: Groningen",
    text: "In jouw omgeving (Groningen) rijden bestuurders gemiddeld erg veilig. Jij scoort momenteel in de top 80%.",
    type: 'map',
    location: "Molenkamp 3A, De Groeve"
  }
};

const openEcoPopup = (type) => {
  activeEcoTip.value = ecoDetails[type];
};
</script>

    <template>
      <div class="wrapped-root">
      <div class="bg-image"></div>

        <div class="sticky-bg" v-if="![0, 13].includes(currentIndex)">
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

        <button class="close-btn" @click="emit('close')">✕</button>

        <div class="progress-dots">
          <div
            v-for="i in totalScreens"
            :key="i"
            class="progress-dot"
            :class="{ 'progress-dot--active': currentIndex === i - 1 }"
          ></div>
        </div>

        <div class="wrapped-overlay" @scroll="onScroll" ref="scrollContainer">
          <div class="fixed-swiper-container" v-if="currentIndex < totalScreens - 1">
      <img :src="swiper" alt="Swiper" class="swiper-icon" />
    </div>

      <section class="screen screen-1 poppins">
        <div class="content">
          <img :src="logo" alt="Logo" class="screen1-logo" />
          <p>Week 47</p>
          <h1>Dit is hoe jij gereden hebt deze week</h1>
        </div>

        <img :src="lines" alt="" class="bg-lines-center" />

        <div class="chameleonGreenMirror">
          <img :src="chameleon" alt="Chameleon" class="chameleon-img-home" />
        </div>

        <BaseButton 
          variant="primary" 
          class="wrapped-button-bottom" 
          @click="nextPage"
        >
          Bekijk mijn week
        </BaseButton>
      </section>

      <section class="screen screen-2">
        <div class="content">
          <h1 class="screen-title">Zo veel heb je gereden</h1>

          <div class="stats-card">
            <div 
              class="stat-row clickable-row" 
              v-for="stat in [
                { id: 'kilometers', icon: car, label: 'Totaal aantal kilometers', value: '183 km' },
                { id: 'ritten', icon: map2, label: 'Aantal ritten', value: '14 ritten' },
                { id: 'ritduur', icon: clock, label: 'Gemiddelde ritduur', value: '25 min' }
              ]" 
              :key="stat.label"
              @click="openEcoPopup(stat.id)"
            >
              <div class="icon-box">
                <img :src="stat.icon" alt="" class="stat-icon" />
              </div>
              <div class="stat-text">
                <div class="stat-label">{{ stat.label }}</div>
                <div class="stat-value">{{ stat.value }}</div>
              </div>
              <span class="info-hint">i</span>
            </div>

            <div class="summary">
              Je reed 183 km in 14 ritten, netjes bezig!
            </div>
          </div>
        </div>
      </section>

      <section class="screen screen-2a">
        <div class="content">
          <h1 class="screen-title">Je bent een echte bochtenkoning!</h1>

          <div class="section-label">
            <h3>Aantal genomen bochten</h3>
          </div>

          <div class="turns-card">
            <p class="turns-highlight">Je hebt deze week <strong>85 goede bochten</strong> genomen!</p>
            
            <div class="gauge-container-large">
              <svg viewBox="0 0 100 55" class="gauge-svg">
                <path d="M 10 50 A 40 40 0 0 1 90 50" fill="none" stroke="#f0f2f5" stroke-width="14" stroke-linecap="square" />
                <path d="M 10 50 A 40 40 0 0 1 85 30" fill="none" stroke="#012856" stroke-width="14" stroke-linecap="round" />
              </svg>
              <div class="gauge-text-center">
                <span class="gauge-val-big">85/100</span>
                <span class="gauge-lab-small">Aantal goede bochten</span>
              </div>
            </div>
          </div>

          <div class="section-label">
            <h3>Meest gereden type bocht</h3>
          </div>

          <div class="type-card">
            <p class="type-highlight"><strong>63%</strong> van jouw bochten waren <strong>Linkerbochten!</strong></p>
            
            <div class="split-bar-container">
              <div class="split-bar">
                <div class="bar-left" style="width: 63%;">63%</div>
                <div class="bar-right" style="width: 37%;">37%</div>
              </div>
              <p class="bar-caption italic">Linkerbochten vs Rechterbochten</p>
            </div>
          </div>
        </div>
      </section>

      <section class="screen screen-3">
        <div class="content">
          <h1 class="screen-title">Zo groen reed jij</h1>

          <div class="eco-card">
            <p class="eco-subtitle">Jouw rijoverzicht</p>
            <h3 class="eco-title">Impact op het milieu</h3>

            <div class="eco-score">
              <div class="eco-icon">
                <img :src="carWhite" alt="Auto icon" class="eco-icon-img" />
              </div>
              <div>
                <div class="eco-score-value">88/100</div>
                <div class="eco-score-label">Eco score</div>
              </div>
            </div>

            <div class="eco-stats">
              <div class="eco-stat">
                <img :src="cloudWarning" alt="CO2" class="eco-stat-icon" />
                <div class="eco-stat-label">Minder CO2</div>
                <div class="eco-stat-value">12%</div>
              </div>

              <div class="eco-stat">
                <img :src="gas" alt="Brandstof" class="eco-stat-icon" />
                <div class="eco-stat-label">Brandstof</div>
                <div class="eco-stat-value">4.5L</div>
              </div>
            </div>

            <div class="eco-saved">
              Je bespaarde <strong>3,2KG CO2</strong><br />
              dat staat gelijk aan:
            </div>

            <div class="eco-comparisons">
              <div class="eco-item" @click="openEcoPopup('boom')">
                <div class="eco-item-icon-bg">
                  <img :src="clock2" alt="Boom" class="eco-item-icon" />
                </div>
                <div class="eco-item-text">
                  <strong>1 maand</strong>
                  <p>CO2 opname door een boom</p>
                </div>
                <span class="info-hint">i</span>
              </div>

              <div class="eco-item" @click="openEcoPopup('lamp')">
                <div class="eco-item-icon-bg">
                  <img :src="lamp" alt="Lamp" class="eco-item-icon" />
                </div>
                <div class="eco-item-text">
                  <strong>12 uur</strong>
                  <p>een lamp laten branden</p>
                </div>
                <span class="info-hint">i</span>
              </div>

              <div class="eco-item" @click="openEcoPopup('douche')">
                <div class="eco-item-icon-bg">
                  <img :src="shower" alt="Douche" class="eco-item-icon" />
                </div>
                <div class="eco-item-text">
                  <strong>5 minuten</strong>
                  <p>warm douchen</p>
                </div>
                <span class="info-hint">i</span>
              </div>
            </div>
          </div>
        </div>
      </section>

      <section class="screen screen-4">
        <div class="content">
          <h1 class="screen-title">Opgebouwde korting</h1>

          <div class="discount-card">
            <div>
              <img :src="stars" alt="stars" class="stars" />
            </div>
            <p class="discount-subtitle">TOTAAL BESPAARD</p>
            <div class="discount-amount">€3,75</div>

            <div class="discount-points clickable-points" @click="openEcoPopup('punten')">
              <img :src="check" alt="Boom" class="discount-item-icon" /> 37 punten verdiend
            </div>
          </div>

          <div class="discount-list">
            <div class="discount-item">
              <div class="discount-icon">
                <img :src="turn" alt="Boom" class="discount-item-icon" />
              </div>
              <div class="discount-text">
                <strong>+ €1,20</strong><br />
                door veilige bochten
              </div>
            </div>

            <div class="discount-item">
              <div class="discount-icon">
                <img :src="warning" alt="Boom" class="discount-item-icon" />
              </div>
              <div class="discount-text">
                <strong>+ €0,95</strong><br />
                door weinig hard remmen
              </div>
            </div>

            <div class="discount-item">
              <div class="discount-icon">
                <img :src="focus" alt="Boom" class="discount-item-icon" />
              </div>
              <div class="discount-text">
                <strong>+ €1,60</strong><br />
                door 92% focus score
              </div>
            </div>
          </div>
        </div>
      </section>

      <section class="screen screen-5">
        <div class="content">
          <h1 class="screen-title">Hoogtepunt van jouw periode</h1>
          
          <div class="highlight-card">
            <div class="highlight-header">
              <h3>Jouw beste moment(en)</h3>
            </div>

            <div class="inner-card">
              <div class="trip-info">
                <div class="trip-header">
                  <div class="star-circle">
                    <img :src="star" alt="Star" class="small-icon" />
                  </div>
                  <div class="trip-title-text">
                    <strong>Je veiligste rit:</strong>
                    <span>Maandagavond om 18:12</span>
                  </div>
                </div>

                <div class="map-visual" :style="{ backgroundImage: `url(${ANWBmap})` }">
                  <div class="score-badge clickable-badge" @click="openEcoPopup('ritScore')">
                    <img :src="checkmarkGreen" alt="Check" class="check-icon" />
                    <span>Score: 100/100</span>
                    <span class="info-hint-white">i</span>
                  </div>
                </div>
              </div>

              <div class="corner-info">
                <div class="corner-header">
                  <div class="corner-icon-bg">
                    <img :src="turn" alt="Turn" class="small-icon" />
                  </div>
                  <div class="corner-text">
                    <strong>Je scherpste bocht</strong>
                    <p>37° onder perfecte controle</p>
                  </div>
                </div>
                <div class="corner-feedback">Zo soepel als boter!</div>
              </div>
            </div>

            <p class="comparison-footer">Je reed 15% veiliger dan vorige week.</p>
          </div>
        </div>
      </section>

      <section class="screen screen-6">
        <div class="content">
          <h1 class="screen-title">Remmen met controle</h1>

          <div class="brake-card">
            <p class="brake-subtitle">Jouw statistieken</p>
            <h3 class="brake-title">Aantal harde remacties</h3>

            <div class="brake-circle-container">
              <div class="brake-circle">
                <span class="brake-number">5</span>
              </div>
            </div>

            <div class="chameleon-badge">
              <img :src="chameleon" alt="Chameleon" class="chameleon-img" />
              <div class="chameleon-text">
                3 minder dan vorige keer
              </div>
            </div>

            <div class="divider"></div>

            <div class="brake-footer">
              <p>
                Je remde <strong>5x hard</strong>, dat is 3x minder dan de vorige keer. 
                <span class="highlight-green">Top gedaan!</span>
              </p>
            </div>
          </div>
        </div>
      </section>

      <section class="screen screen-6a poppins">
        <div class="content">
          <h1 class="screen-title">Hoe gefocust reed je?</h1>

          <div class="focus-card">
            <h3 class="focus-status">Je hebt een sterke focus</h3>
            
            <div class="gauge-container">
              <svg viewBox="0 0 100 50" class="gauge-svg">
                <path d="M 10 50 A 40 40 0 0 1 90 50" fill="none" stroke="#f0f2f5" stroke-width="12" stroke-linecap="round" />
                <path d="M 10 50 A 40 40 0 0 1 35 14" fill="none" stroke="#012856" stroke-width="12" stroke-linecap="round" />
              </svg>
              <div class="gauge-text">
                <span class="gauge-value">4.5/10</span>
                <span class="gauge-label">Focus score</span>
              </div>
            </div>
          </div>

          <h2 class="sub-title-left">Telefoongebruik</h2>

          <div class="phone-use-card">
            <div class="phone-text">
              <p>Afleiding tijdens rijden</p>
              <div class="time">
                <p>30 minuten</p>
              </div>
            </div>
            <img :src="chameleonBlue" alt="Blauwe Kameleon" class="chameleon-inline" />
          </div>

          <p class="focus-footer">
            Je bleef <span class="highlight-gold">40%</span> van de tijd gefocust op de weg, probeer de volgende keer je telefoon minder te gebruiken tijdens het verkeer en haal een hogere score!
          </p>
        </div>
      </section>

      <section class="screen screen-7">
        <div class="content">
          <h1 class="screen-title">Hoe je je ontwikkelt</h1>
          
          <div class="dev-card">
            <div class="trend-icon-bg">
              <img :src="trendDown" alt="Trend omlaag" class="trend-icon" />
            </div>

            <h2 class="dev-status">Je rijdt <span class="text-underline">onveiliger</span> dan vorige periode</h2>

            <div class="region-box clickable-region" @click="openEcoPopup('regio')">
              <div class="region-header">
                <span class="region-label">JOUW REGIO</span>
                <span class="region-badge">Top 80%</span>
              </div>
              
              <div class="progress-bar-bg">
                <div class="progress-bar-fill" style="width: 30%;"></div>
              </div>

              <div class="progress-labels">
                <span>Minder veilig</span>
                <span>Veiliger</span>
              </div>
              <p class="region-desc">Je zit in de <strong>top 80%</strong> veiligste rijders van jouw regio</p>
            </div>

            <img :src="chameleonOrange" alt="Oranje Kameleon" class="chameleon-orange" />
          </div>
        </div>
      </section>

      <section class="screen screen-tip">
        <div class="content">
          <h1 class="screen-title">Jouw persoonlijke tip</h1>

          <div class="tip-card">
            <div class="speech-bubble">
              <p>Probeer bochten net iets rustiger in te gaan, daar kun je nog winst pakken.</p>
            </div>

            <div class="tip-character">
              <img :src="chameleon" alt="Kameleon tip" class="chameleon-img-tip" />
            </div>
          </div>
        </div>
      </section>

      <section class="screen screen-7">
        <div class="content">
          <h1 class="screen-title">Bekijk hier je nieuwe challenges!</h1>

          <div class="stats-card">
            <div 
              class="stat-row clickable-row" 
              v-for="stat in [
                { id: 'bochten_challenge', icon: car, label: 'Maak 20 goede bochten', value: '0/20' },
                { id: 'score_challenge', icon: map2, label: 'Haal een ritscore van min. 80+', value: '0/2' },
                { id: 'remmen_challenge', icon: clock, label: 'Rem 30 keer goed', value: '0/30' }
              ]" 
              :key="stat.label"
              @click="openEcoPopup(stat.id)"
            >
              <div class="icon-box">
                <img :src="stat.icon" alt="" class="stat-icon" />
              </div>
              <div class="stat-text">
                <div class="stat-label">{{ stat.label }}</div>
                <div class="stat-value">{{ stat.value }}</div>
              </div>
              <span class="info-hint">i</span>
            </div>

            <div class="summary">
              Tijd over: <br> 
              <span class="countdown-timer">{{ timeLeft }}</span>
            </div>
          </div>
        </div>
      </section>

      <section class="screen screen-8">
        <div class="content">
          <h1 class="screen-title">Jij bent een...</h1>
          
          <div class="profile-card">
            <div class="image-container-large">
              <img :src="car_on_road" alt="Auto op de weg" class="profile-image" />
              <span class="top-badge-floating">TOP 5%</span>
            </div>

            <div class="profile-details">
              <h2 class="profile-name">Soepele Cruiser</h2>
              <p class="profile-desc">Op basis van jouw rijgedrag over de afgelopen 10 dagen.</p>

              <div class="tag-row">
                <div class="status-tag"><img :src="leafIcon" /> Zuinig</div>
                <div class="status-tag"><img :src="dropIcon" /> Vloeiend</div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <section class="screen screen-9">
        <div class="content">
          <h1 class="screen-title">Deel je wrapped</h1>

          <div class="share-card">
            <div class="share-card-header"><span class="top-badge-inline">TOP 5%</span></div>
            <div class="share-card-body">
              <p class="share-subtitle">Dit is jouw december</p>
              <h2 class="share-main-title">De Soepele Cruiser</h2>

              <div class="share-stats">
                <div class="share-stat">
                  <span class="share-val">3.450</span>
                  <span class="share-lab">Kilometers</span>
                </div>
                <div class="share-stat">
                  <span class="share-val color-green">88</span>
                  <span class="share-lab">Eco-score</span>
                </div>
              </div>

              <div class="image-container-small">
                <img :src="car_sunset" alt="Rit overzicht" class="share-image" />
              </div>

              <div class="share-footer">
                <span class="footer-brand">Ik rijd veiliger met <strong>ANWB</strong></span>
                <img :src="checkmarkGreen" alt="Check" class="check-icon" />
              </div>
            </div>
          </div>

          <div class="social-actions">
            <div class="social-item"><div class="social-circle"><img :src="whatsappIcon" /></div><span>WhatsApp</span></div>
            <div class="social-item"><div class="social-circle"><img :src="instagramIcon" /></div><span>Instagram</span></div>
            <div class="social-item rel"><div class="social-circle"><img :src="shareIcon" /></div><span>Delen</span></div>
          </div>
        </div>
      </section>

      <section class="screen screen-end">
        <div class="content">
          <h1 class="screen-title">Tot de volgende wrapped!</h1>
          <div class="white-card small-padding">
            <h3>Tot over 10 dagen!</h3>
            <p>Blijf veilig rijden, ik houd het allemaal persoonlijk voor je bij!</p>
          </div>
          
          <div class="center-chameleon">
            <img :src="chameleon" class="large-chameleon" />
          </div>

          <BaseButton variant="primary" class="wrapped-button" @click="emit('close')">Terug naar home</BaseButton>
        </div>
      </section>
      <Transition name="fade">
        <div v-if="activeEcoTip" class="eco-popup-overlay" @click="activeEcoTip = null">
          <div class="eco-popup-content" @click.stop>
            <h3>{{ activeEcoTip.title }}</h3>
            
            <div v-if="activeEcoTip.type === 'map'" class="map-container">
              <iframe
                width="100%"
                height="100%"
                frameborder="0" 
                style="border:0"
                :src="`https://maps.google.com/maps?q=Hanzehogeschool%20Groningen&t=&z=15&ie=UTF8&iwloc=&output=embed`"
                allowfullscreen
              ></iframe>
              <div class="marker-pulse-overlay"></div>
            </div>

            <p v-html="activeEcoTip.text"></p>
            
            <BaseButton variant="primary" @click="activeEcoTip = null">Begrepen</BaseButton>
          </div>
        </div>
      </Transition>
    </div>
  </div>
</template>

<style scoped>
.wrapped-root {
  position: fixed;
  inset: 0;
  z-index: 5000;
  background: #012856;
  overflow: hidden;
}

.sticky-bg {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 0;
  pointer-events: none;
}

.close-btn {
  position: fixed;
  top: 16px;
  right: 16px;
  z-index: 6000;
  background: white;
  color: #012856;
  border: none;
  padding: 8px 14px;
  border-radius: 999px;
  font-size: 18px;
  cursor: pointer;
}

.chameleonGreenMirror {
  position: relative;
}

.chameleonGreenMirror img{
  top: 550px;
  position: absolute;
  right: 0;
  height: 130px;
}

.progress-dots {
  position: fixed;
  top: 20px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 8px;
  z-index: 5500;
}

.progress-dot {
  width: 8px;
  height: 8px;
  background: rgba(255, 255, 255, 0.75);
  border-radius: 50%;
  transition: all 0.3s ease;
}

.progress-dot--active {
  background: var(--color-gold);
  width: 24px;
  border-radius: 4px;
}

.wrapped-overlay {
  height: 100vh;
  display: flex;
  flex-direction: row;
  overflow-x: scroll;
  overflow-y: hidden;
  scroll-snap-type: x mandatory;
  scroll-behavior: smooth;
  scrollbar-width: none;
}
.wrapped-overlay::-webkit-scrollbar {
  display: none;
}

.screen {
  min-width: 100vw;
  height: 100vh;
  scroll-snap-align: start;
  display: flex;
  align-items: flex-start;
  justify-content: center;
  color: white;
  text-align: center;
  padding-top: 60px;
}

.screen-1 {
  position: relative;
  display: flex;
  flex-direction: column;
  height: 100vh;
}

.wrapped-button-bottom {
  position: absolute;
  bottom: 80px;
  left: 50%;
  transform: translateX(-50%);
  width: 85%;
  z-index: 10;
}

.chameleonGreenMirror {
  margin-top: auto;
  line-height: 0;
}

.chameleon-img-home {
  width: 200px;
  transform: scaleX(-1);
  display: block;
}

.content {
  max-width: 360px;
  width: 100%;
  padding: 24px;
  position: relative;
  z-index: 1;
  color: #000;
}

.screen-1 p {
  color: white;
  font-style: italic;
  font-family: 'Poppins', sans-serif;
}

.screen-1 h1 {
  color: white;
  font-family: 'Poppins', sans-serif;
  font-weight: 700;
}

.screen-1 .screen1-logo {
  display: block;
  margin: 24px auto 24px auto;
  width: 175px;
  height: auto;
}

.screen-1 h1 {
  color: white;
  font-style: italic;
  margin-bottom: 8px;
  font-weight: 700;
}

.screen-1 p {
  color: white;
}

.stat-label {
  font-size: 12px;
  color: #575757;
  font-weight: 500;
}

.bg-lines-center {
  position: absolute;
  top: 60%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 99.5vw;
  z-index: 0;
  pointer-events: none;
  height: 300px;
}

.screen-1 .content {
  position: relative;
  z-index: 2;
}

.chameleonGreenMirror {
  z-index: 1;
}

.stat-value {
  font-size: 24px;
  color: #012856;
  font-weight: 700;
}

.stat-icon {
  width: 20px;
  height: 20px;
  object-fit: contain;
}


.actions-container {
  border-radius: var(--radius-md);
}

.action-item {
  transition: transform 0.2s;
  cursor: pointer;
}

.action-item:active {
  transform: scale(0.95);
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

.x-small { font-size: 12px; }
.fw-medium { font-weight: 500; }

.screen-title {
  font-size: 32px;
  margin-bottom: 24px;
  color: white;
  font-family: 'Poppins', sans-serif;
  font-weight: 700;
}

.stats-card {
  background: white;
  color: #012856;
  border-radius: 16px;
  padding: 24px;
  width: 100%;
  max-width: 360px;
  display: flex;
  flex-direction: column;
  gap: 16px;
  box-shadow: 0 4px 20px rgba(0,0,0,0.1);
}

.stat-row {
  display: flex;
  align-items: center;
  gap: 16px;
  justify-content: flex-start;
  text-align: left;
  width: 100%;
  padding: 12px 0;
  border-bottom: 1px solid #E0E0E0;
}

.stat-row:last-of-type {
  border-bottom: none;
}

.icon-box {
  width: 48px;
  height: 48px;
  border-radius: 50%;
  background: #EAF4FF;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 20px;
  flex-shrink: 0;
  margin: 0;
}

.stat-text {
  display: flex;
  flex-direction: column;
  justify-content: center;
  text-align: left;
}

.summary {
  margin-top: 12px;
  background: #eef6ff;
  color: #012856;
  padding: 16px;
  border-radius: 8px;
  text-align: center;
  font-weight: 700;
  font-size: 16px;
}

.countdown-timer {
  display: block;
  font-size: 1.1rem;
  color: #E2AD1F;
  font-weight: 700;
  margin-top: 4px;
  font-variant-numeric: tabular-nums;
}

.screen-2 h1 {
    color:white;
    font-size: 32px;
    font-family: 'Poppins', sans-serif;
    font-weight: 700;
}

.eco-card {
  background: #ffffff;
  border-radius: 20px;
  padding: 24px;
  color: #012856;
  box-shadow: 0 4px 20px rgba(0,0,0,0.1);
  text-align: left;
}

.eco-subtitle {
  font-size: 14px;
  color: #575757;
  font-weight: 500;
  margin-bottom: 0;
}

.eco-title {
  font-size: 20px;
  font-weight: 700;
  margin-bottom: 16px;
}

.eco-score {
  display: flex;
  align-items: center;
  gap: 16px;
  margin-bottom: 16px;
}

.eco-icon {
  width: 48px;
  height: 48px;
  background: #3FA261;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 22px;
}

.eco-score-value {
  font-size: 26px;
  font-weight: 700;
}

.eco-score-label {
  font-size: 14px;
  color: #4caf50;
  font-weight: 600;
}

.eco-stats {
  display: flex;
  gap: 12px;
  margin-bottom: 8px;
}

.eco-stat {
  flex: 1;
  background: #EAF4FF;
  padding: 12px;
  border-radius: 12px;
  text-align: center;
}

.eco-stat-label {
  font-size: 12px;
  color: #575757;
  margin-bottom: 0;
  font-weight: 500;
}

.eco-stat-value {
  font-size: 18px;
  font-weight: 700;
}

.eco-saved {
  font-size: 14px;
  margin-bottom: 18px;
}

.eco-saved strong {
  color: #3FA261;
}

.eco-comparisons {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.eco-item {
  display: flex;
  align-items: flex-start;
  gap: 12px;
  font-size: 12px;
  color: #012856;
}

.eco-item strong {
  display: flex;
  align-items: flex-start;
  gap: 12px;
  font-size: 14px;
  color: #012856;
}

.eco-item p {
  margin-bottom: 8px;
  color: #575757;
}

.eco-item-icon-bg {
  width: 36px;
  height: 36px;
  border-radius: 50%;
  background: #f2f7ff;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.eco-item {
  cursor: pointer;
  transition: background 0.2s;
}
.eco-item:active {
  background: #f0f7f2;
  border-radius: 8px;
}

.eco-popup-overlay {
  position: fixed;
  inset: 0;
  background: rgba(1, 40, 86, 0.8);
  z-index: 7000;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 24px;
  backdrop-filter: blur(4px);
}

.eco-popup-content {
  background: white;
  border-radius: 20px;
  padding: 32px 24px;
  width: 100%;
  max-width: 320px;
  text-align: center;
  color: #012856;
}

.eco-popup-content h3 {
  font-weight: 800;
  margin-bottom: 16px;
  font-size: 20px;
}

.eco-popup-content p {
  font-size: 15px;
  line-height: 1.6;
  margin-bottom: 24px;
  color: #575757;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.3s ease;
}
.fade-enter-from, .fade-leave-to {
  opacity: 0;
}

.stars {
  position: absolute;
  right: -20px;
  top: -20px;
  height: 50px;
  width: auto;
}

.eco-item-icon {
  width: 18px;
  height: 18px;
  object-fit: contain;
}

.eco-item-text {
  line-height: 1.4;
}

.eco-icon-img {
  width: 22px;
  height: 22px;
  object-fit: contain;
}

.eco-stat {
  flex: 1;
  background: #f2f7ff;
  padding: 12px;
  border-radius: 12px;
  text-align: left;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 6px;
}

.eco-stat-icon {
  width: 22px;
  height: 22px;
  object-fit: contain;
  margin-bottom: 4px;
}

.discount-card {
  background: #ffffff;
  border-radius: 20px;
  padding: 28px 24px;
  text-align: center;
  box-shadow: 0 4px 20px rgba(0,0,0,0.1);
  margin-bottom: 24px;
  position: relative;
}

.discount-subtitle {
  font-size: 12px;
  letter-spacing: 1px;
  color: #9aa6b2;
  margin-bottom: 8px;
  font-weight: 600;
}

.discount-amount {
  font-size: 54px;
  font-weight: 800;
  color: #012856;
  margin-bottom: 16px;
  font-family: 'Poppins', sans-serif;

}

.discount-points {
  display: inline-block;
  background: #f2f4f7;
  padding: 8px 14px;
  border-radius: 999px;
  font-size: 14px;
  font-weight: 600;
  color: #012856;
  border:#EEEFF1;
  border-style: solid;
  border-width: 2px;

}

.discount-list {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.discount-item {
  background: #ffffff;
  border-radius: 14px;
  padding: 16px;
  display: flex;
  gap: 14px;
  align-items: center;
  box-shadow: 0 2px 12px rgba(0,0,0,0.08);
}

.discount-icon {
  width: 44px;
  height: 44px;
  background: #eef6ff;
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 20px;
  flex-shrink: 0;
}

.discount-text {
  font-size: 14px;
  color: #012856;
  line-height: 1.4;
  text-align: start;
}

.discount-item-icon {
  width: 18px;
  height: 18px;
  object-fit: contain;
  margin-right: 4px;
}

.highlight-card {
  width: 100%;
  max-width: 360px;
}

.highlight-header {
  display: flex;
  gap: 8px;
  color: white;
}

.highlight-header h3 {
  font-weight: 700;
  font-size: 22px;
  margin-bottom: 10px;
  font-family: 'Poppins', sans-serif;

}

.inner-card {
  background: white;
  border-radius: 12px;
  padding: 20px;
  text-align: left;
  box-shadow: 0 4px 15px rgba(0,0,0,0.2);
}

.trip-header {
  display: flex;
  gap: 12px;
  margin-bottom: 15px;
}

.star-circle {
  width: 36px;
  height: 36px;
  background: #FBC02D;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.trip-title-text {
  display: flex;
  flex-direction: column;
}

.trip-title-text strong {
  font-size: 16px;
  color: #012856;
}

.trip-title-text span {
  font-size: 14px;
  color: #757575;
}

.map-visual {
  position: relative;
  height: 160px;
  background-color: #f0f4f8;
  border-radius: 8px;
  margin-bottom: 20px;
  overflow: hidden;
  background-position: center;
  background-size: cover;
  background-repeat: no-repeat;
}

.map-bg {
  position: relative;
  width: 100%;
  height: 100%;
}

.car-overlay {
  position: absolute;
  bottom: 30px;
  right: 45px;
  width: 30px;
  transform: rotate(-90deg);
}

.score-badge {
  position: absolute;
  bottom: 15px;
  left: 50%;
  transform: translateX(-50%);
  background: white;
  padding: 8px 16px;
  border-radius: 25px;
  display: flex;
  align-items: center;
  gap: 8px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  white-space: nowrap;
}

.score-badge span {
  font-weight: 800;
  color: #012856;
}

.check-icon {
  width: 18px;
  height: 18px;
}

.corner-header {
  display: flex;
  gap: 12px;
  align-items: center;
  border-top: 1px solid #eee;
  padding-top: 15px;
}

.corner-icon-bg {
  width: 36px;
  height: 36px;
  background: #EAF4FF;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.corner-text strong {
  display: block;
  font-size: 16px;
  color: #012856;
}

.corner-text p {
  margin: 0;
  font-size: 14px;
  color: #757575;
}

.corner-feedback {
  color: #3FA261;
  font-weight: 700;
  margin-top: 10px;
  margin-left: 48px;
  font-family: 'Poppins', sans-serif;
}

.comparison-footer {
  color: rgba(255,255,255,0.7);
  font-size: 12px;
  margin-top: 16px;
}

.small-icon {
  width: 18px;
  height: 18px;
  object-fit: contain;
}

.brake-card {
  background: white;
  border-radius: 20px;
  padding: 32px 24px;
  color: #012856;
  text-align: left;
  box-shadow: 0 4px 20px rgba(0,0,0,0.1);
  position: relative;
}

.brake-subtitle {
  font-size: 14px;
  color: #575757;
  margin-bottom: 4px;
  font-family: 'Poppins', sans-serif;
}

.brake-title {
  font-size: 20px;
  font-weight: 700;
  margin-bottom: 30px;
  font-family: 'Poppins', sans-serif;
}

.brake-circle-container {
  display: flex;
  justify-content: center;
  margin-bottom: 30px;
}

.brake-circle {
  width: 120px;
  height: 120px;
  border-radius: 50%;
  background: #012856;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 0 0 2px white, 0 0 0 4px #e0e0e0;
  border: 4px solid white;
  outline: 2px dashed #ccc;
  outline-offset: 8px;
}

.brake-number {
  color: white;
  font-size: 48px;
  font-weight: 800;
}

.chameleon-badge {
  display: flex;
  align-items: center;
  background: #E8F5E9;
  border-radius: 50px;
  margin-bottom: 30px;
}

.chameleon-img {
  width: 60px;
  height: auto;
  margin-right: 12px;
  transform: scaleX(-1);
}

.chameleon-text {
  color: #3FA261;
  font-weight: 600;
  font-size: 14px;
  font-family: 'Poppins', sans-serif;
}

.divider {
  height: 1px;
  background: #f0f0f0;
  margin-bottom: 20px;
}

.brake-footer {
  border-left: 3px solid #FFC107;
  padding-left: 16px;
  text-align: left;
}

.brake-footer p {
  font-size: 16px;
  line-height: 1.5;
  color: #444;
  margin: 0;
  font-family: 'Poppins', sans-serif;
}

.highlight-green {
  color: #3FA261;
  font-weight: 700;
  display: block;
  font-size: 18px;
  margin-top: 4px;
  font-family: 'Poppins', sans-serif;
}

.bg-image {
  position: fixed;
  top: 80px;
  left: 30%;
  width: 130%;
  height: 130%;
  transform: translateX(-40%);
  background-image: url('./components/images/NL_map.png');
  background-size: cover;
  background-position: center center;
  opacity: 0.1;
  z-index: 1;
  pointer-events: none; 
}

.focus-card {
  background: white;
  border-radius: 20px;
  padding: 30px 20px;
  margin-bottom: 40px;
  box-shadow: 0 4px 20px rgba(0,0,0,0.1);
}

.focus-status {
  font-size: 18px;
  font-weight: 700;
  color: #012856;
  margin-bottom: 20px;
  margin-bottom: 0;
  font-family: 'Poppins', sans-serif;
}

.gauge-container {
  position: relative;
  width: 220px;
  margin: 0 auto;
  top: 30px;
}

.gauge-svg {
  width: 100%;
  height: auto;
}

.gauge-text {
  position: absolute;
  bottom: 4px;
  left: 50%;
  transform: translateX(-50%);
  text-align: center;
}

.gauge-value {
  display: block;
  font-size: 28px;
  font-weight: 700;
  color: #012856;
  line-height: 1;
  font-size: 22px;
}

.gauge-label {
  font-size: 14px;
  color: #575757;
  font-weight: 500;
}

.sub-title-left {
  text-align: left;
  color: white;
  font-size: 22px;
  font-weight: 700;
  margin-bottom: 8px;
  font-family: 'Poppins', sans-serif;

}

.phone-use-card {
  background: white;
  border-radius: 16px;
  padding: 0px 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  text-align: left;
  margin-bottom: 30px;
  position: relative;
  overflow: visible;
}

.phone-text p {
  margin: 0;
  font-size: 14px;
  color: #575757;
}

.phone-text .time p {
  font-size: 18px;
  color: #012856;
  font-weight: 700;
  font-family: 'Poppins', sans-serif;
}

.chameleon-inline {
  width: 100px;
  height: auto;
  margin-right: -50px;
  margin-top: 0px;
}

.focus-footer {
  color: white;
  font-size: 15px;
  line-height: 1.5;
  text-align: center;
}

.highlight-gold {
  color: #FFC107;
  font-weight: 700;
}

.share-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
}

.no-margin {
  margin-bottom: 10px !important;
}

.dev-card {
  background: white;
  border-radius: 24px;
  padding: 30px 20px 0;
  color: #012856;
  position: relative;
  overflow: hidden;
}

.trend-icon-bg {
  width: 60px;
  height: 60px;
  background: #D87D2B;
  border-radius: 50%;
  margin: 0 auto 20px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.trend-icon { 
  width: auto; 
  height: 16px; 
}

.dev-status { 
  font-size: 24px; 
  font-weight: 700; 
  margin-bottom: 30px;
  font-family: 'Poppins', sans-serif; 
}

.text-underline { 
  text-decoration: underline; 
}

.region-box {
  border: 1px solid #EAF4FF;
  border-radius: 12px;
  padding: 16px;
  margin-bottom: 20px;
  text-align: left;
}

.region-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 12px;
}

.region-label {
  font-size: 12px;
  font-weight: 700;
  color: #012856;
}

.region-badge {
  background: #FFD54F;
  padding: 4px 10px;
  border-radius: 20px;
  font-size: 12px;
  font-weight: 700;
  color: white;
}

.progress-bar-bg {
  height: 12px;
  background: #EAF4FF;
  border-radius: 10px;
  margin-bottom: 6px;
}

.progress-bar-fill {
  height: 100%;
  background: #D87D2B;
  border-radius: 10px;
}

.progress-labels {
  display: flex;
  justify-content: space-between;
  font-size: 12px;
  color: #9aa6b2;
  margin-bottom: 12px;
}

.region-desc {
  font-size: 14px;
  text-align: center;
  margin: 0;
}

.chameleon-orange {
  width: 180px;
  display: block;
  margin: 10px auto 0;
}

.tip-card {
  background: white;
  border-radius: 24px;
  padding: 40px 24px;
  position: relative;
  min-height: 400px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  box-shadow: 0 10px 30px rgba(0,0,0,0.2);
}

.speech-bubble {
  position: relative;
  background: #012856;
  border-radius: 16px;
  padding: 30px 20px;
  color: white;
  text-align: center;
  margin-bottom: 40px;
}

.speech-bubble::after {
  content: '';
  position: absolute;
  bottom: -15px;
  right: 40px;
  width: 0;
  height: 0;
  border-left: 20px solid transparent;
  border-right: 0px solid transparent;
  border-top: 20px solid #012856;
}

.speech-bubble p {
  font-size: 20px;
  line-height: 1.4;
  margin: 0;
  font-weight: 700;
  font-family: 'Poppins', sans-serif;
}

.chameleon-img-tip {
  width: 100px;
  height: auto;
  display: block;
  position: absolute;
  bottom: 0;
  right: 0;
}

.profile-card {
  background: white;
  border-radius: 24px;
  padding: 20px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
}

.image-container-large {
  width: 100%;
  aspect-ratio: 1 / 1;
  border-radius: 16px;
  margin-bottom: 24px;
  position: relative;
  overflow: hidden;
}

.profile-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.top-badge-floating {
  position: absolute;
  top: 12px;
  right: 12px;
  background: white;
  padding: 6px 12px;
  border-radius: 20px;
  font-weight: 700;
  font-size: 12px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  font-family: 'Poppins', sans-serif;
  color: #012856;
}

.profile-name {
  font-size: 28px;
  font-weight: 700;
  color: #012856;
  margin-bottom: 8px;
  font-family: 'Poppins', sans-serif;
}

.profile-desc {
  color: #9aa6b2;
  font-size: 15px;
  margin-bottom: 20px;
  font-family: 'Poppins', sans-serif;
}

.tag-row {
  display: flex;
  justify-content: center;
  gap: 12px;
}

.image-container-small {
  height: 120px;
  border-radius: 12px;
  margin-bottom: 20px;
  overflow: hidden;
  position: relative;
}

.share-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.status-tag {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 8px 16px;
  border: 1px solid #eee;
  border-radius: 20px;
  font-weight: 600;
  color: #012856;
}

.status-tag img {
  width: 16px;
}

.share-card {
  background: white;
  border-radius: 24px;
  text-align: left;
  margin-bottom: 30px;
}

.share-card-header {
  padding: 16px 20px 0;
  text-align: right;
}

.top-badge-inline {
  top: 12px;
  right: 12px;
  background: white;
  padding: 6px 12px;
  border-radius: 20px;
  font-weight: 700;
  font-size: 12px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  font-family: 'Poppins', sans-serif;
  color: #012856;
}

.share-card-body {
  padding: 0 20px 20px;
}

.share-subtitle {
  color: #575757;
  margin: 0;
  font-family: 'Poppins', sans-serif;
}

.share-main-title {
  font-size: 26px;
  font-weight: 700;
  color: #012856;
  margin: 4px 0 20px;
  font-family: 'Poppins', sans-serif;
}

.share-stats {
  display: flex;
  gap: 40px;
  margin-bottom: 20px;
  font-family: 'Poppins', sans-serif;
}

.share-val {
  display: block;
  font-size: 32px;
  font-weight: 700;
  color: #012856;
}

.color-green {
  color: #3FA261;
}

.share-lab {
  font-size: 14px;
  color: #575757;
}

.image-placeholder-small {
  height: 100px;
  background: #f8f8f8;
  border-radius: 12px;
  margin-bottom: 20px;
  background-image: repeating-conic-gradient(
    #f0f0f0 0% 25%,
    white 0% 50%
  );
  background-size: 20px 20px;
}

.share-footer {
  border-top: 1px solid #eee;
  padding-top: 16px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-family: 'Poppins', sans-serif;
}

.footer-brand {
  color: #012856;
}

.yellow-check {
  width: 18px;
  height: 18px;
  background: #FFC107;
  color: white;
  border-radius: 4px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 10px;
}

.social-actions {
  display: flex;
  justify-content: space-around;
}

.social-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  font-size: 14px;
  color: white;
}

.social-circle {
  width: 40px;
  height: 40px;
  background: white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.social-circle img {
  width: 18px;
}

.rel {
  position: relative;
}

.section-label {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 12px;
  color: white;
}

.section-label h3 {
  font-size: 22px;
  font-weight: 700;
  margin: 0;
  font-family: 'Poppins', sans-serif;
}

.pin-icon-yellow {
  color: #FFC107;
  font-size: 20px;
}

.turns-card, .type-card {
  background: white;
  border-radius: 16px;
  padding: 24px 20px;
  color: #012856;
  text-align: left;
  margin-bottom: 30px;
  box-shadow: 0 4px 15px rgba(0,0,0,0.1);
}

.turns-highlight, .type-highlight {
  font-size: 16px;
  line-height: 1.4;
}

p.turns-highlight {
  margin-bottom: 0;
}

.gauge-container-large {
  position: relative;
  width: 100%;
  max-width: 260px;
  margin: 0 auto;
  top: 24px;
}

.gauge-text-center {
  position: absolute;
  bottom: 12px;
  left: 50%;
  transform: translateX(-50%);
  text-align: center;
  width: 100%;
}

.gauge-val-big {
  display: block;
  font-size: 26px;
  font-weight: 700;
  color: #012856;
}

.gauge-lab-small {
  font-size: 13px;
  color: #575757;
}

.split-bar-container {
  margin-top: 10px;
}

.split-bar {
  display: flex;
  height: 36px;
  border-radius: 18px;
  overflow: hidden;
  font-weight: 700;
  font-size: 14px;
  color: white;
}

.bar-left {
  background: #FFC107;
  display: flex;
  align-items: center;
  justify-content: center;
}

.bar-right {
  background: #E2AD1F;
  display: flex;
  align-items: center;
  justify-content: center;
}

.bar-caption {
  text-align: center;
  font-size: 12px;
  color: #012856;
  margin-top: 8px;
  font-weight: 700;
}

.italic {
  font-style: italic;
}

.white-card {
  background: white;
  border-radius: 24px;
  padding: 24px;
  color: #012856;
  text-align: left;
  box-shadow: 0 10px 30px rgba(0,0,0,0.1);
  margin-bottom: 20px;
  position: relative;
  font-family: 'Poppins', sans-serif;
}
.white-card h3 {
  font-weight: 700;
  font-family: 'Poppins', sans-serif;
}

.brake-circle-main {
  width: 130px;
  height: 130px;
  border: 2px dashed #D1D5DB;
  border-radius: 50%;
  margin: 20px auto;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 8px;
}

.inner-circle {
  width: 100%;
  height: 100%;
  background: #012856;
  border-radius: 50%;
  color: white;
  font-size: 40px;
  font-weight: 800;
  display: flex;
  align-items: center;
  justify-content: center;
}

.chameleon-pill {
  display: flex;
  align-items: center;
  background: #E8F5E9;
  border-radius: 50px;
  padding: 4px 16px 4px 8px;
  width: fit-content;
  margin: 0 auto 20px auto;
  gap: 10px;
}

.chameleon-pill img {
  width: 40px;
}

.chameleon-pill span {
  color: #2E7D32;
  font-weight: 600;
  font-size: 14px;
}

.large-chameleon {
  height: 200px;
  width: auto;
  margin-bottom: 24px;
}

@keyframes swipeHint {
  0% {
    transform: translateX(-50%);
    opacity: 0;
  }
  25% {
    transform: translateX(-70%);
    opacity: 1;
  }
  50% {
    transform: translateX(-50%);
    opacity: 0;
  }
  100% {
    transform: translateX(-50%);
    opacity: 0;
  }
}

.fixed-swiper-container {
  position: fixed;
  bottom: 10px;
  left: 50%;
  z-index: 0; 
  pointer-events: none;
}

.swiper-icon {
  width: 100px;
  height: auto;
  animation: swipeHint 4s ease-in-out infinite;
}

.eco-item {
  cursor: pointer;
  transition: background 0.2s;
  padding: 8px;
  border-radius: 12px;
}

.eco-item:active {
  background: rgba(1, 40, 86, 0.05);
}

.info-hint {
  margin-left: auto;
  width: 20px;
  height: 20px;
  border: 1.5px solid #012856;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 11px;
  font-weight: bold;
  opacity: 0.4;
}

.eco-popup-overlay {
  position: fixed;
  inset: 0;
  background: rgba(1, 40, 86, 0.85);
  z-index: 9999;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 24px;
  backdrop-filter: blur(5px);
}

.eco-popup-content {
  background: white;
  border-radius: 24px;
  padding: 32px 24px;
  width: 100%;
  max-width: 340px;
  text-align: center;
  color: #012856;
  box-shadow: 0 10px 30px rgba(0,0,0,0.3);
}

.clickable-points {
  cursor: pointer;
  transition: transform 0.2s ease, background 0.2s ease;
  display: inline-flex;
  align-items: center;
  gap: 8px;
}

.clickable-points:active {
  transform: scale(0.95);
  background: #e2e4e7; /* Iets donkerder grijs bij klik */
}

/* Subtiel i-tje voor de punten badge */
.info-hint-small {
  font-size: 10px;
  background: #012856;
  color: white;
  width: 14px;
  height: 14px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-left: 4px;
  font-weight: bold;
}

.eco-popup-content p {
  white-space: pre-line;
  text-align: left;
}

:deep(.highlight-num) {
  color: #3FA261;
  font-weight: 800;
}

.eco-popup-content p {
  text-align: left;
  line-height: 1.6;
  color: #575757;
}


.clickable-badge:active {
  transform: scale(0.95);
  background-color: #e8f5e9;
}

.info-hint-white {
  width: 14px;
  height: 14px;
  border: 1px solid #3FA261;
  background: white;
  color: #3FA261;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 9px;
  font-weight: bold;
  margin-left: 4px;
}

.eco-popup-content p {
  text-align: left;
}

.clickable-row {
  cursor: pointer;
  transition: background 0.2s ease;
  position: relative;
}

.clickable-row:active {
  background: rgba(1, 40, 86, 0.05);
  border-radius: 12px;
}

.info-hint {
  margin-left: auto;
  width: 18px;
  height: 18px;
  border: 1px solid #9aa6b2;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 10px;
  color: #9aa6b2;
  font-weight: bold;
}

.clickable-region {
  cursor: pointer;
  transition: transform 0.2s ease;
}

.clickable-region:active {
  transform: scale(0.98);
}

.map-container {
  width: 100%;
  height: 220px; /* Iets hoger voor beter overzicht */
  border-radius: 15px;
  overflow: hidden;
  margin: 15px 0;
  position: relative; /* Nodig voor de marker-overlay */
  border: 1px solid #ddd;
}

.marker-pulse-overlay {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  pointer-events: none; /* ESSENTIEEL: laat kliks door naar de kaart */
  width: 16px;
  height: 16px;
  background: #012856;
  border: 3px solid white;
  border-radius: 50%;
  box-shadow: 0 0 10px rgba(0,0,0,0.5);
  z-index: 10; /* Zorg dat hij boven de iframe zweeft */
}

.marker-pulse-overlay::after {
  content: "";
  position: absolute;
  top: -3px; left: -3px;
  width: 16px;
  height: 16px;
  border-radius: 50%;
  border: 2px solid #012856;
  animation: pulse 2s infinite;
}

@keyframes pulse {
  0% { transform: scale(1); opacity: 1; }
  100% { transform: scale(3); opacity: 0; }
}
</style>
