<template>
  <main class="portfolio-shell">
    <nav class="nav-bar">
      <a href="#home" class="brand" aria-label="Kevin Octavius portfolio">
        <span class="brand-mark">KO</span>
        <span>Kevin<span class="brand-dot">.</span></span>
      </a>
      <div class="nav-links">
        <a href="#about">About</a>
        <a href="#projects">Projects</a>
        <a href="#skills">Skills</a>
        <a href="#contact">Contact</a>
      </div>
    </nav>

    <section id="home" class="hero section-reveal">
      <div class="hero-copy">
        <div class="status-pill"><span></span>Available for Internship & Freelance Work</div>
        <p class="eyebrow">Full Stack Developer • IoT Enthusiast</p>
        <h1>Membangun produk digital yang rapi, cepat, dan siap dipakai.</h1>
        <p class="hero-description">
          Saya Kevin Octavius, developer yang fokus merancang dashboard monitoring,
          integrasi IoT, automation bot, dan aplikasi mobile dengan pengalaman end-to-end
          dari backend, database, UI, hingga deployment.
        </p>
        <div class="hero-actions">
          <a href="#projects" class="primary-button">Lihat Project Saya</a>
          <a href="/cv-kevin-octavius.pdf" class="secondary-button" download>Download CV</a>
        </div>
        <div class="social-strip" aria-label="Social media links">
          <a v-for="social in socials" :key="social.label" :href="social.href" target="_blank" rel="noreferrer">
            <span class="social-icon" v-html="social.icon"></span>
            {{ social.label }}
          </a>
        </div>
      </div>

      <aside class="profile-card clickable-profile" @click="openProfile" title="Klik untuk melihat skill dan experience">
        <div class="card-shine"></div>
        <div class="profile-topline">
          <span>Portfolio Card</span>
          <strong>2026</strong>
        </div>
        <div class="avatar-wrap">
          <span class="orbital-badge badge-js">JS</span>
          <span class="orbital-badge badge-vue">Vue</span>
          <span class="orbital-badge badge-node">Node</span>
          <div class="avatar">KO</div>
        </div>
        <h2>Kevin Octavius</h2>
        <p>Full Stack Developer yang senang membuat sistem monitoring dan automation yang reliable.</p>
        <div class="mini-stack">
          <span v-for="stack in featuredStack" :key="stack">{{ stack }}</span>
        </div>
        <div class="profile-footer">
          <span>Click for details</span>
          <strong>↗</strong>
        </div>
      </aside>
    </section>

    <section class="metrics-grid section-reveal" aria-label="Portfolio highlights">
      <div v-for="metric in metrics" :key="metric.label" class="metric-card">
        <strong>{{ metric.value }}</strong>
        <span>{{ metric.label }}</span>
      </div>
    </section>

    <section id="about" class="content-grid section-reveal">
      <div class="section-heading">
        <p class="eyebrow">Tentang Saya</p>
        <h2>Developer yang mengubah kebutuhan teknis menjadi pengalaman produk yang nyaman.</h2>
      </div>
      <div class="glass-card about-card">
        <p>
          Saya terbiasa membangun fitur dari nol: merancang alur data, membuat API,
          menghubungkan database, menyusun UI yang clean, serta menyiapkan deployment.
          Fokus saya adalah membuat solusi yang mudah dibaca user, stabil digunakan, dan siap dikembangkan.
        </p>
        <div class="tech-stack-list">
          <span v-for="stack in techStack" :key="stack">{{ stack }}</span>
        </div>
      </div>
    </section>

    <section id="projects" class="projects-section section-reveal">
      <div class="section-title-row">
        <div>
          <p class="eyebrow">Selected Work</p>
          <h2>Project showcase dengan tampilan yang lebih elegan.</h2>
        </div>
        <div class="slider-actions" aria-label="Project navigation">
          <button type="button" aria-label="Previous project" @click="prevProject">‹</button>
          <button type="button" aria-label="Next project" @click="nextProject">›</button>
        </div>
      </div>

      <div class="project-slider" @touchstart="handleTouchStart" @touchend="handleTouchEnd">
        <article
          v-for="(project, index) in projects"
          :key="project.title"
          class="project-card"
          :class="getProjectClass(index)"
          @click="openProject(project)"
        >
          <div class="project-image-wrap">
            <img :src="project.image" :alt="project.title" class="project-image" loading="lazy" />
            <div class="image-overlay"></div>
            <span class="tap-hint">Klik untuk detail</span>
          </div>
          <div class="project-content">
            <p class="project-type">{{ project.type }}</p>
            <h3>{{ project.title }}</h3>
            <p>{{ project.short }}</p>
            <div class="project-tags">
              <span v-for="tag in project.tags" :key="tag">{{ tag }}</span>
            </div>
            <div class="project-meta">
              <span>Role: Full Stack</span>
              <span>Stack-ready</span>
            </div>
          </div>
        </article>
      </div>

      <div class="project-dots">
        <button
          v-for="(_, index) in projects"
          :key="index"
          type="button"
          :class="{ active: activeProject === index }"
          @click="activeProject = index"
        ></button>
      </div>

      <div class="dashboard-showcase" aria-label="Dashboard project image showcase">
        <button
          v-for="(project, index) in projects"
          :key="`thumb-${project.title}`"
          type="button"
          class="dashboard-thumb"
          :class="{ active: activeProject === index }"
          @click="selectProjectFromThumb(index)"
        >
          <img :src="project.image" :alt="`${project.title} preview`" loading="lazy" />
          <span>{{ project.title }}</span>
        </button>
      </div>
    </section>

    <Transition name="project-modal">
      <section v-if="selectedProject" class="modal-backdrop" @click.self="closeProject">
        <article class="modal-card">
          <button type="button" class="modal-back-button" @click="closeProject">← Kembali</button>
          <img :src="selectedProject.image" :alt="selectedProject.title" class="modal-image" loading="lazy" />
          <div class="modal-body">
            <p class="project-type">{{ selectedProject.type }}</p>
            <h2>{{ selectedProject.title }}</h2>
            <p>{{ selectedProject.description }}</p>
            <div class="project-tags modal-tags">
              <span v-for="tag in selectedProject.tags" :key="tag">{{ tag }}</span>
            </div>
          </div>
        </article>
      </section>
    </Transition>

    <Transition name="project-modal">
      <section v-if="showProfileDetail" class="modal-backdrop" @click.self="closeProfile">
        <article class="modal-card profile-modal-card">
          <button type="button" class="modal-back-button" @click="closeProfile">← Kembali</button>
          <div class="modal-body profile-modal-body">
            <p class="project-type">Profile Detail</p>
            <h2>Kevin Octavius</h2>
            <p>
              Full Stack Developer yang fokus membuat dashboard monitoring, sistem IoT,
              automation bot, dan aplikasi mobile.
            </p>

            <div class="profile-detail-grid">
              <div class="profile-detail-panel">
                <h3>Skills</h3>
                <div class="skills-grid modal-skills-grid">
                  <div v-for="skill in skills" :key="`modal-${skill}`" class="skill-chip">{{ skill }}</div>
                </div>
              </div>

              <div class="profile-detail-panel">
                <h3>Experience</h3>
                <div class="timeline modal-timeline">
                  <div v-for="item in experiences" :key="`modal-${item.title}`" class="timeline-item">
                    <span class="timeline-dot"></span>
                    <div>
                      <h3>{{ item.title }}</h3>
                      <p>{{ item.description }}</p>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </article>
      </section>
    </Transition>

    <section id="skills" class="skills-section section-reveal">
      <div class="section-heading">
        <p class="eyebrow">Skills</p>
        <h2>Tools yang saya gunakan untuk membangun produk modern.</h2>
      </div>
      <div class="tech-card-grid">
        <article v-for="tech in techCards" :key="tech.name" class="tech-card premium-card">
          <div class="tech-icon">{{ tech.icon }}</div>
          <h3>{{ tech.name }}</h3>
          <p>{{ tech.description }}</p>
        </article>
      </div>
    </section>

    <section class="experience-section section-reveal">
      <div class="section-heading">
        <p class="eyebrow">Experience</p>
        <h2>Pengalaman kerja dan mengajar yang membentuk cara saya menyelesaikan masalah.</h2>
      </div>
      <div class="timeline">
        <div v-for="item in experiences" :key="item.title" class="timeline-item">
          <span class="timeline-dot"></span>
          <div>
            <h3>{{ item.title }}</h3>
            <p>{{ item.description }}</p>
          </div>
        </div>
      </div>
    </section>

    <section id="contact" class="contact-section section-reveal">
      <div>
        <p class="eyebrow">Contact</p>
        <h2>Mari diskusikan ide berikutnya.</h2>
        <p class="contact-copy">
          Hubungi saya melalui GitHub, LinkedIn, email, atau WhatsApp. Link masih bisa kamu sesuaikan dengan akun asli milikmu.
        </p>
      </div>
      <div class="contact-links">
        <a v-for="social in socials" :key="`contact-${social.label}`" :href="social.href" target="_blank" rel="noreferrer">
          <span class="social-icon" v-html="social.icon"></span>
          {{ social.label }}
        </a>
        <a href="mailto:kevin@example.com"><span class="social-icon">✉</span>Email</a>
        <a href="https://wa.me/6200000000000" target="_blank" rel="noreferrer"><span class="social-icon">☎</span>WhatsApp</a>
      </div>
    </section>
  </main>
</template>

<script setup>
import { computed, ref } from 'vue';

const projectImage = '/images/art-generator.png';
const techStack = ['Node.js', 'Express', 'Vue', 'Flutter', 'MySQL', 'MQTT', 'Docker', 'Git'];
const featuredStack = ['Vue', 'Node', 'Flutter', 'MQTT'];
const skills = ['JavaScript', 'Node.js', 'Vue.js', 'Flutter', 'SQL', 'MQTT', 'Docker', 'Git'];

const githubIcon = `<svg viewBox="0 0 24 24" aria-hidden="true"><path fill="currentColor" d="M12 2a10 10 0 0 0-3.16 19.49c.5.09.68-.22.68-.48v-1.7c-2.78.6-3.37-1.18-3.37-1.18-.45-1.15-1.1-1.45-1.1-1.45-.9-.62.07-.6.07-.6 1 .07 1.53 1.03 1.53 1.03.89 1.52 2.34 1.08 2.9.83.1-.64.35-1.08.63-1.33-2.22-.25-4.55-1.11-4.55-4.94 0-1.09.39-1.98 1.03-2.68-.1-.25-.45-1.27.1-2.65 0 0 .84-.27 2.75 1.02A9.5 9.5 0 0 1 12 7.03c.85 0 1.7.11 2.5.33 1.9-1.29 2.74-1.02 2.74-1.02.55 1.38.2 2.4.1 2.65.64.7 1.03 1.59 1.03 2.68 0 3.84-2.34 4.69-4.57 4.94.36.31.68.92.68 1.86V21c0 .27.18.58.69.48A10 10 0 0 0 12 2Z"/></svg>`;
const linkedinIcon = `<svg viewBox="0 0 24 24" aria-hidden="true"><path fill="currentColor" d="M6.94 8.98H3.82V20h3.12V8.98ZM5.38 4A1.82 1.82 0 1 0 5.4 7.64 1.82 1.82 0 0 0 5.38 4Zm15 9.68c0-3.35-1.78-4.9-4.16-4.9a3.59 3.59 0 0 0-3.23 1.77V8.98H10V20h3.11v-5.45c0-1.44.27-2.83 2.05-2.83 1.76 0 1.79 1.64 1.79 2.92V20h3.11l.32-6.32Z"/></svg>`;
const twitterIcon = `<svg viewBox="0 0 24 24" aria-hidden="true"><path fill="currentColor" d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.5h1.415l-5.9-7.835a.5.5 0 0 0-.775 0L3.5 19.75H4.915l4.2-4.8 4.2 4.8h1.415zm-6.5-10.5a1.5 1.5 0 1 0 0-3 1.5 1.5 0 0 0 0 3z"/></svg>`;

const metrics = [
  { value: '5+', label: 'Project Showcase' },
  { value: '8', label: 'Core Technologies' },
  { value: 'End-to-End', label: 'Development Flow' },
];

const techCards = [
  { name: 'JavaScript', icon: 'JS', description: 'Membangun logic frontend dan backend yang dinamis, clean, dan mudah dikembangkan.' },
  { name: 'Vue.js', icon: '◇', description: 'Membuat interface interaktif, ringan, dan nyaman digunakan untuk dashboard modern.' },
  { name: 'Node.js', icon: '⬢', description: 'Mengembangkan API, automation service, dan integrasi data untuk kebutuhan operasional.' },
  { name: 'Flutter', icon: '▱', description: 'Membangun aplikasi mobile dengan UI konsisten dan pengalaman pengguna yang smooth.' },
  { name: 'MySQL', icon: 'SQL', description: 'Merancang struktur data dan query untuk kebutuhan aplikasi serta reporting.' },
  { name: 'MQTT', icon: 'IoT', description: 'Menghubungkan device monitoring secara real-time untuk sistem berbasis IoT.' },
  { name: 'Docker', icon: '▣', description: 'Menyiapkan environment yang konsisten untuk development dan deployment.' },
  { name: 'Git', icon: '⌁', description: 'Mengelola version control dan workflow kolaborasi secara rapi.' },
];

const projects = [
  { title: 'DMS Monitoring Dashboard', type: 'Web Dashboard', short: 'Dashboard monitoring real-time dengan visual data yang ringkas dan mudah ditindaklanjuti.', description: 'DMS Monitoring Dashboard menampilkan data operasional secara real-time dengan tampilan ringkas, status perangkat, dan insight yang mudah dibaca oleh user.', tags: ['Vue', 'Node.js', 'MySQL'], image: projectImage },
  { title: 'IoT Power Meter Monitoring', type: 'IoT System', short: 'Sistem monitoring power meter berbasis MQTT untuk membaca data listrik secara real-time.', description: 'Project ini menghubungkan perangkat power meter dengan MQTT untuk membaca data listrik, menampilkan grafik, dan membantu proses analisis konsumsi daya.', tags: ['MQTT', 'Node.js', 'IoT'], image: projectImage },
  { title: 'Telegram Bot Monitoring', type: 'Automation Bot', short: 'Bot alert otomatis agar status sistem dapat dipantau lebih cepat dan praktis.', description: 'Telegram Bot Monitoring mengirimkan alert otomatis saat sistem mendeteksi kondisi tertentu, sehingga user bisa menerima informasi penting lebih cepat.', tags: ['Node.js', 'Telegram API', 'Automation'], image: projectImage },
  { title: 'MyHandyman', type: 'Mobile App', short: 'Aplikasi Flutter untuk membantu user menemukan layanan handyman dan melakukan booking jasa.', description: 'MyHandyman adalah aplikasi mobile yang membantu user mencari layanan handyman, melihat detail jasa, dan melakukan proses booking dengan UI yang sederhana.', tags: ['Flutter', 'Mobile', 'UI/UX'], image: projectImage },
  { title: 'Portfolio Website', type: 'Personal Website', short: 'Website portfolio modern untuk menampilkan profil, skill, project, CV, dan kontak profesional.', description: 'Portfolio ini dibuat dengan Vue dan Vite sebagai tempat menampilkan profile, skill, experience, project, CV, dan kontak profesional.', tags: ['Vue', 'Vite', 'CSS Animation'], image: projectImage },
];

const experiences = [
  { title: 'Internship', description: 'Mengerjakan pengembangan aplikasi, dashboard, integrasi data, troubleshooting fitur, dan penyusunan alur kerja yang lebih efisien.' },
  { title: 'Freelance Coding Teacher', description: 'Mengajar dasar coding, web development, JavaScript, dan membantu murid memahami logic program dengan pendekatan yang sederhana.' },
];

const activeProject = ref(0);
const selectedProject = ref(null);
const showProfileDetail = ref(false);
const touchStartX = ref(0);
const lastProjectIndex = computed(() => projects.length - 1);

function nextProject() {
  activeProject.value = activeProject.value === lastProjectIndex.value ? 0 : activeProject.value + 1;
}

function prevProject() {
  activeProject.value = activeProject.value === 0 ? lastProjectIndex.value : activeProject.value - 1;
}

function getProjectClass(index) {
  if (index === activeProject.value) return 'active';
  if (index === (activeProject.value + 1) % projects.length) return 'next';
  if (index === (activeProject.value - 1 + projects.length) % projects.length) return 'previous';
  return 'hidden-card';
}

function openProject(project) {
  selectedProject.value = project;
}

function selectProjectFromThumb(index) {
  activeProject.value = index;
  selectedProject.value = projects[index];
}

function closeProject() {
  selectedProject.value = null;
}

function openProfile() {
  showProfileDetail.value = true;
}

function closeProfile() {
  showProfileDetail.value = false;
}

function handleTouchStart(event) {
  touchStartX.value = event.changedTouches[0].screenX;
}

function handleTouchEnd(event) {
  const distance = event.changedTouches[0].screenX - touchStartX.value;
  if (distance > 60) prevProject();
  if (distance < -60) nextProject();
}
</script>

<style scoped>
/* === THEME VARIABLES === */
:global(:root) {
  --bg-primary: #05040b;
  --bg-secondary: #0c0920;
  --bg-tertiary: #140927;
  --purple-light: #c084fc;
  --purple-mid: #a855f7;
  --purple-dark: #7c3aed;
  --indigo-accent: #4f46e5;
  --text-primary: #edf2ff;
  --text-secondary: #b9c2d4;
  --text-muted: #8a94a8;
  --glass-bg: rgba(15, 10, 34, 0.78);
  --glass-border: rgba(216, 180, 254, 0.18);
  --card-shadow: 0 24px 70px rgba(0, 0, 0, 0.38), inset 0 1px rgba(255, 255, 255, 0.06);
  --hover-lift: translateY(-8px) scale(1.02);
}

:global(*) { box-sizing: border-box; margin: 0; padding: 0; }
:global(html) { scroll-behavior: smooth; font-size: 16px; }
:global(body) {
  min-width: 320px;
  background: 
    radial-gradient(ellipse at 15% 10%, rgba(168, 85, 247, 0.25), transparent 40%),
    radial-gradient(ellipse at 85% 20%, rgba(79, 70, 229, 0.22), transparent 38%),
    radial-gradient(ellipse at 50% 100%, rgba(88, 28, 135, 0.3), transparent 45%),
    linear-gradient(160deg, var(--bg-primary) 0%, var(--bg-secondary) 45%, var(--bg-tertiary) 100%);
  color: var(--text-primary);
  font-family: 'Inter', ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
  line-height: 1.6;
}
:global(a) { color: inherit; text-decoration: none; }

/* === TYPOGRAPHY SCALE === */
h1 { font-size: clamp(2.8rem, 7vw, 5.8rem); line-height: 0.95; letter-spacing: -0.06em; font-weight: 800; margin-bottom: 1.2rem; }
h2 { font-size: clamp(1.6rem, 3.5vw, 2.8rem); line-height: 1.05; letter-spacing: -0.04em; font-weight: 700; color: var(--text-primary); margin-bottom: 1rem; }
h3 { font-size: clamp(1.1rem, 2vw, 1.35rem); line-height: 1.2; font-weight: 600; color: #fff; margin-bottom: 0.8rem; }
p { color: var(--text-secondary); line-height: 1.7; margin-bottom: 1rem; }
.eyebrow { font-size: 0.75rem; font-weight: 800; letter-spacing: 0.18em; text-transform: uppercase; color: var(--purple-light); margin-bottom: 0.8rem; display: block; }

/* === LAYOUT & SHELL === */
.portfolio-shell { width: min(1200px, calc(100% - 32px)); margin: 0 auto; padding: 24px 0 64px; position: relative; isolation: isolate; }
.nav-bar { position: sticky; top: 16px; z-index: 20; display: flex; justify-content: space-between; align-items: center; gap: 20px; padding: 14px 18px; border: 1px solid var(--glass-border); border-radius: 999px; background: rgba(7, 5, 18, 0.76); backdrop-filter: blur(22px); box-shadow: 0 18px 70px rgba(0,0,0,.32); }
.brand { font-size: 1.2rem; font-weight: 800; text-decoration: none; display: inline-flex; align-items: center; gap: 10px; letter-spacing: -.03em; }
.brand-mark { display: grid; place-items: center; width: 38px; height: 38px; border-radius: 14px; background: linear-gradient(135deg, #a855f7, #4c1d95); color: #fff !important; font-size: .78rem; box-shadow: 0 12px 30px rgba(168,85,247,.35); }
.brand-dot { color: var(--purple-light) !important; }
.nav-links { display: flex; flex-wrap: wrap; gap: 6px; font-size: .92rem; }
.nav-links a { padding: 9px 13px; border-radius: 999px; color: #cbd5e1; transition: color .2s ease, background .2s ease; }
.nav-links a:hover { color: #fff; background: rgba(168,85,247,.14); }

.hero, .content-grid, .contact-section { display: grid; grid-template-columns: 1.12fr .88fr; gap: 28px; align-items: center; }
.hero { min-height: 78vh; padding: 76px 0 42px; }
.section-reveal { animation: fadeUp .8s ease both; }

.status-pill { display: inline-flex; align-items: center; gap: 9px; margin-bottom: 18px; padding: 9px 13px; border: 1px solid rgba(216,180,254,.2); border-radius: 999px; background: rgba(255,255,255,.055); color: #ddd6fe; font-size: .82rem; font-weight: 800; box-shadow: inset 0 1px rgba(255,255,255,.08); }
.status-pill span { width: 8px; height: 8px; border-radius: 50%; background: #22c55e; box-shadow: 0 0 0 6px rgba(34,197,94,.14); }

.hero-description { max-width: 690px; font-size: 1.06rem; }
.hero-actions, .contact-links, .slider-actions { display: flex; flex-wrap: wrap; gap: 12px; }
.primary-button, .secondary-button, .contact-links a, .modal-back-button, .slider-actions button { border: 0; border-radius: 999px; text-decoration: none; cursor: pointer; transition: transform .2s ease, box-shadow .2s ease, background .2s ease; }
.primary-button, .secondary-button, .contact-links a { padding: 12px 18px; font-weight: 800; }
.primary-button { background: linear-gradient(135deg, #c084fc, #7c3aed 54%, #4f46e5); color: #fff; box-shadow: 0 22px 54px rgba(124,58,237,.34); }
.secondary-button, .contact-links a { border: 1px solid var(--glass-border); background: rgba(255,255,255,.065); color: #ede9fe; }
.primary-button:hover, .secondary-button:hover, .contact-links a:hover, .modal-back-button:hover, .slider-actions button:hover { transform: translateY(-3px); box-shadow: 0 16px 44px rgba(88,28,135,.22); border-color: rgba(192,132,252,.48); background: rgba(168,85,247,.16); }

.social-strip { display: flex; flex-wrap: wrap; gap: 12px; margin-top: 24px; }
.social-strip a { padding: 10px 14px; border-radius: 999px; text-decoration: none; font-size: .9rem; font-weight: 900; transition: transform .22s ease, background .22s ease, border-color .22s ease, box-shadow .22s ease; }
.social-icon { display: inline-grid; place-items: center; width: 20px; height: 20px; color: #d8b4fe; vertical-align: middle; margin-right: 6px; }
.social-icon :deep(svg) { width: 20px; height: 20px; display: block; }

/* === CARDS & GLASSMORPHISM === */
.profile-card, .glass-card, .project-card, .timeline-item, .contact-section, .tech-card, .metric-card, .modal-card { border: 1px solid var(--glass-border); background: linear-gradient(145deg, rgba(22,13,44,.86), rgba(12,8,28,.62)); box-shadow: var(--card-shadow); backdrop-filter: blur(22px); position: relative; overflow: hidden; }
.profile-card::before, .glass-card::before, .project-card::before, .timeline-item::before, .contact-section::before, .tech-card::before, .metric-card::before { content: ''; position: absolute; inset: 0; border-radius: inherit; background: linear-gradient(135deg, rgba(255,255,255,.12), transparent 34%, rgba(168,85,247,.12)); pointer-events: none; }

.profile-card { padding: 26px; border-radius: 36px; text-align: center; cursor: pointer; transition: transform .28s ease, box-shadow .28s ease, border-color .28s ease; }
.clickable-profile:hover { transform: translateY(-8px) scale(1.02); border-color: rgba(192,132,252,.46); box-shadow: 0 34px 110px rgba(124,58,237,.24); }
.card-shine { position: absolute; inset: -1px; background: radial-gradient(circle at 50% 0%, rgba(216,180,254,.24), transparent 36%); pointer-events: none; }
.profile-topline, .profile-footer { display: flex; justify-content: space-between; align-items: center; color: #c4b5fd; font-size: .78rem; font-weight: 900; letter-spacing: .08em; text-transform: uppercase; position: relative; }
.avatar-wrap { position: relative; display: grid; place-items: center; min-height: 220px; margin: 12px 0; }
.avatar { width: 138px; height: 138px; border: 1px solid rgba(255,255,255,.14); border-radius: 42px; background: radial-gradient(circle at 30% 20%, #a855f7, #4c1d95 55%, #12091f); font-size: 2.6rem; font-weight: 1000; box-shadow: 0 30px 70px rgba(76,29,149,.45), inset 0 1px rgba(255,255,255,.2); display: grid; place-items: center; color: #fff; }
.orbital-badge { position: absolute; z-index: 2; display: grid; place-items: center; min-width: 50px; height: 38px; padding: 0 10px; border: 1px solid rgba(216,180,254,.22); border-radius: 999px; background: rgba(15,10,34,.82); color: #f5f3ff; font-size: .78rem; font-weight: 1000; box-shadow: 0 12px 30px rgba(0,0,0,.28); animation: floatBadge 4s ease-in-out infinite; }
.badge-js { top: 24px; left: 20px; color: #fde68a; }
.badge-vue { right: 18px; top: 62px; color: #86efac; animation-delay: .4s; }
.badge-node { bottom: 24px; left: 50%; transform: translateX(-50%); color: #bef264; animation-delay: .8s; }

.profile-card h2 { margin-bottom: 10px; font-size: 1.8rem; letter-spacing: -.04em; }
.profile-card p { color: #cbd5e1; line-height: 1.65; }
.profile-footer { padding-top: 16px; border-top: 1px solid rgba(216,180,254,.14); }

.mini-stack, .tech-stack-list, .project-tags, .skills-grid { display: flex; flex-wrap: wrap; gap: 10px; }
.mini-stack { justify-content: center; margin-top: 22px; }
.mini-stack span, .tech-stack-list span, .project-tags span, .skill-chip { border: 1px solid rgba(192,132,252,.26); border-radius: 999px; background: rgba(168,85,247,.1); color: #e9d5ff; font-size: .84rem; font-weight: 700; padding: 7px 10px; }

/* === METRICS & GRID SECTIONS === */
.metrics-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 14px; margin-bottom: 26px; }
.metric-card { padding: 22px; border-radius: 26px; }
.metric-card strong { position: relative; display: block; color: #fff; font-size: clamp(1.45rem, 3vw, 2.2rem); letter-spacing: -.04em; }
.metric-card span { position: relative; color: var(--text-secondary); font-size: .92rem; font-weight: 700; }

.content-grid, .projects-section, .skills-section, .experience-section, .contact-section { padding: 70px 0; }
.glass-card, .timeline-item, .contact-section { border-radius: 28px; padding: 28px; }
.tech-stack-list { margin-top: 22px; }

.section-title-row { display: flex; align-items: end; justify-content: space-between; gap: 20px; margin-bottom: 26px; }
.slider-actions button { display: grid; place-items: center; width: 46px; height: 46px; background: rgba(255,255,255,.08); color: #fff; font-size: 1.8rem; border: 1px solid var(--glass-border); }

/* === PROJECT SLIDER === */
.project-slider { position: relative; min-height: 510px; perspective: 1400px; overflow: hidden; }
.project-card { position: absolute; inset: 0; display: grid; grid-template-columns: minmax(0, 1fr) minmax(280px, .78fr); gap: 24px; align-items: center; padding: 24px; border-radius: 34px; opacity: 0; pointer-events: none; transform: translateX(0) scale(.86); transition: opacity .45s ease, transform .55s cubic-bezier(.2,.8,.2,1), filter .45s ease; }
.project-card.active { z-index: 3; opacity: 1; pointer-events: auto; transform: translateX(0) scale(1); }
.project-card.next { z-index: 2; opacity: .42; transform: translateX(58%) scale(.82) rotateY(-16deg); filter: blur(1px); }
.project-card.previous { z-index: 1; opacity: .24; transform: translateX(-58%) scale(.82) rotateY(16deg); filter: blur(1px); }
.project-card.hidden-card { transform: translateX(120%) scale(.74); }

.project-image-wrap { position: relative; overflow: hidden; min-height: 360px; border-radius: 26px; background: linear-gradient(135deg, rgba(168,85,247,.2), rgba(49,46,129,.24)); box-shadow: inset 0 1px rgba(255,255,255,.08); }
.project-image { width: 100%; height: 100%; min-height: 360px; object-fit: cover; display: block; transition: transform .45s ease, filter .45s ease; }
.project-card.active:hover .project-image { transform: scale(1.08); filter: saturate(1.18); }
.image-overlay { position: absolute; inset: 0; background: linear-gradient(180deg, transparent 45%, rgba(8,5,20,.78)); }
.tap-hint { position: absolute; left: 18px; bottom: 18px; padding: 8px 12px; border-radius: 999px; background: rgba(8,5,20,.72); color: #fff; font-size: .84rem; font-weight: 800; border: 1px solid rgba(255,255,255,.18); backdrop-filter: blur(12px); }

.project-type { color: var(--purple-light); font-size: .82rem; font-weight: 900; letter-spacing: .12em; text-transform: uppercase; margin-bottom: 6px; }
.project-content h3 { font-size: clamp(1.8rem, 4vw, 3.2rem); line-height: 1; letter-spacing: -.05em; }
.project-meta { display: flex; flex-wrap: wrap; gap: 10px; margin-top: 24px; color: #c4b5fd; font-size: .82rem; font-weight: 900; }
.project-meta span { padding: 8px 11px; border-radius: 999px; background: rgba(255,255,255,.06); }

.project-dots { display: flex; justify-content: center; gap: 10px; margin-top: 20px; }
.project-dots button { width: 10px; height: 10px; border: 0; border-radius: 999px; background: rgba(255,255,255,.24); cursor: pointer; transition: width .25s ease, background .25s ease; }
.project-dots button.active { width: 34px; background: linear-gradient(135deg, #c084fc, #7c3aed); }

.dashboard-showcase { display: grid; grid-template-columns: repeat(5, minmax(160px, 1fr)); gap: 14px; margin-top: 28px; padding: 16px; border: 1px solid var(--glass-border); border-radius: 28px; background: rgba(255,255,255,.045); overflow-x: auto; }
.dashboard-thumb { min-width: 160px; border: 1px solid var(--glass-border); border-radius: 20px; padding: 10px; background: rgba(255,255,255,.055); color: #e9d5ff; text-align: left; cursor: pointer; transition: transform .25s ease, border-color .25s ease, background .25s ease; }
.dashboard-thumb:hover, .dashboard-thumb.active { transform: translateY(-6px); border-color: rgba(192,132,252,.5); background: rgba(168,85,247,.15); }
.dashboard-thumb img { width: 100%; height: 86px; display: block; object-fit: cover; border-radius: 14px; margin-bottom: 10px; }
.dashboard-thumb span { display: block; font-size: .78rem; font-weight: 900; line-height: 1.25; }

/* === MODALS === */
.modal-backdrop { position: fixed; inset: 0; z-index: 50; display: grid; place-items: center; padding: 24px; background: rgba(4,2,12,.78); backdrop-filter: blur(16px); }
.modal-card { width: min(920px, 100%); max-height: min(86vh, 820px); overflow: auto; border-radius: 34px; background: linear-gradient(145deg, rgba(22,13,44,.96), rgba(12,8,28,.92)); }
.modal-back-button { margin: 18px 18px 0; padding: 10px 14px; background: rgba(255,255,255,.08); color: #fff; font-weight: 900; border-radius: 999px; }
.modal-image { width: calc(100% - 36px); max-height: 420px; margin: 18px; border-radius: 24px; object-fit: cover; }
.modal-body { padding: 0 28px 30px; }
.profile-modal-card { width: min(1040px, 100%); }
.profile-modal-body { padding-top: 18px; }
.profile-detail-grid { display: grid; grid-template-columns: .9fr 1.1fr; gap: 18px; margin-top: 26px; }
.profile-detail-panel { padding: 20px; border-radius: 24px; background: rgba(255,255,255,.045); }
.modal-skills-grid { grid-template-columns: repeat(2, minmax(0, 1fr)); }
.modal-timeline .timeline-item { background: rgba(15,23,42,.5); }
.modal-tags { margin-top: 20px; }

.project-modal-enter-active, .project-modal-leave-active { transition: opacity .28s ease; }
.project-modal-enter-from, .project-modal-leave-to { opacity: 0; }
.project-modal-enter-active .modal-card { animation: zoomIn .42s cubic-bezier(.2,.8,.2,1) both; }
.project-modal-leave-active .modal-card { animation: zoomOut .26s ease both; }

/* === SKILLS & TECH CARDS === */
.skills-grid { display: grid; grid-template-columns: repeat(4, 1fr); }
.skill-chip { padding: 18px; text-align: center; font-size: 1rem; transition: transform .2s ease, background .2s ease; }
.skill-chip:hover { transform: translateY(-6px) scale(1.03); background: rgba(56,189,248,.16); }

.tech-card-grid { display: grid; grid-template-columns: repeat(4, minmax(0, 1fr)); gap: 16px; }
.tech-card { min-height: 220px; padding: 22px; border-radius: 28px; transition: transform .24s ease, border-color .24s ease, box-shadow .24s ease; }
.tech-card:hover { transform: translateY(-8px); border-color: rgba(192,132,252,.44); box-shadow: 0 28px 84px rgba(88,28,135,.25); }
.tech-icon { display: grid; place-items: center; width: 56px; height: 56px; margin-bottom: 18px; border: 1px solid var(--glass-border); border-radius: 19px; background: linear-gradient(135deg, rgba(168,85,247,.28), rgba(79,70,229,.14)); color: #fff; font-weight: 1000; }
.tech-card h3 { margin-bottom: 10px; font-size: 1.08rem; }
.tech-card p { font-size: .92rem; }

/* === TIMELINE & CONTACT === */
.timeline { display: grid; gap: 16px; }
.timeline-item { display: grid; grid-template-columns: auto 1fr; gap: 16px; padding: 20px; border-radius: 24px; background: rgba(15,23,42,.5); }
.timeline-dot { width: 14px; height: 14px; margin-top: 6px; border-radius: 50%; background: #c084fc; box-shadow: 0 0 0 8px rgba(192,132,252,.12), 0 0 28px rgba(192,132,252,.42); }
.contact-section { margin-top: 40px; }
.contact-links { justify-content: flex-end; }

/* === ANIMATIONS === */
@keyframes fadeUp { from { opacity: 0; transform: translateY(28px); } to { opacity: 1; transform: translateY(0); } }
@keyframes floatBadge { 0%, 100% { translate: 0 0; } 50% { translate: 0 -10px; } }
@keyframes zoomIn { from { opacity: 0; transform: scale(.72) translateY(40px); } to { opacity: 1; transform: scale(1) translateY(0); } }
@keyframes zoomOut { from { opacity: 1; transform: scale(1) translateY(0); } to { opacity: 0; transform: scale(.82) translateY(24px); } }

/* === RESPONSIVE === */
@media (max-width: 860px) {
  .portfolio-shell { width: min(100% - 20px, 1180px); }
  .hero, .content-grid, .contact-section { grid-template-columns: 1fr; }
  .nav-bar, .section-title-row { align-items: flex-start; }
  .nav-bar { border-radius: 24px; }
  .hero { min-height: auto; padding-top: 48px; }
  .project-slider { min-height: 650px; }
  .project-card { grid-template-columns: 1fr; }
  .project-card.next { transform: translateX(72%) scale(.78); }
  .project-card.previous { transform: translateX(-72%) scale(.78); }
  .skills-grid { grid-template-columns: repeat(2, 1fr); }
  .tech-card-grid { grid-template-columns: repeat(2, minmax(0, 1fr)); }
  .metrics-grid { grid-template-columns: 1fr; }
  .dashboard-showcase { grid-template-columns: repeat(5, 180px); }
  .profile-detail-grid { grid-template-columns: 1fr; }
  .contact-links { justify-content: flex-start; }
}
@media (max-width: 520px) {
  .project-slider { min-height: 710px; }
  .project-card { padding: 16px; border-radius: 24px; }
  .project-image, .project-image-wrap { min-height: 260px; }
  .skills-grid { grid-template-columns: 1fr; }
  .tech-card-grid { grid-template-columns: 1fr; }
}
</style>
