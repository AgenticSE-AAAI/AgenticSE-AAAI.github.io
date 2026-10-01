<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { topics, dates, organizers, programSchedule } from './workshop.js'

const activeSection = ref('home')
const navCollapsed = ref(true)
const speakers = []
const pc = []

const sections = [
  { id: 'home', label: 'Home' },
  // { id: 'program', label: 'Schedule' },
  // { id: 'speakers', label: 'Speakers' },
  // { id: 'accepted-papers', label: 'Accepted Papers' },
  { id: 'submission', label: 'Call for Papers' },
  { id: 'dates', label: 'Dates' },
  { id: 'organizers', label: 'Committee' },
]

function scrollTo(id) {
  navCollapsed.value = true
  const el = document.getElementById(id)
  if (el) el.scrollIntoView({ behavior: 'smooth' })
}

function handleScroll() {
  const scrollY = window.scrollY + 100
  for (let i = sections.length - 1; i >= 0; i--) {
    const el = document.getElementById(sections[i].id)
    if (el && el.offsetTop <= scrollY) {
      activeSection.value = sections[i].id
      break
    }
  }
}

onMounted(() => window.addEventListener('scroll', handleScroll))
onUnmounted(() => window.removeEventListener('scroll', handleScroll))
</script>

<template>
  <!-- Navbar -->
  <nav class="navbar navbar-expand-lg fixed-top">
    <div class="container-fluid px-4">
      <a class="navbar-brand" href="#" @click.prevent="scrollTo('home')">
        Agentic SE @ AAAI-27
      </a>
      <button
        class="navbar-toggler border-0"
        type="button"
        @click="navCollapsed = !navCollapsed"
        aria-label="Toggle navigation"
        :aria-expanded="!navCollapsed"
        aria-controls="navigation-links"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div id="navigation-links" class="collapse navbar-collapse" :class="{ show: !navCollapsed }">
        <ul class="navbar-nav ms-auto">
          <li class="nav-item" v-for="s in sections" :key="s.id">
            <a
              class="nav-link"
              :class="{ active: activeSection === s.id }"
              href="#"
              @click.prevent="scrollTo(s.id)"
            >{{ s.label }}</a>
          </li>
        </ul>
      </div>
    </div>
  </nav>

  <!-- Hero Text -->
  <section id="home" class="hero-text-section">
    <div class="container">
      <div class="row">
        <div class="col-lg-11">
          <span class="hero-badge">AAAI-27 Workshop</span>
          <h1 class="hero-title">
            Agentic Software Engineering
          </h1>
          <div class="hero-meta d-flex flex-wrap gap-4 mb-4">
            <div><i class="bi bi-calendar-event"></i> February 22 or 23, 2027</div>
            <div><i class="bi bi-geo-alt"></i> Montréal, Canada</div>
            <div><i class="bi bi-clock"></i> Full-Day</div>
            <div><i class="bi bi-building"></i> Exact workshop day and room to be announced</div>
          </div>
          <p class="mt-4" style="max-width: 800px; font-size: 1.15rem; line-height: 1.7;">
            Co-located with <a href="https://aaai.org/conference/aaai/aaai-27/" target="_blank" rel="noopener noreferrer"><strong>AAAI-27</strong></a>.
            Bridging artificial intelligence (AI), software engineering (SE), programming languages and formal methods (PL/FM), and research software engineering.
          </p>
          <a href="#submission" @click.prevent="scrollTo('submission')" class="btn-register">
            Call for Papers <i class="bi bi-arrow-right ms-2"></i>
          </a>
        </div>
      </div>
    </div>
  </section>


  <!-- Program Schedule (hidden until ready to announce)
  <section id="program" class="section">
    <div class="container">
      <div class="row mb-5">
        <div class="col-12">
          <h2 class="section-title text-center">Program Schedule</h2>
        </div>
      </div>
      <p class="text-center text-muted mb-4">Tentative schedule. The exact workshop day and confirmed speakers will be announced.</p>
      <div class="program-list">
        <div class="program-item" v-for="item in programSchedule" :key="`${item.time}-${item.title}`">
          <div class="program-time">{{ item.time }}</div>
          <div class="program-details">
            <h3 class="program-title">
              <a v-if="item.speakerId" :href="'#' + item.speakerId" @click.prevent="scrollTo(item.speakerId)" class="program-title-link">{{ item.title }}</a>
              <template v-else-if="item.sectionId && item.linkLabel">
                <span>{{ item.title }} (</span><a :href="'#' + item.sectionId" @click.prevent="scrollTo(item.sectionId)" class="program-title-link">{{ item.linkLabel }}</a><span>)</span>
              </template>
              <a v-else-if="item.sectionId" :href="'#' + item.sectionId" @click.prevent="scrollTo(item.sectionId)" class="program-title-link">{{ item.title }}</a>
              <span v-else>{{ item.title }}</span>
              <span v-if="item.room" class="program-room"><i class="bi bi-geo-alt"></i> {{ item.room }}</span>
            </h3>
            <p v-if="item.speaker" class="program-speaker">
              <a v-if="item.speakerId" :href="'#' + item.speakerId" @click.prevent="scrollTo(item.speakerId)" class="program-speaker-link">{{ item.speaker }}</a>
              <span v-else>{{ item.speaker }}</span>
              <span v-if="item.affiliation"> ({{ item.affiliation }})</span>
            </p>
            <p v-else-if="item.panelSpeakers" class="program-speaker">
              <template v-for="(ps, idx) in item.panelSpeakers" :key="ps.id">
                <a :href="'#' + ps.id" @click.prevent="scrollTo(ps.id)" class="program-speaker-link">{{ ps.name }}</a><span v-if="idx < item.panelSpeakers.length - 1">, </span>
              </template>
            </p>
            <p v-if="item.note" class="program-note">{{ item.note }}</p>
          </div>
        </div>
      </div>
    </div>
  </section>
  -->

  <!-- Keynote Speakers (hidden until ready to announce)
  <section id="speakers" class="section">
    <div class="container">
      <div class="row mb-5">
        <div class="col-12">
          <h2 class="section-title text-center">Keynote Speakers</h2>
        </div>
      </div>
      <p v-if="!speakers.length" class="text-center text-muted">Confirmed speakers and talk details will be announced.</p>
      <div class="speakers-list">
        <div class="speaker-card" v-for="s in speakers" :key="s.name" :id="s.id">
          <div class="speaker-photo-col">
            <a v-if="s.photo && s.website" :href="s.website" target="_blank" rel="noopener noreferrer">
              <img class="speaker-photo" :src="s.photo" :alt="s.name">
            </a>
            <img v-else-if="s.photo" class="speaker-photo" :src="s.photo" :alt="s.name">
            <div v-else class="speaker-photo d-flex align-items-center justify-content-center bg-light text-muted" style="font-size: 3rem;">
              <i class="bi bi-person-fill"></i>
            </div>
            <div class="speaker-name-block">
              <h4 class="speaker-name">
                <a v-if="s.website" :href="s.website" target="_blank" rel="noopener noreferrer">{{ s.name }}</a>
                <span v-else>{{ s.name }}</span>
              </h4>
              <p class="speaker-affiliation">{{ s.affiliation }}</p>
            </div>
          </div>
          <div class="speaker-info-col">
            <p v-if="s.time" class="speaker-time"><i class="bi bi-clock"></i> {{ s.time }}</p>
            <template v-if="s.title">
              <h5 class="speaker-talk-title">{{ s.title }}</h5>
              <div class="speaker-abstract" v-html="s.abstract"></div>
              <p class="speaker-bio"><strong>Bio:</strong> {{ s.bio }}</p>
            </template>
            <p v-else class="speaker-tba">Talk details coming soon.</p>
          </div>
        </div>
      </div>
    </div>
  </section>
  -->

  <!-- Accepted Papers (hidden until ready to announce)
  <section id="accepted-papers" class="section section-alt">
    <div class="container">
      <div class="row mb-5"><div class="col-12">
        <h2 class="section-title text-center">Accepted Papers</h2>
      </div></div>
      <p class="text-center text-muted">Accepted papers will be announced after author notification on December 2, 2026.</p>
    </div>
  </section>
  -->

  <!-- Submission / CTA -->
  <section id="submission" class="section section-alt">
    <div class="container">
      <div class="row">
        <div class="col-lg-4">
          <h2 class="section-title">Call for<br>Papers</h2>
        </div>
        <div class="col-lg-8">
          <hr class="section-divider d-lg-none">
          <p class="mb-5" style="font-size: 1.1rem; line-height: 1.8;">
            We welcome original research, position and vision papers, reports, and preliminary results that spark discussion across artificial intelligence (AI), software engineering (SE), programming languages and formal methods (PL/FM), and research software engineering. The workshop covers the full agentic software lifecycle, from capturing requirements and intent to verification, testing, and maintaining production and scientific software.
          </p>

          <h4 class="fw-bold mb-4 text-uppercase" style="letter-spacing: 0.05em;">Topics of Interest</h4>
          <ul class="mb-5" style="font-size: 1.1rem; line-height: 1.8; color: var(--text-muted); list-style-type: disc; padding-left: 1.5rem;">
            <li v-for="t in topics" :key="t.title" class="mb-2">
              <strong style="color: var(--primary);">{{ t.title }}:</strong> <span>{{ t.description }}</span>
            </li>
          </ul>

          <h4 class="fw-bold mb-4 text-uppercase" style="letter-spacing: 0.05em;">Paper Submission</h4>
          <ul class="mb-4" style="font-size: 1.1rem; line-height: 1.8; color: var(--text-muted);">
            <li><strong>Full papers:</strong> Up to 8 pages (excluding references) for original research contributions.</li>
            <li><strong>Short papers:</strong> Up to 4 pages (excluding references) for position and vision papers, reports, and preliminary results.</li>
            <li><strong>Format:</strong> Please use the template in the <a href="https://aaai.org/authorkit27/">AAAI-2027 Author Kit</a>.</li>
            <li><strong>Review:</strong> All submissions undergo double-blind peer review. Please anonymize your submission.</li>
            <li><strong>Proceedings:</strong> The workshop is non-archival. Accepted papers will be listed on this website.</li>
            <!-- <li><strong>Presentation:</strong> Each accepted paper receives a lightning talk and a poster. At least one author must register and present in person.</li> -->
          </ul>
          <p class="text-muted mt-4" style="font-size: 1.1rem; line-height: 1.8;">
            <strong>Submission portal:</strong> The OpenReview link will be announced.
          </p>
        </div>
      </div>
    </div>
  </section>

  <!-- Important Dates -->
  <section id="dates" class="section section-alt">
    <div class="container">
      <div class="row">
        <div class="col-lg-4">
          <h2 class="section-title">Important<br>Dates</h2>
        </div>
        <div class="col-lg-8">
          <hr class="section-divider d-lg-none">
          <div class="date-list">
            <div class="date-item" v-for="d in dates" :key="d.event">
              <span class="date-badge" v-html="d.date"></span>
              <span class="date-event" v-html="d.event"></span>
            </div>
          </div>
          <p class="text-muted mt-4" style="font-size: 0.95rem;">
            <i class="bi bi-info-circle me-2"></i>
            The submission deadline is 11:59 PM Anywhere on Earth (AoE).
          </p>
        </div>
      </div>
    </div>
  </section>

  <!-- Organizers -->
  <section id="organizers" class="section section-alt">
    <div class="container">
      <div class="row mb-5">
        <div class="col-12">
          <h2 class="section-title text-center">Organizing Committee</h2>
        </div>
      </div>
      <div class="row g-4 justify-content-center row-cols-1 row-cols-sm-2 row-cols-lg-5">
        <div class="col" v-for="o in organizers" :key="o.name">
          <component 
            :is="o.website ? 'a' : 'div'"
            v-bind="o.website ? { href: o.website, target: '_blank', rel: 'noopener noreferrer' } : {}"
            :class="o.website ? 'text-decoration-none' : ''"
            :style="[o.website ? { display: 'block', color: 'inherit' } : {}, { height: '100%' }]"
          >
            <div class="organizer-card h-100">
              <img v-if="o.photo" class="avatar" :src="o.photo" :alt="o.name" :style="o.objectPosition ? { objectPosition: o.objectPosition } : {}">
              <div v-else class="avatar d-flex align-items-center justify-content-center bg-light text-muted" style="font-size: 3rem;">
                <i class="bi bi-person-fill"></i>
              </div>
              <h5 style="font-size: 1.05rem" class="mb-2">{{ o.name }}</h5>
              <p class="affiliation">{{ o.affiliation }}</p>
            </div>
          </component>
        </div>
      </div>
    </div>
  </section>

  <!-- Program Committee -->
  <section id="pc" class="section section-alt">
    <div class="container">
      <div class="row mb-5">
        <div class="col-12">
          <h2 class="section-title text-center">Program Committee</h2>
        </div>
      </div>
      <div class="row justify-content-center">
        <div class="col-lg-8">
          <p v-if="!pc.length" class="text-center text-muted">The program committee will be announced.</p>
          <ul class="pc-list">
            <li v-for="p in pc" :key="p.name" class="pc-item">
              <strong>{{ p.name }}</strong><span v-if="p.role" class="text-muted"> ({{ p.role }})</span>
              <span class="text-muted"> — {{ p.affiliation }}</span>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </section>

  <!-- Footer -->
  <footer class="site-footer">
    <div class="container text-center">
      <h3 class="mb-4 fw-bold text-uppercase" style="letter-spacing: 0.05em;">Agentic Software Engineering</h3>
      <p class="mb-2" style="font-size: 1.1rem;">
        AAAI-27 Workshop &middot; Montréal, Canada &middot; February 22 or 23, 2027
      </p>
      <p class="mb-5 mt-4" style="font-size: 1rem;">
        <strong>Contact us:</strong> <a href="mailto:hao.li@queensu.ca">hao.li@queensu.ca</a>
      </p>
      <p class="mb-0" style="font-size: 0.9rem; opacity: 0.7;">
        &copy; 2026 Agentic SE Workshop. Co-located with <a href="https://aaai.org/conference/aaai/aaai-27/" target="_blank" rel="noopener noreferrer">AAAI-27</a>.
      </p>
    </div>
  </footer>
</template>
