<template>
    <main class="portfolio-shell">
        <!-- NAV -->
        <nav class="nav-bar" ref="navBar">
            <a href="#about" class="brand" @click.prevent="scrollTo('#about')">
                <span class="brand-mark">KO</span>
                <span>Kevin<span class="brand-dot">.</span></span>
            </a>
            <div class="nav-links">
                <a v-for="item in navItems" :key="item.href"
                   :href="item.href"
                   class="nav-link"
                   :class="{ active: activeSection === item.href.slice(1) }"
                   @click.prevent="handleNavClick(item.href.slice(1))">
                    {{ item.label }}
                </a>
            </div>
        </nav>

        <!-- ====== HERO / ABOUT ====== -->
        <section id="about" class="hero-section section-reveal">
            <div class="hero-grid">
                <div class="hero-left">
                    <span class="hero-eyebrow">✦ About Me</span>
                    <h1 class="hero-title">
                        Kevin<br/>Octavius
                    </h1>
                    <p class="hero-subtitle">Software Engineer &amp; Full-Stack Developer</p>
                    <p class="hero-description">
                        I'm Kevin Octavius, a Software Engineer with a passion for building beautiful, functional mobile and web applications. I specialize in Flutter development and full-stack solutions, combining technical expertise with creative problem-solving.
                    </p>
                </div>

                <div class="hero-right">
                    <!-- Stats Grid -->
                    <div class="stats-grid">
                        <div v-for="stat in aboutStats" :key="stat.label" class="stat-item">
                            <span class="stat-value">{{ stat.value }}</span>
                            <span class="stat-label">{{ stat.label }}</span>
                        </div>
                    </div>

                    <!-- Tech Tags -->
                    <div class="tech-tags-row">
                        <span v-for="tag in aboutTechTags" :key="tag" class="tech-tag">{{ tag }}</span>
                    </div>

                    <!-- Availability -->
                    <div class="availability-card">
                        <div class="avail-status">
                            <span class="avail-dot"></span>
                            Available for work
                        </div>
                        <a href="/cv-kevin-octavius.pdf" class="resume-btn" download>
                            Download Resume →
                        </a>
                    </div>

                    <!-- Meta Info -->
                    <div class="meta-row">
                        <div v-for="(item, idx) in metaItems" :key="idx" class="meta-item">
                            <span class="meta-icon">{{ item.icon }}</span>
                            <div>
                                <strong>{{ item.title }}</strong>
                                <p>{{ item.desc }}</p>
                            </div>
                        </div>
                    </div>

                    <!-- Quote -->
                    <div class="quote-block">
                        <span class="quote-mark">"</span>
                        <p>Real artists ship.</p>
                        <span class="quote-author">— Steve Jobs</span>
                    </div>
                </div>
            </div>
        </section>

        <!-- ====== PROJECTS ====== -->
        <section id="projects" class="projects-section section-reveal">
            <div class="section-header">
                <span class="section-eyebrow">Selected Work</span>
                <h2 class="section-title">Project showcase.</h2>
            </div>

            <!-- Project Slider -->
            <div class="project-slider-wrapper" ref="sliderRef">
                <div class="project-slider"
                     @mousedown="handleDragStart"
                     @touchstart="handleDragStart"
                     @mousemove="handleDragMove"
                     @touchmove="handleDragMove"
                     @mouseup="handleDragEnd"
                     @touchend="handleDragEnd"
                     @mouseleave="handleDragEnd">

                    <article v-for="(project, index) in projects" :key="project.title"
                             class="project-card"
                             :class="[getProjectClass(index), { 'card-clicked': clickedCard === index }]"
                             :style="getCardStyle(index)"
                             @click="handleProjectClick(index)">

                        <div class="project-image-wrap">
                            <img :src="project.image" :alt="project.title" class="project-image" loading="lazy"/>
                            <div class="image-overlay"></div>
                        </div>

                        <div class="project-content">
                            <span class="project-type">{{ project.type }}</span>
                            <h3>{{ project.title }}</h3>
                            <p>{{ project.short }}</p>
                            <div class="project-tags">
                                <span v-for="tag in project.tags" :key="tag">{{ tag }}</span>
                            </div>
                        </div>
                    </article>
                </div>

                <!-- Slider Controls -->
                <div class="slider-controls">
                    <button type="button" class="slider-btn slider-btn-prev" aria-label="Previous project" @click="prevProject">
                        <span class="btn-arrow">←</span>
                    </button>
                    <div class="project-dots">
                        <button v-for="(_, index) in projects" :key="index" type="button"
                                :class="{ active: activeProject === index }"
                                @click="activeProject = index"></button>
                    </div>
                    <button type="button" class="slider-btn slider-btn-next" aria-label="Next project" @click="nextProject">
                        <span class="btn-arrow">→</span>
                    </button>
                </div>

                <!-- Dashboard Thumbnails -->
                <div class="dashboard-showcase">
                    <button v-for="(project, index) in projects" :key="`thumb-${index}`" type="button"
                            class="dashboard-thumb"
                            :class="{ active: activeProject === index }"
                            @click="selectProjectFromThumb(index)">
                        <img :src="project.image" :alt="`${project.title} preview`" loading="lazy"/>
                    </button>
                </div>
            </div>
        </section>

        <!-- ====== SKILLS ====== -->
        <section id="skills" class="skills-section section-reveal">
            <div class="section-header">
                <span class="section-eyebrow">Skills</span>
                <h2 class="section-title">Tools & technologies.</h2>
            </div>

            <div class="tech-card-grid">
                <article v-for="tech in techCards" :key="tech.name" class="tech-card premium-card" @click="handleCardClick">
                    <div class="tech-icon">{{ tech.icon }}</div>
                    <h3>{{ tech.name }}</h3>
                    <p>{{ tech.description }}</p>
                </article>
            </div>
        </section>

        <!-- ====== EXPERIENCE ====== -->
        <section id="experience" class="experience-section section-reveal">
            <div class="section-header">
                <span class="section-eyebrow">Experience</span>
                <h2 class="section-title">Where I've worked.</h2>
            </div>

            <div class="timeline">
                <div v-for="(item, idx) in experiences" :key="item.title" class="timeline-item"
                     :style="{ animationDelay: `${idx * 0.15}s` }" @click="handleCardClick">
                    <span class="timeline-dot"></span>
                    <div>
                        <h3>{{ item.title }}</h3>
                        <p>{{ item.description }}</p>
                    </div>
                </div>
            </div>
        </section>

        <!-- ====== CONTACT ====== -->
        <section id="contact" class="contact-section section-reveal">
            <div class="contact-grid">
                <div class="contact-left">
                    <span class="section-eyebrow">Contact</span>
                    <h2 class="section-title">Let's work together.</h2>
                    <p class="contact-copy">
                        Hubungi saya melalui GitHub, LinkedIn, email, atau WhatsApp. Link masih bisa kamu sesuaikan dengan akun asli milikmu.
                    </p>
                </div>

                <div class="contact-right">
                    <a v-for="social in socials" :key="`contact-${social.label}`" :href="social.href" target="_blank" rel="noreferrer" class="contact-link">
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
            </div>
        </section>

        <!-- FOOTER -->
        <footer class="site-footer">
            <p>© 2025 Kevin Octavius. Built with Vue & Vite.</p>
        </footer>

        <!-- MODAL -->
        <Transition name="project-modal">
            <section v-if="selectedProject" class="modal-backdrop" @click.self="closeProject">
                <article class="modal-card modal-enter">
                    <button type="button" class="modal-close-btn" @click="closeProject">✕</button>
                    <img :src="selectedProject.image" :alt="selectedProject.title" class="modal-image" loading="lazy"/>
                    <div class="modal-body">
                        <span class="project-type">{{ selectedProject.type }}</span>
                        <h2>{{ selectedProject.title }}</h2>
                        <p>{{ selectedProject.description }}</p>
                        <div class="project-tags modal-tags">
                            <span v-for="tag in selectedProject.tags" :key="tag">{{ tag }}</span>
                        </div>
                    </div>
                </article>
            </section>
        </Transition>
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
    { label: 'Experience', href: '#experience' },
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

// ─── SCROLL TO ─────────────────────────────────────────────────────
function scrollTo(selector) {
    const target = document.querySelector(selector);
    if (target) {
        const navHeight = navBar.value?.offsetHeight || 80;
        const offset = target.getBoundingClientRect().top + window.pageYOffset - navHeight - 20;
        window.scrollTo({ top: offset, behavior: 'smooth' });
    }
}

// ─── NAVIGATION ────────────────────────────────────────────────────
function handleNavClick(section) {
    activeSection.value = section;
    scrollTo(`#${section}`);
    setTimeout(() => updateActiveSection(), 500);
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

// ─── PROJECT SLIDER ────────────────────────────────────────────────
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
            setTimeout(() => { slider.style.transition = ''; }, 150);
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
    if (isDragging.value) return;
    
    // Reset drag intent immediately on click
    isDragIntent.value = false;
    
    // Card click animation
    clickedCard.value = index;
    setTimeout(() => { clickedCard.value = null; }, 600);
    
    // Open modal immediately without delay
    selectedProject.value = projects[index];
}

function selectProjectFromThumb(index) {
    activeProject.value = index;
    handleProjectClick(index);
}

function closeProject() {
    selectedProject.value = null;
}

// ─── CARD CLICK ANIMATION ─────────────────────────────────────────
function handleCardClick(event) {
    const el = event.currentTarget;
    el.style.transition = 'transform 0.12s cubic-bezier(.2,.8,.2,1), box-shadow 0.12s ease';
    el.style.transform = 'scale(0.97)';
    setTimeout(() => {
        el.style.transform = 'scale(1)';
        setTimeout(() => { el.style.transition = ''; }, 150);
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
    if (Math.abs(delta) > 12) isDragIntent.value = true;
    const maxDrag = 300;
    const clamped = Math.max(-maxDrag, Math.min(maxDrag, delta));
    dragOffset.value = clamped;
    dragProgress.value = Math.abs(clamped) / maxDrag;
    if (e.type === 'touchmove' && isDragIntent.value) e.preventDefault();
}

function handleDragEnd(e) {
    if (!isDragging.value) { resetDrag(); return; }
    const delta = dragOffset.value;
    if (isDragIntent.value && Math.abs(delta) > dragThreshold) {
        if (delta < 0) nextProject(); else prevProject();
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
    if (e.key === 'ArrowRight') { e.preventDefault(); nextProject(); }
    if (e.key === 'ArrowLeft') { e.preventDefault(); prevProject(); }
    if (e.key === 'Escape') { if (selectedProject.value) closeProject(); }
}

// ─── LIFECYCLE ─────────────────────────────────────────────────────
onMounted(() => {
    document.addEventListener('keydown', handleKeydown);
    window.addEventListener('scroll', updateActiveSection);
    updateActiveSection();

    const observer = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
            if (entry.isIntersecting) entry.target.classList.add('section-visible');
        });
    }, { threshold: 0.15 });

    document.querySelectorAll('.section-reveal').forEach(el => observer.observe(el));
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

const metaItems = [
    { icon: '🌍', title: 'Flexible with Timezones', desc: 'Based in Oman, available globally' },
    { icon: '🎓', title: 'Sohar University', desc: "Bachelor's in Software Engineering\n2023 – 2026" },
];

const aboutStats = [
    { value: '20+', label: 'Projects' },
    { value: '3+', label: 'Years of Experience' },
    { value: '5+', label: 'Published Platforms' },
    { value: '15+', label: 'Technical Skills' },
];

const aboutTechTags = ['Flutter', 'Full-Stack', 'Mobile', 'Web', 'IoT', 'API'];

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
</script>

<style scoped>
/* ═══════════════════════════════════════════════════════════════════
   AWRS-STYLE THEME — Minimal Dark Portfolio
   ═══════════════════════════════════════════════════════════════════ */

:global(:root) {
    --bg-primary: #0a0a0f;
    --bg-secondary: #111118;
    --bg-card: rgba(255, 255, 255, 0.03);
    --bg-card-hover: rgba(255, 255, 255, 0.06);
    --border-color: rgba(255, 255, 255, 0.06);
    --border-hover: rgba(255, 255, 255, 0.12);
    --text-primary: #f0f0f5;
    --text-secondary: #8a8a9a;
    --text-muted: #55556a;
    --accent: #7c3aed;
    --accent-light: #a78bfa;
    --accent-glow: rgba(124, 58, 237, 0.15);
}

:global(*) { box-sizing: border-box; margin: 0; padding: 0; }
:global(html) { scroll-behavior: smooth; font-size: 16px; }

:global(body) {
    min-width: 320px;
    background: var(--bg-primary);
    color: var(--text-primary);
    font-family: 'Inter', ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
    line-height: 1.6;
    overflow-x: hidden;
}

/* Subtle background grain */
.portfolio-shell::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.02'/%3E%3C/svg%3E");
    pointer-events: none;
    z-index: 0;
}

/* Subtle ambient glow */
.portfolio-shell::after {
    content: '';
    position: fixed;
    top: -40%;
    left: -20%;
    width: 80vw;
    height: 80vh;
    background: radial-gradient(ellipse, rgba(124, 58, 237, 0.06), transparent 60%);
    pointer-events: none;
    z-index: 0;
}

.portfolio-shell {
    width: min(1200px, calc(100% - 40px));
    margin: 0 auto;
    padding: 0 0 80px;
    position: relative;
    z-index: 1;
}

/* ═══════════════════ NAVIGATION ═══════════════════ */
.nav-bar {
    position: sticky;
    top: 20px;
    z-index: 100;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 12px 24px;
    border-radius: 999px;
    background: rgba(15, 15, 22, 0.8);
    backdrop-filter: blur(20px) saturate(1.2);
    border: 1px solid var(--border-color);
    margin-bottom: 60px;
}

.brand {
    display: inline-flex;
    align-items: center;
    gap: 10px;
    font-size: 1rem;
    font-weight: 700;
    letter-spacing: -0.03em;
    text-decoration: none;
    color: var(--text-primary);
}

.brand-mark {
    display: grid;
    place-items: center;
    width: 34px;
    height: 34px;
    border-radius: 10px;
    background: linear-gradient(135deg, var(--accent), #4f46e5);
    color: #fff !important;
    font-size: 0.7rem;
    font-weight: 800;
}

.brand-dot { color: var(--accent-light) !important; }

.nav-links { display: flex; gap: 4px; }

.nav-link {
    padding: 8px 16px;
    border-radius: 999px;
    font-size: 0.85rem;
    font-weight: 500;
    color: var(--text-secondary);
    text-decoration: none;
    transition: all 0.3s ease;
}

.nav-link:hover {
    color: var(--text-primary);
    background: rgba(255, 255, 255, 0.05);
}

.nav-link.active {
    color: var(--text-primary);
    background: rgba(124, 58, 237, 0.12);
}

/* ═══════════════════ SECTION REVEAL ═══════════════════ */
.section-reveal {
    opacity: 0;
    transform: translateY(40px);
    transition: opacity 0.8s cubic-bezier(0.16, 1, 0.3, 1),
                transform 0.8s cubic-bezier(0.16, 1, 0.3, 1);
}

.section-reveal.section-visible {
    opacity: 1;
    transform: translateY(0);
}

/* ═══════════════════ SECTION HEADERS ═══════════════════ */
.section-header {
    margin-bottom: 48px;
}

.section-eyebrow {
    display: block;
    font-size: 0.7rem;
    font-weight: 700;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    color: var(--accent-light);
    margin-bottom: 12px;
}

.section-title {
    font-size: clamp(2rem, 4vw, 3rem);
    font-weight: 800;
    letter-spacing: -0.05em;
    line-height: 1.1;
    color: var(--text-primary);
}

/* ═══════════════════ HERO SECTION ═══════════════════ */
.hero-section {
    padding: 40px 0 80px;
}

.hero-grid {
    display: grid;
    grid-template-columns: 1.3fr 1fr;
    gap: 60px;
    align-items: start;
}

/* Left side */
.hero-eyebrow {
    font-size: 0.75rem;
    font-weight: 700;
    letter-spacing: 0.18em;
    text-transform: uppercase;
    color: var(--accent-light);
    display: block;
    margin-bottom: 20px;
}

.hero-title {
    font-size: clamp(3rem, 6vw, 5.5rem);
    font-weight: 900;
    letter-spacing: -0.06em;
    line-height: 0.95;
    color: var(--text-primary);
    margin-bottom: 16px;
}

.hero-subtitle {
    font-size: clamp(1rem, 2vw, 1.3rem);
    font-weight: 500;
    color: var(--text-secondary);
    margin-bottom: 24px;
}

.hero-description {
    font-size: 0.95rem;
    line-height: 1.8;
    color: var(--text-secondary);
    max-width: 520px;
}

/* Right side */
.hero-right {
    display: flex;
    flex-direction: column;
    gap: 24px;
}

/* Stats Grid */
.stats-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 12px;
}

.stat-item {
    padding: 20px;
    border-radius: 16px;
    background: var(--bg-card);
    border: 1px solid var(--border-color);
    transition: all 0.3s ease;
}

.stat-item:hover {
    background: var(--bg-card-hover);
    border-color: var(--border-hover);
    transform: translateY(-2px);
}

.stat-value {
    display: block;
    font-size: clamp(1.8rem, 3vw, 2.4rem);
    font-weight: 900;
    letter-spacing: -0.04em;
    color: var(--text-primary);
    line-height: 1;
}

.stat-label {
    display: block;
    font-size: 0.75rem;
    font-weight: 600;
    color: var(--text-muted);
    letter-spacing: 0.04em;
    text-transform: uppercase;
    margin-top: 6px;
}

/* Tech Tags */
.tech-tags-row {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
}

.tech-tag {
    padding: 6px 14px;
    border-radius: 999px;
    font-size: 0.75rem;
    font-weight: 600;
    color: var(--text-secondary);
    background: var(--bg-card);
    border: 1px solid var(--border-color);
    transition: all 0.3s ease;
}

.tech-tag:hover {
    color: var(--accent-light);
    border-color: rgba(124, 58, 237, 0.3);
    background: var(--accent-glow);
}

/* Availability Card */
.availability-card {
    padding: 20px;
    border-radius: 16px;
    background: linear-gradient(135deg, rgba(34, 197, 94, 0.08), var(--bg-card));
    border: 1px solid rgba(34, 197, 94, 0.12);
}

.avail-status {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    font-size: 0.7rem;
    font-weight: 800;
    letter-spacing: 0.14em;
    color: #22c55e;
    text-transform: uppercase;
}

.avail-dot {
    width: 6px;
    height: 6px;
    border-radius: 50%;
    background: #22c55e;
    box-shadow: 0 0 0 4px rgba(34, 197, 94, 0.15);
    animation: pulse-dot 2s ease-in-out infinite;
}

.resume-btn {
    display: inline-block;
    margin-top: 12px;
    padding: 10px 20px;
    border-radius: 999px;
    font-size: 0.8rem;
    font-weight: 700;
    color: var(--text-primary);
    background: rgba(255, 255, 255, 0.06);
    border: 1px solid var(--border-color);
    text-decoration: none;
    transition: all 0.3s ease;
}

.resume-btn:hover {
    background: rgba(255, 255, 255, 0.1);
    border-color: var(--border-hover);
    transform: translateY(-2px);
}

/* Meta Row */
.meta-row {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 12px;
}

.meta-item {
    display: flex;
    align-items: center;
    gap: 12px;
    padding: 16px;
    border-radius: 14px;
    background: var(--bg-card);
    border: 1px solid var(--border-color);
    transition: all 0.3s ease;
}

.meta-item:hover {
    background: var(--bg-card-hover);
    transform: translateY(-2px);
}

.meta-icon { font-size: 1.3rem; flex-shrink: 0; }

.meta-item strong {
    display: block;
    font-size: 0.75rem;
    font-weight: 700;
    color: var(--text-primary);
    letter-spacing: 0.02em;
}

.meta-item p {
    font-size: 0.78rem;
    color: var(--text-muted);
    margin-top: 2px;
    line-height: 1.4;
}

/* Quote */
.quote-block {
    padding: 20px;
    border-radius: 16px;
    background: var(--bg-card);
    border: 1px solid var(--border-color);
    display: flex;
    align-items: baseline;
    gap: 8px;
}

.quote-mark {
    font-size: 2rem;
    font-weight: 900;
    color: var(--accent);
    opacity: 0.4;
    line-height: 1;
}

.quote-block p {
    font-size: 0.95rem;
    font-weight: 600;
    color: var(--text-primary);
    margin: 0;
}

.quote-author {
    font-size: 0.7rem;
    font-weight: 700;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    color: var(--text-muted);
}

/* ═══════════════════ PROJECTS SECTION ═══════════════════ */
.projects-section { padding: 80px 0; }

.project-slider-wrapper { position: relative; }

.project-slider {
    position: relative;
    min-height: 520px;
    perspective: 1400px;
    overflow: hidden;
    touch-action: pan-y;
    cursor: grab;
}

.project-slider:active { cursor: grabbing; }

.project-card {
    position: absolute;
    inset: 0;
    display: grid;
    grid-template-columns: minmax(0, 1fr) minmax(280px, 0.75fr);
    gap: 32px;
    align-items: center;
    padding: 32px;
    border-radius: 24px;
    background: var(--bg-card);
    border: 1px solid var(--border-color);
    backdrop-filter: blur(20px);
    opacity: 0;
    pointer-events: none;
    transform: translateX(0) scale(0.86);
    transition: opacity 0.5s cubic-bezier(0.16, 1, 0.3, 1),
                transform 0.5s cubic-bezier(0.16, 1, 0.3, 1),
                filter 0.5s ease;
    will-change: transform, opacity;
}

.project-card.active {
    z-index: 3;
    opacity: 1;
    pointer-events: auto;
    transform: translateX(0) scale(1);
}

.project-card.next {
    z-index: 2;
    opacity: 0.4;
    transform: translateX(58%) scale(0.82) rotateY(-12deg);
    filter: blur(0.8px);
}

.project-card.previous {
    z-index: 1;
    opacity: 0.24;
    transform: translateX(-58%) scale(0.82) rotateY(12deg);
    filter: blur(0.8px);
}

.project-card.hidden-card {
    transform: translateX(120%) scale(0.74);
    opacity: 0;
    pointer-events: none;
}

/* Image */
.project-image-wrap {
    position: relative;
    overflow: hidden;
    min-height: 380px;
    border-radius: 16px;
    background: linear-gradient(135deg, rgba(124, 58, 237, 0.1), rgba(79, 70, 229, 0.1));
}

.project-image {
    width: 100%;
    height: 100%;
    min-height: 380px;
    object-fit: cover;
    display: block;
    transition: transform 0.6s cubic-bezier(0.16, 1, 0.3, 1);
}

.project-card.active:hover .project-image {
    transform: scale(1.05);
}

.image-overlay {
    position: absolute;
    inset: 0;
    background: linear-gradient(180deg, transparent 40%, rgba(10, 10, 15, 0.6));
}

/* Content */
.project-type {
    display: inline-block;
    font-size: 0.7rem;
    font-weight: 800;
    letter-spacing: 0.14em;
    text-transform: uppercase;
    color: var(--accent-light);
    margin-bottom: 8px;
}

.project-content h3 {
    font-size: clamp(1.6rem, 3vw, 2.5rem);
    font-weight: 800;
    letter-spacing: -0.04em;
    line-height: 1.1;
    color: var(--text-primary);
    margin-bottom: 12px;
}

.project-content p {
    font-size: 0.9rem;
    line-height: 1.7;
    color: var(--text-secondary);
    margin-bottom: 16px;
}

.project-tags { display: flex; flex-wrap: wrap; gap: 8px; }

.project-tags span {
    padding: 5px 12px;
    border-radius: 999px;
    font-size: 0.72rem;
    font-weight: 600;
    color: var(--text-secondary);
    background: rgba(255, 255, 255, 0.04);
    border: 1px solid var(--border-color);
}

/* Slider Controls */
.slider-controls {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 20px;
    margin-top: 32px;
}

.slider-btn {
    width: 48px;
    height: 48px;
    border-radius: 50%;
    background: var(--bg-card);
    border: 1px solid var(--border-color);
    color: var(--text-primary);
    cursor: pointer;
    display: grid;
    place-items: center;
    font-size: 1.2rem;
    transition: all 0.3s ease;
}

.slider-btn:hover {
    background: var(--bg-card-hover);
    border-color: var(--border-hover);
    transform: scale(1.08);
}

.slider-btn:active { transform: scale(0.95); }

.project-dots { display: flex; gap: 8px; }

.project-dots button {
    width: 8px;
    height: 8px;
    border-radius: 999px;
    background: rgba(255, 255, 255, 0.15);
    border: none;
    cursor: pointer;
    transition: all 0.4s cubic-bezier(0.16, 1, 0.3, 1);
}

.project-dots button:hover { background: rgba(255, 255, 255, 0.3); }

.project-dots button.active {
    width: 32px;
    background: var(--accent);
}

/* Dashboard Thumbnails */
.dashboard-showcase {
    display: grid;
    grid-template-columns: repeat(5, minmax(160px, 1fr));
    gap: 12px;
    margin-top: 24px;
    padding: 16px;
    border-radius: 20px;
    background: var(--bg-card);
    border: 1px solid var(--border-color);
}

.dashboard-thumb {
    min-width: 160px;
    border: none;
    border-radius: 14px;
    padding: 8px;
    background: transparent;
    cursor: pointer;
    transition: all 0.3s ease;
}

.dashboard-thumb:hover { transform: translateY(-4px); }

.dashboard-thumb.active {
    transform: translateY(-4px);
}

.dashboard-thumb img {
    width: 100%;
    height: 80px;
    display: block;
    object-fit: cover;
    border-radius: 10px;
    transition: all 0.3s ease;
}

/* ═══════════════════ MODAL ═══════════════════ */
.modal-backdrop {
    position: fixed;
    inset: 0;
    z-index: 200;
    display: grid;
    place-items: center;
    padding: 24px;
    background: rgba(5, 5, 10, 0.8);
    backdrop-filter: blur(20px);
}

.modal-card {
    width: min(900px, 100%);
    max-height: min(86vh, 800px);
    overflow-y: auto;
    border-radius: 24px;
    background: var(--bg-secondary);
    border: 1px solid var(--border-color);
}

.modal-close-btn {
    position: absolute;
    top: 16px;
    right: 16px;
    width: 36px;
    height: 36px;
    border-radius: 50%;
    background: rgba(255, 255, 255, 0.08);
    border: 1px solid var(--border-color);
    color: var(--text-primary);
    font-size: 1rem;
    cursor: pointer;
    display: grid;
    place-items: center;
    transition: all 0.3s ease;
}

.modal-close-btn:hover { background: rgba(255, 255, 255, 0.14); }

.modal-image {
    width: calc(100% - 32px);
    max-height: 400px;
    margin: 16px;
    border-radius: 16px;
    object-fit: cover;
}

.modal-body { padding: 0 28px 32px; }

.modal-tags { margin-top: 20px; }

.project-modal-enter-active,
.project-modal-leave-active { transition: opacity 0.3s ease; }

.project-modal-enter-from,
.project-modal-leave-to { opacity: 0; }

.project-modal-enter-active .modal-card {
    animation: modalEnter 0.45s cubic-bezier(0.16, 1, 0.3, 1) both;
}

@keyframes modalEnter {
    from { opacity: 0; transform: scale(0.92) translateY(20px); }
    to { opacity: 1; transform: scale(1) translateY(0); }
}

/* ═══════════════════ SKILLS SECTION ═══════════════════ */
.skills-section { padding: 80px 0; }

.tech-card-grid {
    display: grid;
    grid-template-columns: repeat(4, minmax(0, 1fr));
    gap: 14px;
}

.tech-card {
    padding: 24px;
    border-radius: 18px;
    background: var(--bg-card);
    border: 1px solid var(--border-color);
    transition: all 0.35s cubic-bezier(0.16, 1, 0.3, 1);
    cursor: pointer;
}

.tech-card:hover {
    background: var(--bg-card-hover);
    border-color: var(--border-hover);
    transform: translateY(-4px);
}

.tech-icon {
    display: grid;
    place-items: center;
    width: 48px;
    height: 48px;
    margin-bottom: 16px;
    border-radius: 12px;
    background: rgba(124, 58, 237, 0.1);
    color: var(--accent-light);
    font-weight: 900;
    font-size: 0.8rem;
}

.tech-card h3 {
    font-size: 1rem;
    font-weight: 700;
    color: var(--text-primary);
    margin-bottom: 8px;
}

.tech-card p {
    font-size: 0.85rem;
    line-height: 1.6;
    color: var(--text-secondary);
}

/* ═══════════════════ EXPERIENCE SECTION ═══════════════════ */
.experience-section { padding: 80px 0; }

.timeline { display: grid; gap: 14px; }

.timeline-item {
    display: grid;
    grid-template-columns: auto 1fr;
    gap: 16px;
    padding: 24px;
    border-radius: 18px;
    background: var(--bg-card);
    border: 1px solid var(--border-color);
    opacity: 0;
    transform: translateX(-20px);
    animation: slideInItem 0.5s cubic-bezier(0.16, 1, 0.3, 1) forwards;
}

@keyframes slideInItem {
    to { opacity: 1; transform: translateX(0); }
}

.timeline-item:hover {
    background: var(--bg-card-hover);
    border-color: var(--border-hover);
    transform: translateX(4px);
}

.timeline-dot {
    width: 12px;
    height: 12px;
    margin-top: 5px;
    border-radius: 50%;
    background: var(--accent);
    box-shadow: 0 0 0 6px rgba(124, 58, 237, 0.12);
}

.timeline-item h3 {
    font-size: 1rem;
    font-weight: 700;
    color: var(--text-primary);
    margin-bottom: 6px;
}

.timeline-item p {
    font-size: 0.88rem;
    line-height: 1.7;
    color: var(--text-secondary);
}

/* ═══════════════════ CONTACT SECTION ═══════════════════ */
.contact-section { padding: 80px 0; }

.contact-grid {
    display: grid;
    grid-template-columns: 1.2fr 1fr;
    gap: 60px;
    align-items: start;
}

.contact-copy {
    font-size: 0.95rem;
    line-height: 1.8;
    color: var(--text-secondary);
    max-width: 480px;
}

.contact-right { display: flex; flex-direction: column; gap: 12px; }

.contact-link {
    display: inline-flex;
    align-items: center;
    gap: 12px;
    padding: 16px 24px;
    border-radius: 14px;
    background: var(--bg-card);
    border: 1px solid var(--border-color);
    color: var(--text-primary);
    text-decoration: none;
    font-size: 0.9rem;
    font-weight: 600;
    transition: all 0.3s ease;
}

.contact-link:hover {
    background: var(--bg-card-hover);
    border-color: var(--border-hover);
    transform: translateX(4px);
}

.social-icon {
    display: inline-grid;
    place-items: center;
    width: 20px;
    height: 20px;
    color: var(--accent-light);
}

.social-icon :deep(svg) { width: 20px; height: 20px; }

/* ═══════════════════ FOOTER ═══════════════════ */
.site-footer {
    text-align: center;
    padding: 40px 0;
    border-top: 1px solid var(--border-color);
    margin-top: 40px;
}

.site-footer p {
    font-size: 0.8rem;
    color: var(--text-muted);
}

/* ═══════════════════ ANIMATIONS ═══════════════════ */
@keyframes pulse-dot {
    0%, 100% { box-shadow: 0 0 0 4px rgba(34, 197, 94, 0.15); }
    50% { box-shadow: 0 0 0 8px rgba(34, 197, 94, 0.06); }
}

/* ═══════════════════ RESPONSIVE ═══════════════════ */
@media (max-width: 900px) {
    .hero-grid { grid-template-columns: 1fr; gap: 40px; }
    .hero-right { order: -1; }
    .contact-grid { grid-template-columns: 1fr; gap: 32px; }

    .project-card {
        grid-template-columns: 1fr;
        padding: 20px;
    }

    .project-image,
    .project-image-wrap { min-height: 240px; }

    .tech-card-grid { grid-template-columns: repeat(2, minmax(0, 1fr)); }

    .dashboard-showcase { grid-template-columns: repeat(5, 140px); }

    .nav-bar {
        border-radius: 20px;
        flex-wrap: wrap;
        justify-content: center;
        gap: 8px;
    }

    .meta-row { grid-template-columns: 1fr; }
}

@media (max-width: 560px) {
    .portfolio-shell { width: min(100% - 20px); padding: 0 0 40px; }
    .nav-bar { margin-bottom: 32px; padding: 10px 16px; }
    .nav-link { padding: 6px 10px; font-size: 0.78rem; }

    .hero-title { font-size: clamp(2.4rem, 8vw, 3.5rem); }
    .section-title { font-size: clamp(1.6rem, 5vw, 2.2rem); }

    .project-slider { min-height: 600px; }
    .tech-card-grid { grid-template-columns: 1fr; }
    .dashboard-showcase { grid-template-columns: repeat(5, 120px); gap: 8px; padding: 10px; }
    .stats-grid { grid-template-columns: 1fr 1fr; }

    .slider-btn { width: 40px; height: 40px; font-size: 1rem; }
}
</style>
