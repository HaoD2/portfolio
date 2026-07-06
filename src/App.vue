<template>
    <main class="portfolio-shell">
        <!-- NAV -->
        <nav class="nav-bar" ref="navBar">
            <a href="#about" class="brand" aria-label="Kevin Octavius portfolio">
                <span class="brand-mark">KO</span>
                <span>Kevin<span class="brand-dot">.</span></span>
            </a>
            <div class="nav-links">
                <a v-for="item in navItems" :key="item.href"
                :href="item.href"
                class="nav-link"
                :class="{ active: activeSection === item.href.slice(1) }"
                @click="handleNavClick(item.href.slice(1), $event)">
                {{ item.label }}
                <span class="nav-indicator"></span>
            </a>
        </div>
    </nav>

    <!-- ====== ABOUT ME (Hero) ====== -->
    <section id="about" class="about-section section-reveal">
        <div class="about-container">

            <!-- Left column -->
            <div class="about-left">
                <div class="about-header">
                    <span class="about-badge">✦ About Me</span>
                    <div class="about-header-line"></div>
                </div>

                <div class="about-name-block">
                    <span class="about-initials">KO</span>
                    <h1 class="about-name">Kevin Octavius</h1>
                </div>

                <p class="about-role">Software Engineer &amp; Full-Stack Developer</p>

                <p class="about-description">
                    I'm Kevin Octavius, a Software Engineer with a passion for building beautiful, functional mobile
                    and web applications. I specialize in Flutter development and full-stack solutions,
                    combining technical expertise with creative problem-solving.
                </p>

                <div class="about-availability">
                    <div class="availability-badge">
                        <span class="avail-dot"></span>
                        AVAILABLE FOR WORK
                    </div>
                    <p class="availability-cta">
                        <span>HAVE A VISION?</span>
                        <strong>LET'S BUILD IT</strong>
                        together.
                    </p>
                    <a href="/cv-kevin-octavius.pdf" class="resume-link" download>
                        <span class="resume-icon">📄</span> Resume
                        <span class="resume-arrow">→</span>
                    </a>
                </div>

                <div class="about-meta-grid">
                    <div class="about-meta-item">
                        <span class="meta-icon">🌍</span>
                        <div>
                            <strong>FLEXIBLE WITH TIMEOZONES</strong>
                            <p>Based in Oman, available globally</p>
                        </div>
                    </div>
                    <div class="about-meta-item">
                        <span class="meta-icon">🎓</span>
                        <div>
                            <strong>SOHAR UNIVERSITY</strong>
                            <p>Bachelor's in Software Engineering<br>2023 – 2026</p>
                        </div>
                    </div>
                </div>

                <div class="about-quote">
                    <span class="quote-mark">“</span>
                    <p>Real artists ship.</p>
                    <span class="quote-attribution">STEVE JOBS</span>
                </div>
            </div>

            <!-- Right column: stats -->
            <div class="about-right">
                <div class="about-stats-grid">
                    <div v-for="stat in aboutStats" :key="stat.label" class="about-stat-card">
                        <span class="stat-number">{{ stat.value }}</span>
                        <span class="stat-label">{{ stat.label }}</span>
                        <div class="stat-shimmer"></div>
                    </div>
                </div>
                <div class="about-tech-tags">
                    <span v-for="tag in aboutTechTags" :key="tag">{{ tag }}</span>
                </div>
            </div>

        </div>
    </section>
    <!-- ====== END ABOUT ====== -->

    <!-- METRICS -->
    <section class="metrics-grid section-reveal" aria-label="Portfolio highlights">
        <div v-for="metric in metrics" :key="metric.label" class="metric-card">
            <strong>{{ metric.value }}</strong>
            <span>{{ metric.label }}</span>
        </div>
    </section>

    <!-- PROJECTS -->
    <section id="projects" class="projects-section section-reveal">
        <div class="section-title-row">
            <div>
                <p class="eyebrow">Selected Work</p>
                <h2>Project showcase dengan tampilan yang lebih elegan.</h2>
            </div>
            <div class="slider-actions" aria-label="Project navigation">
                <button type="button" class="slider-btn slider-btn-prev" aria-label="Previous project"
                @click="prevProject">
                <span class="btn-arrow">‹</span>
                <span class="btn-glow"></span>
            </button>
            <button type="button" class="slider-btn slider-btn-next" aria-label="Next project"
            @click="nextProject">
            <span class="btn-arrow">›</span>
            <span class="btn-glow"></span>
        </button>
    </div>
</div>

<div class="swipe-hint">
    <span class="swipe-icon">↔</span>
    <span>Swipe atau seret kartu untuk menjelajahi project</span>
</div>

<div class="project-slider"
@mousedown="handleDragStart"
@touchstart="handleDragStart"
@mousemove="handleDragMove"
@touchmove="handleDragMove"
@mouseup="handleDragEnd"
@touchend="handleDragEnd"
@mouseleave="handleDragEnd"
ref="sliderRef">

<div class="drag-track" :style="{ transform: `scaleX(${dragProgress})`, opacity: isDragging ? 1 : 0 }"></div>

<article v-for="(project, index) in projects" :key="project.title"
class="project-card"
:class="[getProjectClass(index), { 'card-clicked': clickedCard === index }]"
:style="getCardStyle(index)"
@click="!isDragging && handleProjectClick(index)">
<div class="project-image-wrap">
    <img :src="project.image" :alt="project.title" class="project-image" loading="lazy" />
    <div class="image-overlay"></div>
    <span class="tap-hint">Klik untuk detail</span>
    <div class="card-ripple" v-if="clickedCard === index"></div>
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
    <button v-for="(_, index) in projects" :key="index" type="button"
    :class="{ active: activeProject === index }"
    @click="activeProject = index"></button>
</div>

<div class="dashboard-showcase" aria-label="Dashboard project image showcase">
    <button v-for="(project, index) in projects" :key="`thumb-${project.title}`" type="button"
    class="dashboard-thumb"
    :class="{ active: activeProject === index }"
    @click="selectProjectFromThumb(index)">
    <img :src="project.image" :alt="`${project.title} preview`" loading="lazy" />
    <span>{{ project.title }}</span>
</button>
</div>
</section>

<!-- MODALS -->
<Transition name="project-modal">
    <section v-if="selectedProject" class="modal-backdrop" @click.self="closeProject">
        <article class="modal-card modal-enter">
            <button type="button" class="modal-back-button" @click="closeProject">
                <span class="back-arrow">←</span> Kembali
            </button>
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

<!-- SKILLS -->
<section id="skills" class="skills-section section-reveal">
    <div class="section-heading">
        <p class="eyebrow">Skills</p>
        <h2>Tools yang saya gunakan untuk membangun produk modern.</h2>
    </div>
    <div class="tech-card-grid">
        <article v-for="tech in techCards" :key="tech.name" class="tech-card premium-card"
        @click="handleCardClick">
        <div class="tech-icon">{{ tech.icon }}</div>
        <h3>{{ tech.name }}</h3>
        <p>{{ tech.description }}</p>
        <div class="tech-glow"></div>
    </article>
</div>
</section>

<!-- EXPERIENCE -->
<section class="experience-section section-reveal">
    <div class="section-heading">
        <p class="eyebrow">Experience</p>
        <h2>Pengalaman kerja dan mengajar yang membentuk cara saya menyelesaikan masalah.</h2>
    </div>
    <div class="timeline">
        <div v-for="(item, idx) in experiences" :key="item.title" class="timeline-item"
        :style="{ animationDelay: `${idx * 0.15}s` }"
        @click="handleCardClick">
        <span class="timeline-dot"></span>
        <div>
            <h3>{{ item.title }}</h3>
            <p>{{ item.description }}</p>
        </div>
    </div>
</div>
</section>

<!-- CONTACT -->
<section id="contact" class="contact-section section-reveal">
    <div>
        <p class="eyebrow">Contact</p>
        <h2>Mari diskusikan ide berikutnya.</h2>
        <p class="contact-copy">
            Hubungi saya melalui GitHub, LinkedIn, email, atau WhatsApp. Link masih bisa kamu sesuaikan dengan akun asli milikmu.
        </p>
    </div>
    <div class="contact-links">
        <a v-for="social in socials" :key="`contact-${social.label}`" :href="social.href" target="_blank"
        rel="noreferrer" class="contact-link">
        <span class="social-icon" v-html="social.icon"></span>
        {{ social.label }}
    </a>
    <a href="mailto:kevin@example.com" class="contact-link">
        <span class="social-icon">✉</span>Email
    </a>
    <a href="https://wa.me/6200000000000" target="_blank" rel="noreferrer" class="contact-link">
        <span class="social-icon">☎</span>WhatsApp
    </a>
</div>
</section>
</main>
</template>

<script setup>
import { computed, ref, nextTick, onMounted, onUnmounted } from 'vue';

// ─── IMAGE GENERATOR ───────────────────────────────────────────────
function generatePlaceholderImage(title, color, darkColor) {
    const initial = title.charAt(0);
    const svg = `<svg xmlns="http://www.w3.org/2000/svg" width="800" height="500" viewBox="0 0 800 500">
      <defs>
        <linearGradient id="g-${title.replace(/\s/g,'')}" x1="0%" y1="0%" x2="100%" y2="100%">
          <stop offset="0%" style="stop-color:${color};stop-opacity:0.92" />
          <stop offset="50%" style="stop-color:${darkColor || color};stop-opacity:0.75" />
          <stop offset="100%" style="stop-color:${darkColor || color};stop-opacity:0.45" />
        </linearGradient>
        <radialGradient id="r-${title.replace(/\s/g,'')}" cx="30%" cy="20%" r="60%">
          <stop offset="0%" style="stop-color:#ffffff;stop-opacity:0.18" />
          <stop offset="100%" style="stop-color:#ffffff;stop-opacity:0" />
        </radialGradient>
      </defs>
      <rect width="800" height="500" fill="url(#g-${title.replace(/\s/g,'')})" />
      <rect width="800" height="500" fill="url(#r-${title.replace(/\s/g,'')})" />
      <text x="400" y="235" font-family="Inter, system-ui, sans-serif" font-size="140" font-weight="800" fill="white" text-anchor="middle" dominant-baseline="central" opacity="0.92" letter-spacing="-0.04em">${initial}</text>
      <text x="400" y="320" font-family="Inter, system-ui, sans-serif" font-size="26" font-weight="600" fill="white" text-anchor="middle" dominant-baseline="central" opacity="0.75" letter-spacing="0.02em">${title}</text>
      <circle cx="680" cy="80" r="120" fill="white" opacity="0.04" />
      <circle cx="80" cy="420" r="80" fill="white" opacity="0.03" />
    </svg>`;
    return `data:image/svg+xml;charset=utf-8,${encodeURIComponent(svg)}`;
}

// ─── PROJECT DATA ──────────────────────────────────────────────────
const projectColors = [
    { primary: '#4f46e5', dark: '#3730a3' },
    { primary: '#0891b2', dark: '#0e7490' },
    { primary: '#7c3aed', dark: '#5b21b6' },
    { primary: '#d97706', dark: '#b45309' },
    { primary: '#db2777', dark: '#be185d' },
];

const projectData = [
    { title: 'DMS Monitoring Dashboard', type: 'Web Dashboard', short: 'Dashboard monitoring real-time dengan visual data yang ringkas dan mudah ditindaklanjuti.', description: 'DMS Monitoring Dashboard menampilkan data operasional secara real-time dengan tampilan ringkas, status perangkat, dan insight yang mudah dibaca oleh user.', tags: ['Vue', 'Node.js', 'MySQL'] },
    { title: 'IoT Power Meter Monitoring', type: 'IoT System', short: 'Sistem monitoring power meter berbasis MQTT untuk membaca data listrik secara real-time.', description: 'Project ini menghubungkan perangkat power meter dengan MQTT untuk membaca data listrik, menampilkan grafik, dan membantu proses analisis konsumsi daya.', tags: ['MQTT', 'Node.js', 'IoT'] },
    { title: 'Telegram Bot Monitoring', type: 'Automation Bot', short: 'Bot alert otomatis agar status sistem dapat dipantau lebih cepat dan praktis.', description: 'Telegram Bot Monitoring mengirimkan alert otomatis saat sistem mendeteksi kondisi tertentu, sehingga user bisa menerima informasi penting lebih cepat.', tags: ['Node.js', 'Telegram API', 'Automation'] },
    { title: 'MyHandyman', type: 'Mobile App', short: 'Aplikasi Flutter untuk membantu user menemukan layanan handyman dan melakukan booking jasa.', description: 'MyHandyman adalah aplikasi mobile yang membantu user mencari layanan handyman, melihat detail jasa, dan melakukan proses booking dengan UI yang sederhana.', tags: ['Flutter', 'Mobile', 'UI/UX'] },
    { title: 'Portfolio Website', type: 'Personal Website', short: 'Website portfolio modern untuk menampilkan profil, skill, project, CV, dan kontak profesional.', description: 'Portfolio ini dibuat dengan Vue dan Vite sebagai tempat menampilkan profile, skill, experience, project, CV, dan kontak profesional.', tags: ['Vue', 'Vite', 'CSS Animation'] },
];

const projects = projectData.map((p, i) => ({
    ...p,
    image: generatePlaceholderImage(p.title, projectColors[i % projectColors.length].primary, projectColors[i % projectColors.length].dark),
}));

// ─── NAV ITEMS ─────────────────────────────────────────────────────
const navItems = [
    { label: 'About', href: '#about' },
    { label: 'Projects', href: '#projects' },
    { label: 'Skills', href: '#skills' },
    { label: 'Contact', href: '#contact' },
];

// ─── STATE ──────────────────────────────────────────────────────────
const activeProject = ref(0);
const selectedProject = ref(null);
const sliderRef = ref(null);
const navBar = ref(null);
const activeSection = ref('about');
const clickedCard = ref(null);

// Drag state
const isDragging = ref(false);
const dragStartX = ref(0);
const dragOffset = ref(0);
const dragProgress = ref(0);
const dragThreshold = 40;
const isDragIntent = ref(false);

// ─── COMPUTED ──────────────────────────────────────────────────────
const lastProjectIndex = computed(() => projects.length - 1);

// ─── NAVIGATION ────────────────────────────────────────────────────
function nextProject() {
    if (isDragging.value) return;
    animateProjectTransition(() => {
        activeProject.value = activeProject.value === lastProjectIndex.value ? 0 : activeProject.value + 1;
    });
    resetDrag();
}

function prevProject() {
    if (isDragging.value) return;
    animateProjectTransition(() => {
        activeProject.value = activeProject.value === 0 ? lastProjectIndex.value : activeProject.value - 1;
    });
    resetDrag();
}

function animateProjectTransition(callback) {
    const slider = document.querySelector('.project-slider');
    if (slider) {
        slider.style.transition = 'transform 0.15s ease';
        slider.style.transform = 'scale(0.98)';
        setTimeout(() => {
            slider.style.transform = 'scale(1)';
            setTimeout(() => {
                slider.style.transition = '';
            }, 150);
        }, 150);
    }
    callback();
}

function getProjectClass(index) {
    if (index === activeProject.value) return 'active';
    if (index === (activeProject.value + 1) % projects.length) return 'next';
    if (index === (activeProject.value - 1 + projects.length) % projects.length) return 'previous';
    return 'hidden-card';
}

function getCardStyle(index) {
    if (!isDragging.value) return {};
    const diff = index - activeProject.value;
    let normalized = diff;
    if (diff > projects.length / 2) normalized = diff - projects.length;
    if (diff < -projects.length / 2) normalized = diff + projects.length;

    const progress = dragProgress.value;
    if (Math.abs(normalized) > 1) return {};

    const offset = normalized * 100 * progress + progress * 100 * (normalized > 0 ? 1 : -1) * 0.2;
    return {
        transform: `translateX(${offset * 0.6}%) scale(${1 - Math.abs(offset) * 0.0012})`,
        opacity: 1 - Math.abs(offset) * 0.004,
        pointerEvents: isDragging.value ? 'none' : 'auto',
        transition: 'none',
    };
}

function handleProjectClick(index) {
    if (isDragging.value || isDragIntent.value) return;
    clickedCard.value = index;
    setTimeout(() => { clickedCard.value = null; }, 600);
    setTimeout(() => {
        selectedProject.value = projects[index];
    }, 200);
}

function selectProjectFromThumb(index) {
    activeProject.value = index;
    handleProjectClick(index);
}

function closeProject() {
    selectedProject.value = null;
}

// ─── NAV MENU ──────────────────────────────────────────────────────
function handleNavClick(section, event) {
    event.preventDefault();
    activeSection.value = section;
    const target = document.getElementById(section);
    if (target) {
        const navHeight = navBar.value?.offsetHeight || 80;
        const offset = target.getBoundingClientRect().top + window.pageYOffset - navHeight - 20;
        window.scrollTo({ top: offset, behavior: 'smooth' });
    }
    setTimeout(() => {
        updateActiveSection();
    }, 500);
}

function updateActiveSection() {
    const sections = navItems.map(item => item.href.slice(1));
    const scrollY = window.scrollY + 120;
    for (const section of sections) {
        const el = document.getElementById(section);
        if (el) {
            const rect = el.getBoundingClientRect();
            const top = rect.top + window.pageYOffset;
            const bottom = rect.bottom + window.pageYOffset;
            if (scrollY >= top && scrollY < bottom) {
                activeSection.value = section;
                break;
            }
        }
    }
}

// ─── CARD CLICK ANIMATION ─────────────────────────────────────────
function handleCardClick(event) {
    const el = event.currentTarget;
    el.style.transition = 'transform 0.12s cubic-bezier(.2,.8,.2,1), box-shadow 0.12s ease';
    el.style.transform = 'scale(0.97)';
    el.style.boxShadow = '0 4px 20px rgba(168,85,247,0.15)';
    setTimeout(() => {
        el.style.transform = 'scale(1)';
        el.style.boxShadow = '';
        setTimeout(() => {
            el.style.transition = '';
        }, 150);
    }, 150);
}

// ─── DRAG / SWIPE HANDLING ────────────────────────────────────────
function resetDrag() {
    isDragging.value = false;
    isDragIntent.value = false;
    dragOffset.value = 0;
    dragProgress.value = 0;
    dragStartX.value = 0;
}

function handleDragStart(e) {
    const clientX = e.type === 'touchstart' ? e.touches[0].clientX : e.clientX;
    dragStartX.value = clientX;
    isDragging.value = true;
    isDragIntent.value = false;
    dragOffset.value = 0;
    dragProgress.value = 0;
    document.body.style.userSelect = 'none';
    document.body.style.cursor = 'grabbing';
}

function handleDragMove(e) {
    if (!isDragging.value) return;
    const clientX = e.type === 'touchmove' ? e.touches[0].clientX : e.clientX;
    const delta = clientX - dragStartX.value;
    if (Math.abs(delta) > 12) {
        isDragIntent.value = true;
    }
    const maxDrag = 300;
    const clamped = Math.max(-maxDrag, Math.min(maxDrag, delta));
    dragOffset.value = clamped;
    dragProgress.value = Math.abs(clamped) / maxDrag;
    if (e.type === 'touchmove' && isDragIntent.value) {
        e.preventDefault();
    }
}

function handleDragEnd(e) {
    if (!isDragging.value) {
        resetDrag();
        return;
    }
    const delta = dragOffset.value;
    if (isDragIntent.value && Math.abs(delta) > dragThreshold) {
        if (delta < 0) {
            nextProject();
        } else {
            prevProject();
        }
    } else {
        resetDrag();
        nextTick(() => {});
    }
    isDragging.value = false;
    isDragIntent.value = false;
    dragOffset.value = 0;
    dragProgress.value = 0;
    document.body.style.userSelect = '';
    document.body.style.cursor = '';
}

// ─── KEYBOARD NAVIGATION ──────────────────────────────────────────
function handleKeydown(e) {
    if (e.key === 'ArrowRight') { e.preventDefault();
        nextProject(); }
    if (e.key === 'ArrowLeft') { e.preventDefault();
        prevProject(); }
    if (e.key === 'Escape') {
        if (selectedProject.value) closeProject();
    }
}

// ─── LIFECYCLE ─────────────────────────────────────────────────────
onMounted(() => {
    document.addEventListener('keydown', handleKeydown);
    window.addEventListener('scroll', updateActiveSection);
    updateActiveSection();

    const observer = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
            if (entry.isIntersecting) {
                entry.target.classList.add('section-visible');
            }
        });
    }, { threshold: 0.15 });

    document.querySelectorAll('.section-reveal').forEach(el => {
        observer.observe(el);
    });
});

onUnmounted(() => {
    document.removeEventListener('keydown', handleKeydown);
    window.removeEventListener('scroll', updateActiveSection);
    document.body.style.userSelect = '';
    document.body.style.cursor = '';
});

// ─── STATIC DATA ───────────────────────────────────────────────────
const githubIcon = `<svg viewBox="0 0 24 24" aria-hidden="true"><path fill="currentColor" d="M12 2a10 10 0 0 0-3.16 19.49c.5.09.68-.22.68-.48v-1.7c-2.78.6-3.37-1.18-3.37-1.18-.45-1.15-1.1-1.45-1.1-1.45-.9-.62.07-.6.07-.6 1 .07 1.53 1.03 1.53 1.03.89 1.52 2.34 1.08 2.9.83.1-.64.35-1.08.63-1.33-2.22-.25-4.55-1.11-4.55-4.94 0-1.09.39-1.98 1.03-2.68-.1-.25-.45-1.27.1-2.65 0 0 .84-.27 2.75 1.02A9.5 9.5 0 0 1 12 7.03c.85 0 1.7.11 2.5.33 1.9-1.29 2.74-1.02 2.74-1.02.55 1.38.2 2.4.1 2.65.64.7 1.03 1.59 1.03 2.68 0 3.84-2.34 4.69-4.57 4.94.36.31.68.92.68 1.86V21c0 .27.18.58.69.48A10 10 0 0 0 12 2Z"/></svg>`;
const linkedinIcon = `<svg viewBox="0 0 24 24" aria-hidden="true"><path fill="currentColor" d="M6.94 8.98H3.82V20h3.12V8.98ZM5.38 4A1.82 1.82 0 1 0 5.4 7.64 1.82 1.82 0 0 0 5.38 4Zm15 9.68c0-3.35-1.78-4.9-4.16-4.9a3.59 3.59 0 0 0-3.23 1.77V8.98H10V20h3.11v-5.45c0-1.44.27-2.83 2.05-2.83 1.76 0 1.79 1.64 1.79 2.92V20h3.11l.32-6.32Z"/></svg>`;
const twitterIcon = `<svg viewBox="0 0 24 24" aria-hidden="true"><path fill="currentColor" d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.5h1.415l-5.9-7.835a.5.5 0 0 0-.775 0L3.5 19.75H4.915l4.2-4.8 4.2 4.8h1.415zm-6.5-10.5a1.5 1.5 0 1 0 0-3 1.5 1.5 0 0 0 0 3z"/></svg>`;

const socials = [
    { label: 'GitHub', href: 'https://github.com/kevinoctavius', icon: githubIcon },
    { label: 'LinkedIn', href: 'https://linkedin.com/in/kevinoctavius', icon: linkedinIcon },
    { label: 'Twitter', href: 'https://twitter.com/kevinoctavius', icon: twitterIcon },
];

const metrics = [
    { value: '5+', label: 'Project Showcase' },
    { value: '8', label: 'Core Technologies' },
    { value: 'End-to-End', label: 'Development Flow' },
];

const featuredStack = ['Flutter', 'Full-Stack', 'Mobile', 'Web'];
const skills = ['JavaScript', 'Node.js', 'Vue.js', 'Flutter', 'SQL', 'MQTT', 'Docker', 'Git'];

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

const experiences = [
    { title: 'Internship', description: 'Mengerjakan pengembangan aplikasi, dashboard, integrasi data, troubleshooting fitur, dan penyusunan alur kerja yang lebih efisien.' },
    { title: 'Freelance Coding Teacher', description: 'Mengajar dasar coding, web development, JavaScript, dan membantu murid memahami logic program dengan pendekatan yang sederhana.' },
];

// ─── ABOUT STATS ──────────────────────────────────────────────────
const aboutStats = [
    { value: '20+', label: 'Projects' },
    { value: '3+', label: 'Years of Experience' },
    { value: '5+', label: 'Published Platforms' },
    { value: '15+', label: 'Technical Skills' },
];

const aboutTechTags = ['Flutter', 'Full-Stack', 'Mobile', 'Web', 'IoT', 'API'];
</script>

<style scoped>
/* ─── THEME VARIABLES ─────────────────────────────────────────────── */
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

:global(*) {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}
:global(html) {
    scroll-behavior: smooth;
    font-size: 16px;
}
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
:global(a) {
    color: inherit;
    text-decoration: none;
}

/* ─── TYPOGRAPHY ──────────────────────────────────────────────────── */
h1 {
    font-size: clamp(2.8rem, 7vw, 5.8rem);
    line-height: 0.95;
    letter-spacing: -0.06em;
    font-weight: 800;
    margin-bottom: 1.2rem;
}
h2 {
    font-size: clamp(1.6rem, 3.5vw, 2.8rem);
    line-height: 1.05;
    letter-spacing: -0.04em;
    font-weight: 700;
    color: var(--text-primary);
    margin-bottom: 1rem;
}
h3 {
    font-size: clamp(1.1rem, 2vw, 1.35rem);
    line-height: 1.2;
    font-weight: 600;
    color: #fff;
    margin-bottom: 0.8rem;
}
p {
    color: var(--text-secondary);
    line-height: 1.7;
    margin-bottom: 1rem;
}
.eyebrow {
    font-size: 0.75rem;
    font-weight: 800;
    letter-spacing: 0.18em;
    text-transform: uppercase;
    color: var(--purple-light);
    margin-bottom: 0.8rem;
    display: block;
}

/* ─── LAYOUT ──────────────────────────────────────────────────────── */
.portfolio-shell {
    width: min(1200px, calc(100% - 32px));
    margin: 0 auto;
    padding: 24px 0 64px;
    position: relative;
    isolation: isolate;
}

/* ─── NAV BAR ─────────────────────────────────────────────────────── */
.nav-bar {
    position: sticky;
    top: 16px;
    z-index: 20;
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 20px;
    padding: 14px 18px;
    border: 1px solid var(--glass-border);
    border-radius: 999px;
    background: rgba(7, 5, 18, 0.76);
    backdrop-filter: blur(22px);
    box-shadow: 0 18px 70px rgba(0, 0, 0, .32);
}
.brand {
    font-size: 1.2rem;
    font-weight: 800;
    text-decoration: none;
    display: inline-flex;
    align-items: center;
    gap: 10px;
    letter-spacing: -.03em;
}
.brand-mark {
    display: grid;
    place-items: center;
    width: 38px;
    height: 38px;
    border-radius: 14px;
    background: linear-gradient(135deg, #a855f7, #4c1d95);
    color: #fff !important;
    font-size: .78rem;
    box-shadow: 0 12px 30px rgba(168, 85, 247, .35);
    transition: transform 0.3s ease;
}
.brand:hover .brand-mark {
    transform: rotate(-8deg) scale(1.05);
}
.brand-dot {
    color: var(--purple-light) !important;
}
.nav-links {
    display: flex;
    flex-wrap: wrap;
    gap: 4px;
    font-size: .92rem;
}
.nav-link {
    position: relative;
    padding: 9px 16px;
    border-radius: 999px;
    color: #cbd5e1;
    transition: color 0.3s ease, background 0.3s ease;
    cursor: pointer;
    font-weight: 500;
    overflow: hidden;
}
.nav-link:hover {
    color: #fff;
    background: rgba(168, 85, 247, .12);
}
.nav-link.active {
    color: #fff;
    background: rgba(168, 85, 247, .18);
}
.nav-indicator {
    position: absolute;
    bottom: 4px;
    left: 50%;
    width: 0;
    height: 2.5px;
    border-radius: 4px;
    background: linear-gradient(90deg, var(--purple-mid), var(--indigo-accent));
    transform: translateX(-50%);
    transition: width 0.35s cubic-bezier(.2, .8, .2, 1);
}
.nav-link.active .nav-indicator {
    width: 60%;
}
.nav-link:hover .nav-indicator {
    width: 40%;
}
.nav-link.active:hover .nav-indicator {
    width: 70%;
}

/* ─── SECTION REVEAL ────────────────────────────────────────────── */
.section-reveal {
    opacity: 0;
    transform: translateY(40px);
    transition: opacity 0.8s cubic-bezier(.2, .8, .2, 1), transform 0.8s cubic-bezier(.2, .8, .2, 1);
}
.section-reveal.section-visible {
    opacity: 1;
    transform: translateY(0);
}
.section-reveal:nth-child(2) {
    transition-delay: 0.05s;
}
.section-reveal:nth-child(3) {
    transition-delay: 0.1s;
}
.section-reveal:nth-child(4) {
    transition-delay: 0.15s;
}
.section-reveal:nth-child(5) {
    transition-delay: 0.2s;
}
.section-reveal:nth-child(6) {
    transition-delay: 0.25s;
}

/* ─── BUTTONS ────────────────────────────────────────────────────── */
.primary-button,
.secondary-button,
.contact-links a,
.modal-back-button,
.slider-actions button {
    border: 0;
    border-radius: 999px;
    text-decoration: none;
    cursor: pointer;
    transition: transform 0.25s cubic-bezier(.2, .8, .2, 1),
        box-shadow 0.25s ease,
        background 0.25s ease;
    position: relative;
    overflow: hidden;
}
.primary-button,
.secondary-button,
.contact-links a {
    padding: 12px 22px;
    font-weight: 800;
    display: inline-flex;
    align-items: center;
    gap: 8px;
}
.primary-button {
    background: linear-gradient(135deg, #c084fc, #7c3aed 54%, #4f46e5);
    color: #fff;
    box-shadow: 0 22px 54px rgba(124, 58, 237, .34);
}
.primary-button .btn-sparkle {
    display: inline-block;
    font-size: 1.1rem;
    transition: transform 0.3s ease;
}
.primary-button:hover .btn-sparkle {
    transform: rotate(90deg) scale(1.2);
}
.secondary-button .btn-icon {
    display: inline-block;
    transition: transform 0.3s ease;
}
.secondary-button:hover .btn-icon {
    transform: translateY(2px);
}
.secondary-button,
.contact-links a {
    border: 1px solid var(--glass-border);
    background: rgba(255, 255, 255, .065);
    color: #ede9fe;
}
.primary-button:hover,
.secondary-button:hover,
.contact-links a:hover,
.modal-back-button:hover {
    transform: translateY(-4px) scale(1.02);
    box-shadow: 0 20px 50px rgba(88, 28, 135, .3);
    border-color: rgba(192, 132, 252, .48);
    background: rgba(168, 85, 247, .16);
}
.primary-button:hover {
    background: linear-gradient(135deg, #d4a0ff, #8b5cf6 54%, #6366f1);
    box-shadow: 0 28px 64px rgba(124, 58, 237, .45);
}
.primary-button:active,
.secondary-button:active,
.contact-links a:active {
    transform: scale(0.94);
}

/* ─── SOCIAL ────────────────────────────────────────────────────── */
.social-strip {
    display: flex;
    flex-wrap: wrap;
    gap: 12px;
    margin-top: 24px;
}
.social-link {
    padding: 10px 16px;
    border-radius: 999px;
    text-decoration: none;
    font-size: .9rem;
    font-weight: 700;
    border: 1px solid rgba(216, 180, 254, .12);
    background: rgba(255, 255, 255, .035);
    transition: transform 0.3s cubic-bezier(.2, .8, .2, 1),
        background 0.3s ease,
        border-color 0.3s ease,
        box-shadow 0.3s ease;
    display: inline-flex;
    align-items: center;
    gap: 6px;
}
.social-link:hover {
    transform: translateY(-4px) scale(1.04);
    background: rgba(168, 85, 247, .12);
    border-color: rgba(192, 132, 252, .4);
    box-shadow: 0 12px 30px rgba(168, 85, 247, .15);
}
.social-link:active {
    transform: scale(0.92);
}
.social-icon {
    display: inline-grid;
    place-items: center;
    width: 20px;
    height: 20px;
    color: #d8b4fe;
    vertical-align: middle;
}
.social-icon :deep(svg) {
    width: 20px;
    height: 20px;
    display: block;
}

/* ─── METRICS ────────────────────────────────────────────────────── */
.metrics-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 14px;
    margin-bottom: 26px;
}
.metric-card {
    padding: 22px;
    border-radius: 26px;
    border: 1px solid var(--glass-border);
    background: linear-gradient(145deg, rgba(22, 13, 44, .86), rgba(12, 8, 28, .62));
    box-shadow: var(--card-shadow);
    backdrop-filter: blur(22px);
    transition: transform 0.3s cubic-bezier(.2, .8, .2, 1),
        box-shadow 0.3s ease,
        border-color 0.3s ease;
    cursor: default;
}
.metric-card:hover {
    transform: translateY(-6px) scale(1.02);
    border-color: rgba(192, 132, 252, .3);
    box-shadow: 0 20px 50px rgba(88, 28, 135, .15);
}
.metric-card:active {
    transform: scale(0.96);
}
.metric-card strong {
    position: relative;
    display: block;
    color: #fff;
    font-size: clamp(1.45rem, 3vw, 2.2rem);
    letter-spacing: -.04em;
}
.metric-card span {
    position: relative;
    color: var(--text-secondary);
    font-size: .92rem;
    font-weight: 700;
}

/* ═══════════════════════════════════════════════════════════════════
   ABOUT SECTION (Hero)
   ═══════════════════════════════════════════════════════════════════ */
.about-section {
    padding: 40px 0 30px;
    position: relative;
    min-height: 70vh;
    display: flex;
    align-items: center;
}

.about-container {
    display: grid;
    grid-template-columns: 1.2fr 0.8fr;
    gap: 40px;
    align-items: start;
    width: 100%;
}

/* ─── LEFT COLUMN ────────────────────────────────────────────────── */
.about-left {
    display: flex;
    flex-direction: column;
    gap: 18px;
}

/* "About Me" header */
.about-header {
    display: flex;
    align-items: center;
    gap: 14px;
    margin-bottom: 4px;
}
.about-badge {
    font-size: 0.82rem;
    font-weight: 800;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    color: var(--purple-light);
    background: rgba(168, 85, 247, 0.12);
    padding: 4px 14px 4px 12px;
    border-radius: 999px;
    border: 1px solid rgba(192, 132, 252, 0.15);
    display: inline-flex;
    align-items: center;
    gap: 6px;
}
.about-header-line {
    flex: 1;
    height: 1.5px;
    background: linear-gradient(90deg, rgba(192, 132, 252, 0.3), transparent 90%);
    border-radius: 2px;
}

/* Name block */
.about-name-block {
    display: flex;
    align-items: baseline;
    gap: 12px;
    flex-wrap: wrap;
}
.about-initials {
    font-size: clamp(2.8rem, 5vw, 4.2rem);
    font-weight: 900;
    letter-spacing: -0.04em;
    background: linear-gradient(135deg, #c084fc, #7c3aed 50%, #4f46e5);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    line-height: 1;
}
.about-name {
    font-size: clamp(2rem, 3.6vw, 3.2rem);
    font-weight: 800;
    letter-spacing: -0.04em;
    color: #fff;
    margin: 0;
    line-height: 1;
}

/* Role */
.about-role {
    font-size: 1.05rem;
    font-weight: 600;
    color: var(--text-secondary);
    margin: -2px 0 2px;
    letter-spacing: -0.01em;
}

/* Description */
.about-description {
    font-size: 1rem;
    color: var(--text-secondary);
    line-height: 1.7;
    max-width: 560px;
    margin: 0 0 4px;
}

/* ─── AVAILABILITY ────────────────────────────────────────────────── */
.about-availability {
    background: linear-gradient(145deg, rgba(22, 13, 44, 0.7), rgba(12, 8, 28, 0.5));
    border: 1px solid var(--glass-border);
    border-radius: 24px;
    padding: 20px 24px 22px;
    margin: 4px 0 6px;
    backdrop-filter: blur(8px);
    transition: border-color 0.3s ease, box-shadow 0.3s ease;
}
.about-availability:hover {
    border-color: rgba(192, 132, 252, 0.3);
    box-shadow: 0 8px 30px rgba(88, 28, 135, 0.1);
}

.availability-badge {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    font-size: 0.7rem;
    font-weight: 900;
    letter-spacing: 0.14em;
    color: #86efac;
    text-transform: uppercase;
    background: rgba(34, 197, 94, 0.08);
    padding: 4px 14px 4px 10px;
    border-radius: 999px;
    border: 1px solid rgba(34, 197, 94, 0.18);
}
.avail-dot {
    width: 7px;
    height: 7px;
    border-radius: 50%;
    background: #22c55e;
    box-shadow: 0 0 0 4px rgba(34, 197, 94, 0.12);
    animation: pulse-dot 2s ease-in-out infinite;
}

.availability-cta {
    font-size: 1.15rem;
    font-weight: 500;
    color: var(--text-secondary);
    margin: 10px 0 8px;
    line-height: 1.3;
}
.availability-cta span {
    font-weight: 400;
    color: var(--text-muted);
}
.availability-cta strong {
    color: #fff;
    font-weight: 800;
    background: linear-gradient(135deg, #c084fc, #a855f7);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
}

.resume-link {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    padding: 10px 20px;
    border-radius: 999px;
    background: linear-gradient(135deg, rgba(168, 85, 247, 0.18), rgba(79, 70, 229, 0.12));
    border: 1px solid rgba(192, 132, 252, 0.2);
    color: #ede9fe;
    font-weight: 700;
    font-size: 0.9rem;
    transition: transform 0.25s cubic-bezier(.2, .8, .2, 1),
        background 0.25s ease,
        border-color 0.25s ease,
        box-shadow 0.25s ease;
    text-decoration: none;
    margin-top: 2px;
}
.resume-link:hover {
    transform: translateY(-3px) scale(1.03);
    background: linear-gradient(135deg, rgba(168, 85, 247, 0.28), rgba(79, 70, 229, 0.2));
    border-color: rgba(192, 132, 252, 0.45);
    box-shadow: 0 12px 36px rgba(168, 85, 247, 0.15);
}
.resume-link:active {
    transform: scale(0.94);
}
.resume-icon {
    font-size: 1rem;
}
.resume-arrow {
    display: inline-block;
    transition: transform 0.3s ease;
}
.resume-link:hover .resume-arrow {
    transform: translateX(4px);
}

/* ─── META GRID ──────────────────────────────────────────────────── */
.about-meta-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 12px;
    margin: 2px 0 4px;
}

.about-meta-item {
    display: flex;
    gap: 12px;
    padding: 14px 16px;
    border-radius: 18px;
    background: rgba(255, 255, 255, 0.035);
    border: 1px solid rgba(216, 180, 254, 0.07);
    transition: background 0.3s ease, border-color 0.3s ease, transform 0.3s ease;
}
.about-meta-item:hover {
    background: rgba(255, 255, 255, 0.06);
    border-color: rgba(192, 132, 252, 0.15);
    transform: translateY(-2px);
}

.meta-icon {
    font-size: 1.4rem;
    flex-shrink: 0;
    margin-top: 1px;
}
.about-meta-item strong {
    display: block;
    font-size: 0.78rem;
    font-weight: 800;
    letter-spacing: 0.03em;
    color: #e9d5ff;
    text-transform: uppercase;
}
.about-meta-item p {
    font-size: 0.85rem;
    color: var(--text-secondary);
    margin: 2px 0 0;
    line-height: 1.4;
}

/* ─── QUOTE ──────────────────────────────────────────────────────── */
.about-quote {
    display: flex;
    align-items: baseline;
    gap: 6px;
    padding: 12px 0 0;
    border-top: 1px solid rgba(216, 180, 254, 0.08);
    margin-top: 4px;
}
.quote-mark {
    font-size: 1.8rem;
    font-weight: 900;
    color: var(--purple-mid);
    line-height: 1;
    opacity: 0.5;
}
.about-quote p {
    font-size: 1rem;
    font-weight: 600;
    color: #e9d5ff;
    margin: 0;
    letter-spacing: -0.01em;
}
.quote-attribution {
    font-size: 0.75rem;
    font-weight: 800;
    letter-spacing: 0.12em;
    color: var(--text-muted);
    text-transform: uppercase;
    margin-left: 4px;
}

/* ─── RIGHT COLUMN: STATS ───────────────────────────────────────── */
.about-right {
    display: flex;
    flex-direction: column;
    gap: 18px;
}

.about-stats-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 14px;
}

.about-stat-card {
    position: relative;
    padding: 20px 16px;
    border-radius: 24px;
    background: linear-gradient(145deg, rgba(22, 13, 44, 0.7), rgba(12, 8, 28, 0.5));
    border: 1px solid var(--glass-border);
    text-align: center;
    transition: transform 0.35s cubic-bezier(.2, .8, .2, 1),
        border-color 0.35s ease,
        box-shadow 0.35s ease;
    overflow: hidden;
    cursor: default;
}
.about-stat-card:hover {
    transform: translateY(-6px) scale(1.02);
    border-color: rgba(192, 132, 252, 0.25);
    box-shadow: 0 16px 48px rgba(88, 28, 135, 0.12);
}
.about-stat-card:active {
    transform: scale(0.95);
}

.stat-number {
    display: block;
    font-size: clamp(2.2rem, 4vw, 3.2rem);
    font-weight: 900;
    letter-spacing: -0.04em;
    background: linear-gradient(135deg, #c084fc, #7c3aed 50%, #4f46e5);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    line-height: 1.1;
}
.stat-label {
    display: block;
    font-size: 0.8rem;
    font-weight: 700;
    color: var(--text-secondary);
    letter-spacing: 0.02em;
    margin-top: 4px;
}

.stat-shimmer {
    position: absolute;
    top: -50%;
    left: -50%;
    width: 200%;
    height: 200%;
    background: radial-gradient(circle at 30% 20%, rgba(168, 85, 247, 0.05), transparent 60%);
    pointer-events: none;
    opacity: 0;
    transition: opacity 0.5s ease;
}
.about-stat-card:hover .stat-shimmer {
    opacity: 1;
}

/* ─── TECH TAGS ──────────────────────────────────────────────────── */
.about-tech-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    justify-content: center;
    padding: 14px 16px;
    border-radius: 20px;
    background: rgba(255, 255, 255, 0.03);
    border: 1px solid rgba(216, 180, 254, 0.06);
}
.about-tech-tags span {
    padding: 6px 14px;
    border-radius: 999px;
    background: rgba(168, 85, 247, 0.08);
    border: 1px solid rgba(192, 132, 252, 0.12);
    color: #e9d5ff;
    font-size: 0.78rem;
    font-weight: 600;
    transition: transform 0.2s ease, background 0.2s ease;
}
.about-tech-tags span:hover {
    transform: translateY(-2px) scale(1.05);
    background: rgba(168, 85, 247, 0.16);
}

/* ═══════════════════════════════════════════════════════════════════
   PROJECTS SECTION
   ═══════════════════════════════════════════════════════════════════ */
.projects-section {
    padding: 70px 0;
}
.section-title-row {
    display: flex;
    align-items: end;
    justify-content: space-between;
    gap: 20px;
    margin-bottom: 26px;
}
.slider-actions {
    display: flex;
    gap: 12px;
}
.slider-btn {
    position: relative;
    display: grid;
    place-items: center;
    width: 56px;
    height: 56px;
    border-radius: 50%;
    background: rgba(255, 255, 255, .06);
    color: #fff;
    border: 1px solid var(--glass-border);
    cursor: pointer;
    transition: transform 0.3s cubic-bezier(.2, .8, .2, 1),
        background 0.3s ease,
        border-color 0.3s ease,
        box-shadow 0.3s ease;
    overflow: visible;
}
.slider-btn .btn-arrow {
    font-size: 2rem;
    line-height: 1;
    position: relative;
    z-index: 2;
    transition: transform 0.3s ease;
}
.slider-btn .btn-glow {
    position: absolute;
    inset: -2px;
    border-radius: 50%;
    background: radial-gradient(circle at center, rgba(168, 85, 247, 0.3), transparent 70%);
    opacity: 0;
    transition: opacity 0.4s ease;
    pointer-events: none;
}
.slider-btn:hover {
    transform: translateY(-3px) scale(1.08);
    background: rgba(168, 85, 247, .18);
    border-color: rgba(192, 132, 252, .5);
    box-shadow: 0 12px 40px rgba(168, 85, 247, .25);
}
.slider-btn:hover .btn-glow {
    opacity: 1;
}
.slider-btn:hover .btn-arrow {
    transform: scale(1.15);
}
.slider-btn:active {
    transform: scale(0.88);
}
.slider-btn:active .btn-arrow {
    transform: scale(0.85);
}
.slider-btn-prev:hover .btn-arrow {
    transform: translateX(-3px) scale(1.15);
}
.slider-btn-next:hover .btn-arrow {
    transform: translateX(3px) scale(1.15);
}

.swipe-hint {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 12px;
    margin-bottom: 20px;
    padding: 10px 18px;
    border-radius: 999px;
    background: rgba(255, 255, 255, .04);
    border: 1px solid rgba(216, 180, 254, .08);
    color: var(--text-muted);
    font-size: .82rem;
    font-weight: 500;
    animation: fadeUp 0.6s ease both 0.3s;
}
.swipe-icon {
    display: inline-block;
    font-size: 1.2rem;
    animation: swipePulse 2.5s ease-in-out infinite;
}

.project-slider {
    position: relative;
    min-height: 510px;
    perspective: 1400px;
    overflow: hidden;
    touch-action: pan-y;
    cursor: grab;
    border-radius: 34px;
}
.project-slider:active {
    cursor: grabbing;
}

.drag-track {
    position: absolute;
    bottom: 0;
    left: 0;
    height: 4px;
    background: linear-gradient(90deg, var(--purple-mid), var(--indigo-accent));
    border-radius: 4px;
    transform-origin: left;
    z-index: 10;
    transition: opacity 0.3s ease;
    box-shadow: 0 0 20px rgba(168, 85, 247, 0.3);
}

.project-card {
    position: absolute;
    inset: 0;
    display: grid;
    grid-template-columns: minmax(0, 1fr) minmax(280px, .78fr);
    gap: 24px;
    align-items: center;
    padding: 24px;
    border-radius: 34px;
    border: 1px solid var(--glass-border);
    background: linear-gradient(145deg, rgba(22, 13, 44, .86), rgba(12, 8, 28, .62));
    box-shadow: var(--card-shadow);
    backdrop-filter: blur(22px);
    opacity: 0;
    pointer-events: none;
    transform: translateX(0) scale(.86);
    transition: opacity 0.5s cubic-bezier(.2, .8, .2, 1),
        transform 0.5s cubic-bezier(.2, .8, .2, 1),
        filter 0.5s cubic-bezier(.2, .8, .2, 1);
    will-change: transform, opacity;
    cursor: pointer;
}
.project-card.active {
    z-index: 3;
    opacity: 1;
    pointer-events: auto;
    transform: translateX(0) scale(1);
}
.project-card.next {
    z-index: 2;
    opacity: 0.42;
    transform: translateX(58%) scale(.82) rotateY(-12deg);
    filter: blur(0.8px);
}
.project-card.previous {
    z-index: 1;
    opacity: 0.24;
    transform: translateX(-58%) scale(.82) rotateY(12deg);
    filter: blur(0.8px);
}
.project-card.hidden-card {
    transform: translateX(120%) scale(.74);
    opacity: 0;
    pointer-events: none;
}

.project-card.card-clicked {
    animation: cardPop 0.5s cubic-bezier(.2, .8, .2, 1) forwards;
}
@keyframes cardPop {
    0% {
        transform: scale(1);
    }
    30% {
        transform: scale(0.94);
        box-shadow: 0 4px 20px rgba(168, 85, 247, 0.3);
    }
    60% {
        transform: scale(1.03);
        box-shadow: 0 20px 60px rgba(168, 85, 247, 0.2);
    }
    100% {
        transform: scale(1);
        box-shadow: var(--card-shadow);
    }
}

.card-ripple {
    position: absolute;
    inset: 0;
    border-radius: 26px;
    pointer-events: none;
    background: radial-gradient(circle at center, rgba(168, 85, 247, 0.25), transparent 70%);
    animation: rippleOut 0.6s cubic-bezier(.2, .8, .2, 1) forwards;
}
@keyframes rippleOut {
    0% {
        transform: scale(0.5);
        opacity: 1;
    }
    100% {
        transform: scale(2);
        opacity: 0;
    }
}

.project-card:active:not(.hidden-card) {
    transform: scale(0.97);
    transition: transform 0.1s ease;
}
.project-card.active:active {
    transform: scale(0.97);
}

.project-image-wrap {
    position: relative;
    overflow: hidden;
    min-height: 360px;
    border-radius: 26px;
    background: linear-gradient(135deg, rgba(168, 85, 247, .2), rgba(49, 46, 129, .24));
    box-shadow: inset 0 1px rgba(255, 255, 255, .08);
}
.project-image {
    width: 100%;
    height: 100%;
    min-height: 360px;
    object-fit: cover;
    display: block;
    transition: transform 0.5s cubic-bezier(.2, .8, .2, 1),
        filter 0.5s ease;
}
.project-card.active:hover .project-image {
    transform: scale(1.08);
    filter: saturate(1.18);
}
.image-overlay {
    position: absolute;
    inset: 0;
    background: linear-gradient(180deg, transparent 45%, rgba(8, 5, 20, .78));
}
.tap-hint {
    position: absolute;
    left: 18px;
    bottom: 18px;
    padding: 8px 12px;
    border-radius: 999px;
    background: rgba(8, 5, 20, .72);
    color: #fff;
    font-size: .84rem;
    font-weight: 800;
    border: 1px solid rgba(255, 255, 255, .18);
    backdrop-filter: blur(12px);
    transition: transform 0.3s ease, opacity 0.3s ease;
}
.project-card.active:hover .tap-hint {
    transform: translateY(-4px);
    opacity: 0.7;
}

.project-type {
    color: var(--purple-light);
    font-size: .82rem;
    font-weight: 900;
    letter-spacing: .12em;
    text-transform: uppercase;
    margin-bottom: 6px;
}
.project-content h3 {
    font-size: clamp(1.8rem, 4vw, 3.2rem);
    line-height: 1;
    letter-spacing: -.05em;
}
.project-meta {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    margin-top: 24px;
    color: #c4b5fd;
    font-size: .82rem;
    font-weight: 900;
}
.project-meta span {
    padding: 8px 11px;
    border-radius: 999px;
    background: rgba(255, 255, 255, .06);
}

.project-dots {
    display: flex;
    justify-content: center;
    gap: 10px;
    margin-top: 20px;
}
.project-dots button {
    width: 10px;
    height: 10px;
    border: 0;
    border-radius: 999px;
    background: rgba(255, 255, 255, .24);
    cursor: pointer;
    transition: width 0.4s cubic-bezier(.2, .8, .2, 1),
        background 0.4s ease,
        transform 0.3s ease;
}
.project-dots button:hover {
    transform: scale(1.3);
    background: rgba(255, 255, 255, .4);
}
.project-dots button.active {
    width: 34px;
    background: linear-gradient(135deg, #c084fc, #7c3aed);
    transform: scale(1);
}
.project-dots button.active:hover {
    transform: scale(1.1);
}

.dashboard-showcase {
    display: grid;
    grid-template-columns: repeat(5, minmax(160px, 1fr));
    gap: 14px;
    margin-top: 28px;
    padding: 16px;
    border: 1px solid var(--glass-border);
    border-radius: 28px;
    background: rgba(255, 255, 255, .045);
    overflow-x: auto;
}
.dashboard-thumb {
    min-width: 160px;
    border: 1px solid var(--glass-border);
    border-radius: 20px;
    padding: 10px;
    background: rgba(255, 255, 255, .055);
    color: #e9d5ff;
    text-align: left;
    cursor: pointer;
    transition: transform 0.35s cubic-bezier(.2, .8, .2, 1),
        border-color 0.35s ease,
        background 0.35s ease,
        box-shadow 0.35s ease;
}
.dashboard-thumb:hover {
    transform: translateY(-6px) scale(1.03);
    border-color: rgba(192, 132, 252, .5);
    background: rgba(168, 85, 247, .15);
    box-shadow: 0 12px 30px rgba(168, 85, 247, .12);
}
.dashboard-thumb:active {
    transform: scale(0.94);
}
.dashboard-thumb.active {
    transform: translateY(-6px);
    border-color: rgba(192, 132, 252, .6);
    background: rgba(168, 85, 247, .2);
    box-shadow: 0 12px 40px rgba(168, 85, 247, .2);
}
.dashboard-thumb img {
    width: 100%;
    height: 86px;
    display: block;
    object-fit: cover;
    border-radius: 14px;
    margin-bottom: 10px;
    transition: transform 0.3s ease;
}
.dashboard-thumb:hover img {
    transform: scale(1.04);
}
.dashboard-thumb span {
    display: block;
    font-size: .78rem;
    font-weight: 900;
    line-height: 1.25;
}

/* ─── MODALS ─────────────────────────────────────────────────────── */
.modal-backdrop {
    position: fixed;
    inset: 0;
    z-index: 50;
    display: grid;
    place-items: center;
    padding: 24px;
    background: rgba(4, 2, 12, .78);
    backdrop-filter: blur(16px);
}
.modal-card {
    width: min(920px, 100%);
    max-height: min(86vh, 820px);
    overflow: auto;
    border-radius: 34px;
    border: 1px solid var(--glass-border);
    background: linear-gradient(145deg, rgba(22, 13, 44, .96), rgba(12, 8, 28, .92));
    box-shadow: var(--card-shadow);
    backdrop-filter: blur(22px);
    transform-origin: center;
}
.modal-back-button {
    margin: 18px 18px 0;
    padding: 10px 18px;
    background: rgba(255, 255, 255, .08);
    color: #fff;
    font-weight: 900;
    border-radius: 999px;
    display: inline-flex;
    align-items: center;
    gap: 8px;
    border: 1px solid rgba(255, 255, 255, .06);
}
.modal-back-button .back-arrow {
    display: inline-block;
    transition: transform 0.3s ease;
}
.modal-back-button:hover .back-arrow {
    transform: translateX(-4px);
}
.modal-image {
    width: calc(100% - 36px);
    max-height: 420px;
    margin: 18px;
    border-radius: 24px;
    object-fit: cover;
}
.modal-body {
    padding: 0 28px 30px;
}
.modal-tags {
    margin-top: 20px;
}

.project-modal-enter-active,
.project-modal-leave-active {
    transition: opacity 0.35s ease;
}
.project-modal-enter-from,
.project-modal-leave-to {
    opacity: 0;
}
.project-modal-enter-active .modal-card {
    animation: modalEnter 0.45s cubic-bezier(.2, .8, .2, 1) both;
}
.project-modal-leave-active .modal-card {
    animation: modalExit 0.3s cubic-bezier(.2, .8, .2, 1) both;
}
@keyframes modalEnter {
    0% {
        opacity: 0;
        transform: scale(0.8) translateY(30px) rotateX(-6deg);
    }
    100% {
        opacity: 1;
        transform: scale(1) translateY(0) rotateX(0);
    }
}
@keyframes modalExit {
    0% {
        opacity: 1;
        transform: scale(1) translateY(0);
    }
    100% {
        opacity: 0;
        transform: scale(0.9) translateY(20px);
    }
}

/* ─── SKILLS ────────────────────────────────────────────────────── */
.skills-section {
    padding: 70px 0;
}
.section-heading {
    margin-bottom: 32px;
}
.tech-card-grid {
    display: grid;
    grid-template-columns: repeat(4, minmax(0, 1fr));
    gap: 16px;
}
.tech-card {
    min-height: 220px;
    padding: 22px;
    border-radius: 28px;
    border: 1px solid var(--glass-border);
    background: linear-gradient(145deg, rgba(22, 13, 44, .86), rgba(12, 8, 28, .62));
    box-shadow: var(--card-shadow);
    backdrop-filter: blur(22px);
    transition: transform 0.4s cubic-bezier(.2, .8, .2, 1),
        border-color 0.4s ease,
        box-shadow 0.4s ease;
    cursor: pointer;
    position: relative;
}
.tech-card:hover {
    transform: translateY(-8px) scale(1.02);
    border-color: rgba(192, 132, 252, .44);
    box-shadow: 0 28px 84px rgba(88, 28, 135, .25);
}
.tech-card:active {
    transform: scale(0.94);
}
.tech-glow {
    position: absolute;
    inset: 0;
    border-radius: 28px;
    background: radial-gradient(circle at var(--mouse-x, 50%) var(--mouse-y, 50%),
            rgba(168, 85, 247, 0.08),
            transparent 60%);
    opacity: 0;
    transition: opacity 0.5s ease;
    pointer-events: none;
}
.tech-card:hover .tech-glow {
    opacity: 1;
}
.tech-icon {
    display: grid;
    place-items: center;
    width: 56px;
    height: 56px;
    margin-bottom: 18px;
    border: 1px solid var(--glass-border);
    border-radius: 19px;
    background: linear-gradient(135deg, rgba(168, 85, 247, .28), rgba(79, 70, 229, .14));
    color: #fff;
    font-weight: 1000;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
}
.tech-card:hover .tech-icon {
    transform: scale(1.08) rotate(-4deg);
    box-shadow: 0 8px 24px rgba(168, 85, 247, .2);
}
.tech-card h3 {
    margin-bottom: 10px;
    font-size: 1.08rem;
}
.tech-card p {
    font-size: .92rem;
}

/* ─── EXPERIENCE ───────────────────────────────────────────────── */
.experience-section {
    padding: 70px 0;
}
.timeline {
    display: grid;
    gap: 16px;
}
.timeline-item {
    display: grid;
    grid-template-columns: auto 1fr;
    gap: 16px;
    padding: 20px;
    border-radius: 24px;
    border: 1px solid var(--glass-border);
    background: linear-gradient(145deg, rgba(22, 13, 44, .86), rgba(12, 8, 28, .62));
    box-shadow: var(--card-shadow);
    backdrop-filter: blur(22px);
    opacity: 0;
    transform: translateX(-20px);
    animation: slideInItem 0.5s cubic-bezier(.2, .8, .2, 1) forwards;
    cursor: default;
}
@keyframes slideInItem {
    to {
        opacity: 1;
        transform: translateX(0);
    }
}
.timeline-item:hover {
    transform: translateX(4px) translateY(-4px);
    box-shadow: 0 12px 40px rgba(88, 28, 135, .12);
    border-color: rgba(192, 132, 252, .2);
}
.timeline-item:active {
    transform: scale(0.98);
}
.timeline-dot {
    width: 14px;
    height: 14px;
    margin-top: 6px;
    border-radius: 50%;
    background: #c084fc;
    box-shadow: 0 0 0 8px rgba(192, 132, 252, .12), 0 0 28px rgba(192, 132, 252, .42);
    transition: transform 0.3s ease, box-shadow 0.3s ease;
    flex-shrink: 0;
}
.timeline-item:hover .timeline-dot {
    transform: scale(1.3);
    box-shadow: 0 0 0 12px rgba(192, 132, 252, .18), 0 0 40px rgba(192, 132, 252, .5);
}

/* ─── CONTACT ────────────────────────────────────────────────────── */
.contact-section {
    margin-top: 40px;
    padding: 70px 0;
    display: grid;
    grid-template-columns: 1.12fr .88fr;
    gap: 28px;
    align-items: center;
    border: 1px solid var(--glass-border);
    background: linear-gradient(145deg, rgba(22, 13, 44, .86), rgba(12, 8, 28, .62));
    box-shadow: var(--card-shadow);
    backdrop-filter: blur(22px);
    border-radius: 28px;
    padding: 28px;
}
.contact-copy {
    max-width: 500px;
}
.contact-links {
    display: flex;
    flex-wrap: wrap;
    gap: 12px;
    justify-content: flex-end;
}
.contact-link {
    padding: 12px 20px;
    border-radius: 999px;
    border: 1px solid rgba(216, 180, 254, .12);
    background: rgba(255, 255, 255, .035);
    font-weight: 700;
    display: inline-flex;
    align-items: center;
    gap: 8px;
    transition: transform 0.3s cubic-bezier(.2, .8, .2, 1),
        background 0.3s ease,
        border-color 0.3s ease,
        box-shadow 0.3s ease;
}
.contact-link:hover {
    transform: translateY(-4px) scale(1.04);
    background: rgba(168, 85, 247, .12);
    border-color: rgba(192, 132, 252, .4);
    box-shadow: 0 12px 30px rgba(168, 85, 247, .12);
}
.contact-link:active {
    transform: scale(0.92);
}

/* ─── ANIMATIONS ────────────────────────────────────────────────── */
@keyframes fadeUp {
    from {
        opacity: 0;
        transform: translateY(28px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}
@keyframes pulse-dot {
    0%,
    100% {
        box-shadow: 0 0 0 6px rgba(34, 197, 94, .14);
    }
    50% {
        box-shadow: 0 0 0 12px rgba(34, 197, 94, .04);
    }
}
@keyframes swipePulse {
    0%,
    100% {
        transform: translateX(0);
        opacity: 0.6;
    }
    50% {
        transform: translateX(8px);
        opacity: 1;
    }
}

/* ─── RESPONSIVE ────────────────────────────────────────────────── */
@media (max-width: 860px) {
    .portfolio-shell {
        width: min(100% - 20px, 1180px);
    }
    .about-container {
        grid-template-columns: 1fr;
        gap: 28px;
    }
    .about-meta-grid {
        grid-template-columns: 1fr;
    }
    .about-stats-grid {
        grid-template-columns: 1fr 1fr;
    }
    .about-right {
        order: -1;
    }
    .contact-section {
        grid-template-columns: 1fr;
    }
    .contact-links {
        justify-content: flex-start;
    }
    .nav-bar {
        border-radius: 24px;
        flex-wrap: wrap;
        justify-content: center;
    }
    .nav-links {
        justify-content: center;
    }
    .project-slider {
        min-height: 650px;
    }
    .project-card {
        grid-template-columns: 1fr;
        padding: 18px;
        border-radius: 28px;
        gap: 16px;
    }
    .project-card.next {
        transform: translateX(72%) scale(.78);
    }
    .project-card.previous {
        transform: translateX(-72%) scale(.78);
    }
    .tech-card-grid {
        grid-template-columns: repeat(2, minmax(0, 1fr));
    }
    .metrics-grid {
        grid-template-columns: 1fr;
    }
    .dashboard-showcase {
        grid-template-columns: repeat(5, 180px);
    }
    .swipe-hint {
        font-size: .72rem;
        padding: 8px 14px;
    }
    .slider-btn {
        width: 46px;
        height: 46px;
    }
    .slider-btn .btn-arrow {
        font-size: 1.6rem;
    }
    .about-availability {
        padding: 16px 18px 18px;
    }
    .availability-cta {
        font-size: 1rem;
    }
}

@media (max-width: 520px) {
    .project-slider {
        min-height: 710px;
    }
    .project-card {
        padding: 14px;
        border-radius: 22px;
        gap: 12px;
    }
    .project-image,
    .project-image-wrap {
        min-height: 200px;
    }
    .project-image-wrap {
        min-height: 200px;
    }
    .tech-card-grid {
        grid-template-columns: 1fr;
    }
    .dashboard-showcase {
        grid-template-columns: repeat(5, 140px);
        gap: 10px;
        padding: 12px;
    }
    .dashboard-thumb {
        min-width: 120px;
        padding: 8px;
    }
    .dashboard-thumb img {
        height: 64px;
    }
    .dashboard-thumb span {
        font-size: .7rem;
    }
    .project-dots button {
        width: 8px;
        height: 8px;
    }
    .project-dots button.active {
        width: 28px;
    }
    .slider-btn {
        width: 40px;
        height: 40px;
    }
    .slider-btn .btn-arrow {
        font-size: 1.3rem;
    }
    .nav-bar {
        padding: 10px 14px;
        border-radius: 18px;
    }
    .nav-link {
        padding: 6px 10px;
        font-size: .78rem;
    }
    .brand {
        font-size: 1rem;
    }
    .brand-mark {
        width: 30px;
        height: 30px;
        font-size: .65rem;
    }
    .about-stats-grid {
        grid-template-columns: 1fr 1fr;
        gap: 10px;
    }
    .about-stat-card {
        padding: 16px 12px;
    }
    .stat-number {
        font-size: clamp(1.8rem, 10vw, 2.6rem);
    }
    .about-name-block {
        gap: 8px;
    }
    .about-initials {
        font-size: 2.4rem;
    }
    .about-name {
        font-size: 1.8rem;
    }
    .about-meta-item {
        padding: 12px 14px;
    }
    .about-tech-tags {
        padding: 10px 12px;
    }
    .about-tech-tags span {
        font-size: 0.7rem;
        padding: 4px 10px;
    }
    .about-availability {
        padding: 14px 14px 16px;
    }
}
</style>