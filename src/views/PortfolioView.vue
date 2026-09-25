<template>
  <main class="work-page">
    <section class="section-heading"><h1 class="section-title">Past project <span>experience</span></h1></section>

    <section class="portfolio-carousel" tabindex="0" aria-label="Portfolio projects carousel" @keydown.left.prevent="previousProject" @keydown.right.prevent="nextProject">
      <div class="carousel-stage" @pointerdown="startDrag" @pointerup="finishDrag" @pointercancel="cancelDrag" @pointerleave="finishDrag">
        <article v-for="(item, index) in items" :key="item.id" class="project-slide glass-panel" :class="{ active: index === activeIndex }" :style="cardStyle(index)" @click="selectProject(index)">
          <div class="project-image"><img :src="item.image" :alt="item.name" loading="lazy" /></div>
        </article>
      </div>

      <div class="carousel-controls"><button class="carousel-arrow" type="button" aria-label="Previous project" @click="previousProject">←</button><span>{{ String(activeIndex + 1).padStart(2, '0') }} / {{ String(items.length).padStart(2, '0') }}</span><button class="carousel-arrow" type="button" aria-label="Next project" @click="nextProject">→</button></div>

      <article class="project-info glass-panel"><div class="project-info-top"><p class="project-type">{{ activeProject.type }}</p><span class="project-year">{{ activeProject.year }}</span></div><div class="project-info-title"><h2>{{ activeProject.name }}</h2><a v-if="activeProject.demo !== 'null'" :href="activeProject.demo" target="_blank" rel="noreferrer">View project ↗</a></div><p>{{ activeProject.status }}</p><div class="project-dots" aria-label="Choose a project"><button v-for="(item, index) in items" :key="item.id" type="button" :class="{ active: index === activeIndex }" :aria-label="'Show ' + item.name" :aria-current="index === activeIndex ? 'true' : undefined" @click="selectProject(index)"></button></div></article>
    </section>
  </main>
</template>

<script>
export default {
  name: 'PortfolioView',
  data() {
    return {
      activeIndex: 0,
      dragStartX: null,
      isDragging: false,
      items: [
        { id: 1, name: 'Milia', image: '/img/portfolio-milia.png', type: 'HR management system', year: '2024', status: 'A system for managing employee data, tasks, performance, and human resource administration.', demo: 'https://milia.redguardsecurity.com/' },
        { id: 2, name: 'Araveal', image: '/img/portfolio-game.png', type: '2D platformer game', year: '2023', status: 'A 2D platformer with a character, obstacles, and levels designed to feel fun to play.', demo: 'null' },
        { id: 3, name: 'WeSaF Al-falah', image: '/img/portfolio-spp.png', type: 'Payment platform', year: '2023', status: 'A simple and structured platform for managing school tuition payments.', demo: 'null' },
        { id: 4, name: 'Lemas Al-falah', image: '/img/portfolio-lms.png', type: 'Learning management', year: '2023', status: 'A learning management system that helps students access assignments and study materials.', demo: 'null' },
        { id: 5, name: 'PSAB', image: '/img/portfolio-psab.png', type: 'Environmental monitoring', year: '2022', status: 'A real-time monitoring system for river cleanliness and clean water conditions.', demo: 'null' },
        { id: 6, name: 'LingkunganKu', image: '/img/portfolio-lingkunganku.png', type: 'Public service', year: '2022', status: 'A public complaint platform for reporting environmental issues nearby.', demo: 'null' },
        { id: 7, name: 'StuntCheck', image: '/img/portfolio-stuntcheck.png', type: 'AI-powered health system', year: '2024', status: 'Early stunting risk detection through digital anthropometric analysis.', demo: 'null' },
        { id: 8, name: 'SmartFinance', image: '/img/portfolio-smartfinance.png', type: 'Personal finance platform', year: '2026', status: 'A finance management platform for tracking income, expenses, and everyday financial goals.', demo: 'null' },
        { id: 9, name: 'Tani Bijak', image: '/img/portfolio-tanibijak.jpeg', type: 'Agriculture platform', year: '2026', status: 'A digital platform that supports smarter farming decisions and agricultural information.', demo: 'null' },
        { id: 10, name: 'PresenPro', image: '/img/portfolio-presenpro.png', type: 'Dynamic QR attendance system', year: '2026', status: 'An attendance system that uses dynamic QR codes to make check-ins more secure, practical, and easy to monitor.', demo: 'null' },
      ],
    };
  },
  computed: {
    activeProject() { return this.items[this.activeIndex]; },
  },
  methods: {
    cardStyle(index) {
      let offset = index - this.activeIndex;
      const total = this.items.length;
      if (offset > total / 2) offset -= total;
      if (offset < -total / 2) offset += total;
      const visible = Math.abs(offset) <= 2;
      return {
        '--card-offset': offset,
        '--card-scale': 1 - (Math.min(Math.abs(offset), 2) * 0.075),
        zIndex: visible ? 10 - Math.abs(offset) : 0,
        opacity: visible ? 1 : 0,
        pointerEvents: visible ? 'auto' : 'none',
      };
    },
    selectProject(index) { if (index !== this.activeIndex) this.activeIndex = index; },
    nextProject() { this.activeIndex = (this.activeIndex + 1) % this.items.length; },
    previousProject() { this.activeIndex = (this.activeIndex - 1 + this.items.length) % this.items.length; },
    startDrag(event) { this.dragStartX = event.clientX; this.isDragging = true; event.currentTarget.setPointerCapture?.(event.pointerId); },
    finishDrag(event) { if (!this.isDragging || this.dragStartX === null) return; const distance = event.clientX - this.dragStartX; if (Math.abs(distance) > 45) distance < 0 ? this.nextProject() : this.previousProject(); this.cancelDrag(); },
    cancelDrag() { this.dragStartX = null; this.isDragging = false; },
  },
};
</script>

<style scoped>
.work-page { padding: 1rem 0 3rem; text-align: left; }.section-heading { margin-bottom: 1.2rem; }.section-title { margin: 0; color: var(--text); font-family: 'Poppins'; font-size: clamp(2.2rem, 5vw, 4.4rem); font-weight: 600; letter-spacing: -.07em; line-height: 1; }.section-title span { color: var(--accent); }
.portfolio-carousel { outline: 0; }.carousel-stage { position: relative; height: min(54vw, 480px); min-height: 300px; margin: 0 auto; touch-action: pan-y; user-select: none; }.project-slide { position: absolute; top: 0; left: 50%; width: min(72%, 650px); height: 100%; padding: .55rem; border-radius: 24px; transform: translateX(calc(-50% + (var(--card-offset) * 58px))) scale(var(--card-scale)) rotate(calc(var(--card-offset) * -2deg)); transform-origin: center bottom; transition: transform .5s cubic-bezier(.2,.8,.2,1), opacity .35s ease, filter .5s ease; cursor: pointer; }.project-slide:not(.active) { filter: saturate(.65) brightness(.7); }.project-slide.active { cursor: grab; }.project-slide.active:active { cursor: grabbing; }.project-image { width: 100%; height: 100%; overflow: hidden; border-radius: 18px; background: var(--surface-strong); }.project-image img { display: block; width: 100%; height: 100%; object-fit: cover; }.carousel-controls { display: flex; align-items: center; justify-content: center; gap: 1.1rem; margin: .8rem 0 1rem; color: var(--muted); font-size: .72rem; }.carousel-arrow { display: grid; place-items: center; width: 38px; height: 38px; border: 1px solid var(--line); border-radius: 50%; color: var(--text); background: var(--surface); cursor: pointer; font-size: 1.1rem; transition: background .3s ease, color .3s ease, transform .3s ease; }.carousel-arrow:hover { color: var(--accent-ink); background: var(--accent); transform: translateY(-2px); }.project-info { max-width: 760px; margin: 0 auto; padding: 1.3rem 1.5rem; border-radius: 20px; }.project-info-top, .project-info-title { display: flex; align-items: center; justify-content: space-between; gap: 1rem; }.project-type { margin: 0 0 .45rem; color: var(--accent); font-size: .65rem; font-weight: 600; letter-spacing: .08em; text-transform: uppercase; }.project-year { color: var(--muted); font-size: .7rem; }.project-info h2 { margin: 0; color: var(--text); font-size: clamp(1.35rem, 3vw, 2rem); font-weight: 600; letter-spacing: -.04em; }.project-info-title a { color: var(--text); font-size: .72rem; font-weight: 600; }.project-info-title a:hover { color: var(--accent); }.project-info > p { max-width: 650px; margin: .8rem 0 1.1rem; color: var(--muted); font-size: .8rem; line-height: 1.7; }.project-dots { display: flex; flex-wrap: wrap; gap: .35rem; }.project-dots button { width: 7px; height: 7px; padding: 0; border: 0; border-radius: 50%; background: var(--line-strong); cursor: pointer; transition: transform .3s ease, background .3s ease; }.project-dots button.active { width: 20px; border-radius: 99px; background: var(--accent); }.project-dots button:hover { transform: scale(1.3); }
@media (max-width: 760px) { .work-page { padding-top: .5rem; }.section-heading { margin-bottom: 1rem; }.carousel-stage { height: min(68vw, 340px); min-height: 235px; }.project-slide { width: 88%; padding: .4rem; border-radius: 20px; transform: translateX(calc(-50% + (var(--card-offset) * 34px))) scale(var(--card-scale)) rotate(calc(var(--card-offset) * -2.5deg)); }.project-image { border-radius: 15px; }.project-info { padding: 1rem; border-radius: 18px; }.project-info h2 { font-size: 1.25rem; }.project-info > p { font-size: .75rem; } }
.project-image img { object-fit: contain; padding: .5rem; }
</style>
