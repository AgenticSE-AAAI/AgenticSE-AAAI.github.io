<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { topics, dates, organizers, programCommittee, programSchedule } from './workshop.js'

const activeSection = ref('home')
const navCollapsed = ref(true)
const isScrolled = ref(false)
const speakers = []

const sections = [
  { id: 'home', label: 'Home' },
  // { id: 'program', label: 'Schedule' },
  // { id: 'speakers', label: 'Speakers' },
  // { id: 'accepted-papers', label: 'Accepted Papers' },
  { id: 'submission', label: 'Call for Papers' },
  { id: 'dates', label: 'Dates' },
  { id: 'reviewer', label: 'Reviewers' },
  { id: 'organizers', label: 'Committee' },
]

function scrollTo(id) {
  navCollapsed.value = true
  const el = document.getElementById(id)
  if (el) el.scrollIntoView({ behavior: window.matchMedia('(prefers-reduced-motion: reduce)').matches ? 'auto' : 'smooth' })
}

function handleScroll() {
  isScrolled.value = window.scrollY > 32
  const scrollY = window.scrollY + 100
  for (let i = sections.length - 1; i >= 0; i--) {
    const el = document.getElementById(sections[i].id)
    if (el && el.offsetTop <= scrollY) {
      activeSection.value = sections[i].id
      break
    }
  }
}

onMounted(() => {
  handleScroll()
  window.addEventListener('scroll', handleScroll, { passive: true })
})
onUnmounted(() => window.removeEventListener('scroll', handleScroll))
</script>

<template>
  <a href="#main-content" class="skip-link">Skip to content</a>
  <!-- Navbar -->
  <nav class="navbar navbar-expand-lg fixed-top" :class="{ 'navbar-solid': isScrolled || !navCollapsed }" aria-label="Main navigation">
    <div class="container">
      <a class="navbar-brand" href="#home" @click.prevent="scrollTo('home')">
        Agentic SE
        <span class="brand-edition">
          <img class="brand-logo" src="/images/aaai-logo-white.png" alt="" width="700" height="640">
          AAAI-27
        </span>
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
              :href="'#' + s.id"
              :aria-current="activeSection === s.id ? 'location' : undefined"
              @click.prevent="scrollTo(s.id)"
            >{{ s.label }}</a>
          </li>
        </ul>
      </div>
    </div>
  </nav>

  <main id="main-content">
  <!-- Workshop introduction -->
  <section id="home" class="hero-section" aria-labelledby="workshop-title">
    <img
      class="hero-background"
      src="/images/montreal-old-port-dusk.jpg"
      alt=""
      width="2400"
      height="1600"
      fetchpriority="high"
      decoding="async"
    >
    <div class="container hero-content">
        <div class="hero-copy">
          <p class="hero-eyebrow">AAAI-27 Workshop <span aria-hidden="true">/</span> Montréal, Canada</p>
          <h1 id="workshop-title" class="hero-title">
            Agentic Software Engineering
          </h1>
          <p class="hero-description">
            Co-located with <a href="https://aaai.org/conference/aaai/aaai-27/" target="_blank" rel="noopener noreferrer"><strong>AAAI-27</strong></a>.
            Bridging artificial intelligence, software engineering, programming languages and formal methods, and research software engineering.
          </p>
          <div class="hero-actions">
            <a href="https://openreview.net/group?id=AAAI.org/2027/Workshop/AgenticSE" target="_blank" rel="noopener noreferrer" class="btn-register hero-primary">
              Submit Paper (OpenReview)
            </a>
            <a href="#submission" class="hero-secondary" @click.prevent="scrollTo('submission')">
              Call for Papers
            </a>
          </div>
        </div>
    </div>
    <div class="container hero-bottom">
      <span><i class="bi bi-geo-alt" aria-hidden="true"></i> Old Port, Montréal</span>
    </div>
  </section>

  <div class="workshop-facts" aria-label="Workshop at a glance">
    <dl class="container facts-grid">
      <div>
        <dt>When</dt>
        <dd>February 22 or 23, 2027 <small>Exact workshop day and room to be announced</small></dd>
      </div>
      <div>
        <dt>Where</dt>
        <dd>Montréal, Canada <small>Full-Day</small></dd>
      </div>
      <div>
        <dt>Co-located with</dt>
        <dd><a class="conference-brand" href="https://aaai.org/conference/aaai/aaai-27/" target="_blank" rel="noopener noreferrer"><img src="/images/aaai-logo-white.png" alt="AAAI" width="700" height="640"><span>AAAI-27</span></a></dd>
      </div>
    </dl>
  </div>

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
  <section id="submission" class="section cfp-section" aria-labelledby="cfp-title">
    <div class="container">
      <h2 id="cfp-title" class="section-title section-title-band">Call for Papers</h2>
      <div class="cfp-intro">
        <p>
          Software has emerged as the primary proving ground for agentic AI. Agentic coding systems such as Claude Code and OpenAI Codex are among the first truly autonomous agents deployed in complex, real-world environments. However, writing code is merely the entry point. Agents must also capture requirements and intent, reason over specifications and formal artifacts, validate and verify their outputs, and build, test, and maintain production and scientific software.
        </p>
        <p>
          The <strong>AAAI-27 Workshop on Agentic Software Engineering</strong> bridges the AI community with the software engineering, programming languages and formal methods, and research software engineering communities. We welcome original research, position and vision papers, reports, and preliminary results on the topics below.
        </p>
      </div>

      <h3 class="content-heading">Topics of Interest</h3>
      <ul class="topics-grid">
        <li v-for="t in topics" :key="t.title">
          <h4>{{ t.title }}</h4>
          <p v-html="t.description"></p>
        </li>
      </ul>

      <div class="submission-details">
        <h3 class="content-heading">Paper Submission</h3>
        <ul class="submission-guidelines">
          <li><strong>Long papers:</strong> Up to 8 pages (excluding references).</li>
          <li><strong>Short papers:</strong> Up to 4 pages (excluding references).</li>
          <li><strong>Position papers:</strong> Up to 4 pages (excluding references).</li>
          <li><strong>Format:</strong> Please use the template in the <a href="https://aaai.org/authorkit27/">AAAI-2027 Author Kit</a>.</li>
          <li><strong>Review:</strong> All submissions are double-blind. Please anonymize your submission.</li>
          <li><strong>Proceedings:</strong> The workshop is non-archival, with no formal proceedings. Accepted papers will be listed on this website.</li>
          <li><strong>Dual submission:</strong> Work under review or destined for other venues is welcome.</li>
          <li><strong>Presentation:</strong> At least one author must <a href="https://aaai.getregistered.net/aaai-conference-2027" target="_blank" rel="noopener noreferrer">register</a> and present in person (poster or oral).</li>
        </ul>
        <div class="submission-action">
          <a href="https://openreview.net/group?id=AAAI.org/2027/Workshop/AgenticSE" target="_blank" rel="noopener noreferrer" class="btn-register">
            Submit Paper (OpenReview)
          </a>
        </div>
      </div>
    </div>
  </section>

  <!-- Important Dates -->
  <section id="dates" class="section section-alt dates-section" aria-labelledby="dates-title">
    <div class="container">
      <div class="dates-heading">
        <h2 id="dates-title" class="section-title">Important Dates</h2>
        <p class="dates-timezone">
          <i class="bi bi-globe2" aria-hidden="true"></i>
          The submission deadline is 11:59 PM Anywhere on Earth (AoE).
        </p>
      </div>
      <div class="date-cards">
        <article v-for="d in dates" :key="d.event" class="date-card" :class="{ 'date-card-featured': d.featured }">
          <h3 class="date-card-label">{{ d.event }}</h3>
          <p class="date-card-day">{{ d.day }}</p>
          <p class="date-card-month">{{ d.monthYear }}</p>
          <p class="date-card-note">{{ d.note }}</p>
        </article>
      </div>
    </div>
  </section>

  <!-- Reviewer Self-Nomination -->
  <section id="reviewer" class="section reviewer-section" aria-labelledby="reviewer-title">
    <div class="container">
      <div class="row">
        <div class="col-lg-4">
          <h2 id="reviewer-title" class="section-title">Call for Reviewers</h2>
        </div>
        <div class="col-lg-8">
          <hr class="section-divider d-lg-none">
          <p class="reviewer-intro mb-0">
            We are looking for qualified reviewers with expertise in AI agents, software engineering, and related areas. If you are interested in serving as a reviewer for the <strong>AgenticSE @ AAAI-27</strong> workshop, please fill out the self-nomination form below.
          </p>
          <a href="https://docs.google.com/forms/d/e/1FAIpQLScoT6DMc6PO_T8BAdAx5aidZU4u7WcUF56XmCY2I4zYLLZdSw/viewform" target="_blank" rel="noopener noreferrer" class="btn-register reviewer-cta">
            <span>Self-Nominate as Reviewer</span>
          </a>
        </div>
      </div>
    </div>
  </section>

  <!-- Organizers -->
  <section id="organizers" class="section">
    <div class="container">
      <div class="row mb-5">
        <div class="col-12">
          <h2 class="section-title section-title-band">Organizing Committee</h2>
        </div>
      </div>
      <div class="row g-4 justify-content-center row-cols-1 row-cols-sm-2 row-cols-lg-3 row-cols-xxl-5">
        <div class="col" v-for="o in organizers" :key="o.name">
          <component 
            :is="o.website ? 'a' : 'div'"
            v-bind="o.website ? { href: o.website, target: '_blank', rel: 'noopener noreferrer' } : {}"
            :class="o.website ? 'text-decoration-none' : ''"
            :style="[o.website ? { display: 'block', color: 'inherit' } : {}, { height: '100%' }]"
          >
            <div class="organizer-card h-100">
              <img v-if="o.photo" class="avatar" :src="o.photo" :alt="o.name" loading="lazy" :style="o.objectPosition ? { objectPosition: o.objectPosition } : {}">
              <div v-else class="avatar d-flex align-items-center justify-content-center bg-light text-muted" style="font-size: 3rem;">
                <i class="bi bi-person-fill"></i>
              </div>
              <h5 class="mb-2">{{ o.name }}</h5>
              <ul class="committee-affiliations">
                <li v-for="affiliation in o.affiliations" :key="affiliation">{{ affiliation }}</li>
              </ul>
            </div>
          </component>
        </div>
      </div>
    </div>
  </section>

  <!-- Program Committee -->
  <section id="pc" class="section">
    <div class="container">
      <div class="row mb-5">
        <div class="col-12">
          <h2 class="section-title section-title-band">Program Committee</h2>
        </div>
      </div>
      <div class="row justify-content-center">
        <div class="col-12">
          <p v-if="!programCommittee.length" class="text-center text-muted">The program committee will be announced.</p>
          <ul v-else class="pc-list">
            <li v-for="p in programCommittee" :key="p.name" class="pc-item">
              <div class="pc-person">
                <a v-if="p.website" :href="p.website" target="_blank" rel="noopener noreferrer" class="pc-name">{{ p.name }}</a>
                <strong v-else class="pc-name">{{ p.name }}</strong>
                <span v-if="p.role" class="pc-role">{{ p.role }}</span>
              </div>
              <ul class="committee-affiliations">
                <li v-for="affiliation in p.affiliations" :key="affiliation">{{ affiliation }}</li>
              </ul>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </section>
  </main>

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
