<template>
    <main class="portfolio-shell">
        <!-- NAV -->
        <nav class="nav-bar" ref="navBar">
            <div class="nav-brand">
                <span class="brand-mark">KO</span>
                <span class="brand-name">Kevin Octavius</span>
            </div>
            
            <!-- Language Toggle -->
            <div class="lang-toggle">
                <button 
                    @click="setLanguage('id')" 
                    class="lang-btn"
                    :class="{ active: currentLanguage === 'id' }"
                >
                    ID
                </button>
                <button 
                    @click="setLanguage('en')" 
                    class="lang-btn"
                    :class="{ active: currentLanguage === 'en' }"
                >
                    EN
                </button>
            </div>

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
                    <span class="hero-eyebrow">✦ {{ t('about.eyebrow') }}</span>
                    <h1 class="hero-title">
                        <img 
                            :src="getImagePath('/images/profile/profile.jpg')" 
                            alt="Kevin Octavius" 
                            class="hero-profile-img"
                            @error="handleProfileError"
                        />
                        Kevin<br/>Octavius
                    </h1>
                    <p class="hero-subtitle">{{ t('about.subtitle') }}</p>
                    <p class="hero-description">
                        {{ t('about.description') }}
                    </p>
                </div>

                <div class="hero-right">
                    <div class="stats-grid">
                        <div v-for="stat in aboutStats" :key="stat.label" class="stat-item">
                            <span class="stat-value">{{ stat.value }}</span>
                            <span class="stat-label">{{ stat.label }}</span>
                        </div>
                    </div>

                    <div class="tech-tags-row">
                        <span v-for="tag in aboutTechTags" :key="tag" class="tech-tag">{{ tag }}</span>
                    </div>

                    <div class="availability-card">
                        <div class="avail-status">
                            <span class="avail-dot"></span>
                            {{ t('about.available') }}
                        </div>
                    <a :href="getImagePath('/document/CV_Kevin.pdf')" class="resume-btn" download>
                        {{ t('about.download') }} →
                    </a>
                    </div>

                    <div class="meta-row">
                        <div v-for="(item, idx) in metaItems" :key="idx" class="meta-item">
                            <span class="meta-icon">{{ item.icon }}</span>
                            <div>
                                <strong>{{ item.title }}</strong>
                                <p>{{ item.desc }}</p>
                            </div>
                        </div>
                    </div>

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
                <span class="section-eyebrow">{{ t('projects.eyebrow') }}</span>
                <h2 class="section-title">{{ t('projects.title') }}</h2>
            </div>

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
                            <img 
                                :src="project.image" 
                                :alt="project.title" 
                                class="project-image" 
                                loading="lazy"
                                @error="handleImageError"
                            />
                            <div class="image-overlay"></div>
                        </div>

                        <div class="project-content">
                            <span class="project-type">{{ getProjectType(project.type) }}</span>
                            <h3>{{ project.title }}</h3>
                            <p>{{ getProjectShort(project.short) }}</p>
                            <div class="project-tags">
                                <span v-for="tag in getProjectTags(project.tags)" :key="tag">{{ tag }}</span>
                            </div>
                        </div>
                    </article>
                </div>

                <div class="slider-controls">
                    <button type="button" class="slider-btn slider-btn-prev" aria-label="Previous project" @click="prevProject">
                        <span class="btn-arrow">←</span>
                    </button>
                    <div class="project-dots">
                        <button v-for="(_, index) in projects" :key="index" type="button"
                                :class="{ active: activeProject === index }"
                                @click="selectProjectFromThumb(index)"></button>
                    </div>
                    <button type="button" class="slider-btn slider-btn-next" aria-label="Next project" @click="nextProject">
                        <span class="btn-arrow">→</span>
                    </button>
                </div>

                <div class="dashboard-showcase">
                    <button v-for="(project, index) in projects" :key="`thumb-${index}`" type="button"
                            class="dashboard-thumb"
                            :class="{ active: activeProject === index }"
                            @click="selectProjectFromThumb(index)">
                        <img 
                            :src="project.image" 
                            :alt="`${project.title} preview`" 
                            loading="lazy"
                            @error="handleImageError"
                        />
                    </button>
                </div>
            </div>
        </section>

        <!-- ====== SKILLS ====== -->
        <section id="skills" class="skills-section section-reveal">
            <div class="section-header">
                <span class="section-eyebrow">{{ t('skills.eyebrow') }}</span>
                <h2 class="section-title">{{ t('skills.title') }}</h2>
            </div>

            <div class="tech-card-grid">
                <article v-for="tech in techCards" :key="tech.name" class="tech-card premium-card" @click="handleCardClick">
                    <div class="tech-icon">{{ tech.icon }}</div>
                    <h3>{{ tech.name }}</h3>
                    <p>{{ getTechDescription(tech.description) }}</p>
                </article>
            </div>
        </section>

        <!-- ====== CONTACT ====== -->
        <section id="contact" class="contact-section section-reveal">
            <div class="contact-grid">
                <div class="contact-left">
                    <span class="section-eyebrow">{{ t('contact.eyebrow') }}</span>
                    <h2 class="section-title">{{ t('contact.title') }}</h2>
                    <p class="contact-copy">
                        {{ t('contact.copy') }}
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

        <footer class="site-footer">
            <p>© 2025 Kevin Octavius. {{ t('footer.built') }}</p>
        </footer>

        <!-- MODAL -->
        <Teleport to="body">
            <Transition name="project-modal">
                <div v-if="selectedProject" class="modal-backdrop" @click.self="closeProject">
                    <article class="modal-card">
                        <button type="button" class="modal-close-btn" @click="closeProject">✕</button>

                        <div class="modal-slider">
                            <Transition name="slide-fade" mode="out-in">
                                <img 
                                    :key="modalImageIndex"
                                    :src="(selectedProject.subImages[modalImageIndex] || selectedProject.mainImage)"
                                    :alt="`${selectedProject.title} — View ${modalImageIndex + 1}`"
                                    class="modal-image" 
                                    loading="lazy"
                                    @error="handleImageError"
                                />
                            </Transition>

                            <button type="button" class="modal-slider-btn modal-slider-prev" @click="prevModalImage">‹</button>
                            <button type="button" class="modal-slider-btn modal-slider-next" @click="nextModalImage">›</button>

                            <div class="modal-dots">
                                <button v-for="(_, i) in selectedProject.subImages.length + 1" :key="i" type="button"
                                        :class="{ active: modalImageIndex === i }"
                                        @click="modalImageIndex = i"></button>
                            </div>

                            <div class="modal-thumbnails">
                                <button type="button" :class="{ active: modalImageIndex === 0 }" @click="modalImageIndex = 0">
                                    <img :src="selectedProject.mainImage" alt="Main" @error="handleImageError"/>
                                </button>
                                <button v-for="(sub, i) in selectedProject.subImages" :key="i" type="button"
                                        :class="{ active: modalImageIndex === i + 1 }" @click="modalImageIndex = i + 1">
                                    <img :src="sub" alt="Sub" @error="handleImageError"/>
                                </button>
                            </div>
                        </div>

                        <div class="modal-body">
                            <span class="project-type">{{ getProjectType(selectedProject.type) }}</span>
                            <h2>{{ selectedProject.title }}</h2>
                            <p>{{ getProjectDescription(selectedProject.description) }}</p>
                            <div class="project-tags modal-tags">
                                <span v-for="tag in getProjectTags(selectedProject.tags)" :key="tag">{{ tag }}</span>
                            </div>
                        </div>
                    </article>
                </div>
            </Transition>
        </Teleport>
    </main>
</template>

<script setup>
import { computed, ref, onMounted, onUnmounted } from 'vue';

// ─── LANGUAGE SYSTEM ──────────────────────────────────────────────
const currentLanguage = ref('id');

const translations = {
    id: {
        about: {
            eyebrow: 'Tentang Saya',
            subtitle: 'Software Engineer & Full-Stack Developer',
            description: 'Saya Kevin Octavius, seorang Software Engineer dengan hasrat membangun aplikasi mobile dan web yang indah dan fungsional. Saya berspesialisasi dalam pengembangan Flutter dan solusi full-stack, menggabungkan keahlian teknis dengan pemecahan masalah kreatif.',
            available: 'Tersedia untuk bekerja',
            download: 'Download CV'
        },
        projects: {
            eyebrow: 'Karya Terpilih',
            title: 'Showcase proyek.'
        },
        skills: {
            eyebrow: 'Keahlian',
            title: 'Tools & teknologi.'
        },
        contact: {
            eyebrow: 'Kontak',
            title: 'Mari bekerja sama.',
            copy: 'Hubungi saya melalui GitHub, LinkedIn, email, atau WhatsApp.'
        },
        footer: {
            built: 'Dibangun dengan Vue & Vite.'
        },
        projectTypes: {
            'Web Dashboard': 'Dashboard Web',
            'IoT System': 'Sistem IoT',
            'IoT Dashboard': 'Dashboard IoT',
            'Mobile App': 'Aplikasi Mobile',
            'Personal Website': 'Website Pribadi'
        },
        projectShorts: {
            'Dashboard monitoring real-time dengan visual data yang ringkas dan mudah ditindaklanjuti.': 'Dashboard monitoring real-time dengan visual data yang ringkas dan mudah ditindaklanjuti.',
            'Sistem monitoring KWH berbasis MQTT untuk membaca data listrik secara real-time.': 'Sistem monitoring KWH berbasis MQTT untuk membaca data listrik secara real-time.',
            'Dashboard monitoring ECU untuk memantau data perangkat secara real-time.': 'Dashboard monitoring ECU untuk memantau data perangkat secara real-time.',
            'Aplikasi Flutter untuk membantu user menemukan layanan handyman dan melakukan booking jasa.': 'Aplikasi Flutter untuk membantu user menemukan layanan handyman dan melakukan booking jasa.',
            'Website portfolio modern untuk menampilkan profil, skill, project, CV, dan kontak profesional.': 'Website portfolio modern untuk menampilkan profil, skill, project, CV, dan kontak profesional.'
        },
        projectDescriptions: {
            'DMS Monitoring Dashboard menampilkan data operasional secara real-time dengan tampilan ringkas, status perangkat, dan insight yang mudah dibaca oleh user.': 'DMS Monitoring Dashboard menampilkan data operasional secara real-time dengan tampilan ringkas, status perangkat, dan insight yang mudah dibaca oleh user.',
            'Project ini menghubungkan perangkat KWH dengan MQTT untuk membaca data listrik, menampilkan grafik, dan membantu proses analisis konsumsi daya.': 'Project ini menghubungkan perangkat KWH dengan MQTT untuk membaca data listrik, menampilkan grafik, dan membantu proses analisis konsumsi daya.',
            'ECU Monitoring Dashboard menampilkan data operasional perangkat secara real-time dengan tampilan yang ringkas dan mudah dipahami.': 'ECU Monitoring Dashboard menampilkan data operasional perangkat secara real-time dengan tampilan yang ringkas dan mudah dipahami.',
            'MyHandyman adalah aplikasi mobile yang membantu user mencari layanan handyman, melihat detail jasa, dan melakukan proses booking dengan UI yang sederhana.': 'MyHandyman adalah aplikasi mobile yang membantu user mencari layanan handyman, melihat detail jasa, dan melakukan proses booking dengan UI yang sederhana.',
            'Portfolio ini dibuat dengan Vue dan Vite sebagai tempat menampilkan profile, skill, experience, project, CV, dan kontak profesional.': 'Portfolio ini dibuat dengan Vue dan Vite sebagai tempat menampilkan profile, skill, experience, project, CV, dan kontak profesional.'
        },
        projectTags: {
            'Vue': 'Vue',
            'Node.js': 'Node.js',
            'MySQL': 'MySQL',
            'MQTT': 'MQTT',
            'IoT': 'IoT',
            'Dashboard': 'Dashboard',
            'Flutter': 'Flutter',
            'Mobile': 'Mobile',
            'UI/UX': 'UI/UX',
            'Vite': 'Vite',
            'CSS Animation': 'Animasi CSS'
        },
        techDescriptions: {
            'Membangun logic frontend dan backend yang dinamis, clean, dan mudah dikembangkan.': 'Membangun logic frontend dan backend yang dinamis, clean, dan mudah dikembangkan.',
            'Membuat interface interaktif, ringan, dan nyaman digunakan untuk dashboard modern.': 'Membuat interface interaktif, ringan, dan nyaman digunakan untuk dashboard modern.',
            'Mengembangkan API, automation service, dan integrasi data untuk kebutuhan operasional.': 'Mengembangkan API, automation service, dan integrasi data untuk kebutuhan operasional.',
            'Membangun aplikasi mobile dengan UI konsisten dan pengalaman pengguna yang smooth.': 'Membangun aplikasi mobile dengan UI konsisten dan pengalaman pengguna yang smooth.',
            'Merancang struktur data dan query untuk kebutuhan aplikasi serta reporting.': 'Merancang struktur data dan query untuk kebutuhan aplikasi serta reporting.',
            'Menghubungkan device monitoring secara real-time untuk sistem berbasis IoT.': 'Menghubungkan device monitoring secara real-time untuk sistem berbasis IoT.',
            'Menyiapkan environment yang konsisten untuk development dan deployment.': 'Menyiapkan environment yang konsisten untuk development dan deployment.',
            'Mengelola version control dan workflow kolaborasi secara rapi.': 'Mengelola version control dan workflow kolaborasi secara rapi.'
        }
    },
    en: {
        about: {
            eyebrow: 'About Me',
            subtitle: 'Software Engineer & Full-Stack Developer',
            description: 'I\'m Kevin Octavius, a Software Engineer with a passion for building beautiful, functional mobile and web applications. I specialize in Flutter development and full-stack solutions, combining technical expertise with creative problem-solving.',
            available: 'Available for work',
            download: 'Download Resume'
        },
        projects: {
            eyebrow: 'Selected Work',
            title: 'Project showcase.'
        },
        skills: {
            eyebrow: 'Skills',
            title: 'Tools & technologies.'
        },
        contact: {
            eyebrow: 'Contact',
            title: 'Let\'s work together.',
            copy: 'Contact me via GitHub, LinkedIn, email, or WhatsApp.'
        },
        footer: {
            built: 'Built with Vue & Vite.'
        },
        projectTypes: {
            'Web Dashboard': 'Web Dashboard',
            'IoT System': 'IoT System',
            'IoT Dashboard': 'IoT Dashboard',
            'Mobile App': 'Mobile App',
            'Personal Website': 'Personal Website'
        },
        projectShorts: {
            'Dashboard monitoring real-time dengan visual data yang ringkas dan mudah ditindaklanjuti.': 'Real-time monitoring dashboard with concise and actionable data visualization.',
            'Sistem monitoring KWH berbasis MQTT untuk membaca data listrik secara real-time.': 'MQTT-based KWH monitoring system for real-time electricity data reading.',
            'Dashboard monitoring ECU untuk memantau data perangkat secara real-time.': 'ECU monitoring dashboard for real-time device data monitoring.',
            'Aplikasi Flutter untuk membantu user menemukan layanan handyman dan melakukan booking jasa.': 'Flutter app to help users find handyman services and book appointments.',
            'Website portfolio modern untuk menampilkan profil, skill, project, CV, dan kontak profesional.': 'Modern portfolio website to showcase profile, skills, projects, CV, and professional contacts.'
        },
        projectDescriptions: {
            'DMS Monitoring Dashboard menampilkan data operasional secara real-time dengan tampilan ringkas, status perangkat, dan insight yang mudah dibaca oleh user.': 'DMS Monitoring Dashboard displays operational data in real-time with a concise layout, device status, and easy-to-read insights for users.',
            'Project ini menghubungkan perangkat KWH dengan MQTT untuk membaca data listrik, menampilkan grafik, dan membantu proses analisis konsumsi daya.': 'This project connects KWH devices with MQTT to read electricity data, display graphs, and assist in power consumption analysis.',
            'ECU Monitoring Dashboard menampilkan data operasional perangkat secara real-time dengan tampilan yang ringkas dan mudah dipahami.': 'ECU Monitoring Dashboard displays device operational data in real-time with a concise and easy-to-understand layout.',
            'MyHandyman adalah aplikasi mobile yang membantu user mencari layanan handyman, melihat detail jasa, dan melakukan proses booking dengan UI yang sederhana.': 'MyHandyman is a mobile app that helps users find handyman services, view service details, and book appointments with a simple UI.',
            'Portfolio ini dibuat dengan Vue dan Vite sebagai tempat menampilkan profile, skill, experience, project, CV, dan kontak profesional.': 'This portfolio is built with Vue and Vite to showcase profile, skills, experience, projects, CV, and professional contacts.'
        },
        projectTags: {
            'Vue': 'Vue',
            'Node.js': 'Node.js',
            'MySQL': 'MySQL',
            'MQTT': 'MQTT',
            'IoT': 'IoT',
            'Dashboard': 'Dashboard',
            'Flutter': 'Flutter',
            'Mobile': 'Mobile',
            'UI/UX': 'UI/UX',
            'Vite': 'Vite',
            'CSS Animation': 'CSS Animation'
        },
        techDescriptions: {
            'Membangun logic frontend dan backend yang dinamis, clean, dan mudah dikembangkan.': 'Building dynamic, clean, and maintainable frontend and backend logic.',
            'Membuat interface interaktif, ringan, dan nyaman digunakan untuk dashboard modern.': 'Creating interactive, lightweight, and user-friendly interfaces for modern dashboards.',
            'Mengembangkan API, automation service, dan integrasi data untuk kebutuhan operasional.': 'Developing APIs, automation services, and data integration for operational needs.',
            'Membangun aplikasi mobile dengan UI konsisten dan pengalaman pengguna yang smooth.': 'Building mobile apps with consistent UI and smooth user experience.',
            'Merancang struktur data dan query untuk kebutuhan aplikasi serta reporting.': 'Designing data structures and queries for application needs and reporting.',
            'Menghubungkan device monitoring secara real-time untuk sistem berbasis IoT.': 'Connecting real-time device monitoring for IoT-based systems.',
            'Menyiapkan environment yang konsisten untuk development dan deployment.': 'Setting up consistent environments for development and deployment.',
            'Mengelola version control dan workflow kolaborasi secara rapi.': 'Managing version control and collaboration workflow efficiently.'
        }
    }
};

function t(path) {
    const keys = path.split('.');
    let result = translations[currentLanguage.value];
    for (const key of keys) {
        if (result && result[key] !== undefined) {
            result = result[key];
        } else {
            return path;
        }
    }
    return result;
}

function getProjectType(type) {
    return t('projectTypes')[type] || type;
}

function getProjectShort(short) {
    return t('projectShorts')[short] || short;
}

function getProjectDescription(desc) {
    return t('projectDescriptions')[desc] || desc;
}

function getProjectTags(tags) {
    return tags.map(tag => t('projectTags')[tag] || tag);
}

function getTechDescription(desc) {
    return t('techDescriptions')[desc] || desc;
}

function setLanguage(lang) {
    currentLanguage.value = lang;
    localStorage.setItem('preferred-language', lang);
}

// Load saved language
onMounted(() => {
    const savedLang = localStorage.getItem('preferred-language');
    if (savedLang && (savedLang === 'id' || savedLang === 'en')) {
        currentLanguage.value = savedLang;
    }
});

// ─── BASE URL ──────────────────────────────────────────────────────
const BASE_URL = '/portfolio';

function getImagePath(path) {
    return `${BASE_URL}${path}`;
}

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

// ─── IMAGE ERROR HANDLING ──────────────────────────────────────────
function handleImageError(event) {
    const img = event.target;
    const title = img.alt || 'Project';
    const placeholder = generatePlaceholderImage(title, '#4f46e5', '#3730a3');
    img.src = placeholder;
}

function handleProfileError(event) {
    const img = event.target;
    img.style.display = 'none';
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
    { 
        title: 'Device Monitoring System', 
        type: 'Web Dashboard', 
        short: 'Dashboard monitoring real-time dengan visual data yang ringkas dan mudah ditindaklanjuti.', 
        description: 'DMS Monitoring Dashboard menampilkan data operasional secara real-time dengan tampilan ringkas, status perangkat, dan insight yang mudah dibaca oleh user.', 
        tags: ['Vue', 'Node.js', 'MySQL'] 
    },
    { 
        title: 'Monitoring KWH', 
        type: 'IoT System', 
        short: 'Sistem monitoring KWH berbasis MQTT untuk membaca data listrik secara real-time.', 
        description: 'Project ini menghubungkan perangkat KWH dengan MQTT untuk membaca data listrik, menampilkan grafik, dan membantu proses analisis konsumsi daya.', 
        tags: ['MQTT', 'Node.js', 'IoT'] 
    },
    { 
        title: 'ECU', 
        type: 'IoT Dashboard', 
        short: 'Dashboard monitoring ECU untuk memantau data perangkat secara real-time.', 
        description: 'ECU Monitoring Dashboard menampilkan data operasional perangkat secara real-time dengan tampilan yang ringkas dan mudah dipahami.', 
        tags: ['Node.js', 'MQTT', 'Dashboard'] 
    },
    { 
        title: 'MyHandyman', 
        type: 'Mobile App', 
        short: 'Aplikasi Flutter untuk membantu user menemukan layanan handyman dan melakukan booking jasa.', 
        description: 'MyHandyman adalah aplikasi mobile yang membantu user mencari layanan handyman, melihat detail jasa, dan melakukan proses booking dengan UI yang sederhana.', 
        tags: ['Flutter', 'Mobile', 'UI/UX'] 
    },
    { 
        title: 'Portfolio Website', 
        type: 'Personal Website', 
        short: 'Website portfolio modern untuk menampilkan profil, skill, project, CV, dan kontak profesional.', 
        description: 'Portfolio ini dibuat dengan Vue dan Vite sebagai tempat menampilkan profile, skill, experience, project, CV, dan kontak profesional.', 
        tags: ['Vue', 'Vite', 'CSS Animation'] 
    },
];

// ─── PROJECT IMAGES ──────────────────────────────────────────────────
const projectImages = [
    {
        mainImage: getImagePath('/images/dms/dms-main.png'),
        subImages: [
            getImagePath('/images/dms/dms-sub.png'),
            getImagePath('/images/dms/dms-sub-2.png'),
            getImagePath('/images/dms/dms-main-2.png'),
        ],
    },
    {
        mainImage: getImagePath('/images/monKwh/monkwh-main.png'),
        subImages: [
            getImagePath('/images/monKwh/monkwh-sub.png'),
        ],
    },
    {
        mainImage: getImagePath('/images/ecu/Ecu-Main.png'),
        subImages: [
            getImagePath('/images/ecu/Ecu-Sub-1.png'),
            getImagePath('/images/ecu/Ecu-Sub-2.png'),
        ],
    },
    {
        mainImage: generatePlaceholderImage('MyHandyman', projectColors[3 % projectColors.length].primary, projectColors[3 % projectColors.length].dark),
        subImages: [
            generatePlaceholderImage('MyHandyman — View 1', projectColors[3 % projectColors.length].primary, projectColors[(3 + 1) % projectColors.length].dark),
        ],
    },
    {
        mainImage: generatePlaceholderImage('Portfolio Website', projectColors[4 % projectColors.length].primary, projectColors[4 % projectColors.length].dark),
        subImages: [
            generatePlaceholderImage('Portfolio Website — View 1', projectColors[4 % projectColors.length].primary, projectColors[(4 + 1) % projectColors.length].dark),
            generatePlaceholderImage('Portfolio Website — View 2', projectColors[(4 + 2) % projectColors.length].primary, projectColors[4 % projectColors.length].dark),
            generatePlaceholderImage('Portfolio Website — View 3', projectColors[(4 + 1) % projectColors.length].primary, projectColors[(4 + 2) % projectColors.length].dark),
        ],
    },
];

const projects = projectData.map((p, i) => ({
    ...p,
    mainImage: projectImages[i].mainImage,
    subImages: projectImages[i].subImages,
}));

// Alias image for backwards compatibility with slider thumbnails
projects.forEach(p => { p.image = p.mainImage; });

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
const isModalOpen = ref(false);
const modalImageIndex = ref(0);

// ─── MODAL FUNCTIONS ──────────────────────────────────────────────
function nextModalImage() {
    if (!selectedProject.value) return;
    const total = selectedProject.value.subImages.length + 1;
    modalImageIndex.value = (modalImageIndex.value + 1) % total;
}

function prevModalImage() {
    if (!selectedProject.value) return;
    const total = selectedProject.value.subImages.length + 1;
    modalImageIndex.value = (modalImageIndex.value - 1 + total) % total;
}

// ─── FUNGSI handleProjectClick ──────────────────────────────────
function handleProjectClick(index) {
    // Reset drag intent
    isDragIntent.value = false;
    isDragging.value = false;
    
    // Card click animation
    clickedCard.value = index;
    setTimeout(() => { 
        clickedCard.value = null; 
    }, 600);
    
    // Buka modal
    setTimeout(() => {
        selectedProject.value = projects[index];
        isModalOpen.value = true;
        modalImageIndex.value = 0;
    }, 50);
}

// ─── FUNGSI selectProjectFromThumb ──────────────────────────────────
function selectProjectFromThumb(index) {
    activeProject.value = index;
    handleProjectClick(index);
}

// ─── FUNGSI closeProject ──────────────────────────────────────────────
function closeProject() {
    selectedProject.value = null;
    isModalOpen.value = false;
}

// ─── DRAG STATE ──────────────────────────────────────────────────
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
    }
    resetDrag();
    document.body.style.userSelect = '';
    document.body.style.cursor = '';
}

// ─── KEYBOARD NAVIGATION ──────────────────────────────────────────
function handleKeydown(e) {
    if (e.key === 'ArrowRight' || e.key === 'ArrowDown') { 
        e.preventDefault(); 
        nextProject(); 
    }
    if (e.key === 'ArrowLeft' || e.key === 'ArrowUp') { 
        e.preventDefault(); 
        prevProject(); 
    }
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
    { icon: '🇮🇩', title: 'WIB (UTC+7)', desc: 'Jakarta, Indonesia' },
    { icon: '🎓', title: 'S1 Teknik Informatika', desc: "ISTTS — Institut Sains & Teknologi Komputasi\n2019 – 2024" },
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

.portfolio-shell::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.02'/%3E%3C/svg%3E");
    pointer-events: none;
    z-index: 0;
}

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
    gap: 16px;
    flex-wrap: wrap;
}

.nav-brand {
    display: flex;
    align-items: center;
    gap: 10px;
}

.brand-mark {
    display: grid;
    place-items: center;
    width: 34px;
    height: 34px;
    border-radius: 10px;
    background: linear-gradient(135deg, var(--accent), #4f46e5);
    color: #fff;
    font-size: 0.7rem;
    font-weight: 800;
}

.brand-name {
    font-size: 0.9rem;
    font-weight: 700;
    color: var(--text-primary);
}

.lang-toggle {
    display: flex;
    gap: 4px;
    background: rgba(255, 255, 255, 0.05);
    border-radius: 999px;
    padding: 4px;
}

.lang-btn {
    padding: 4px 12px;
    border: none;
    border-radius: 999px;
    font-size: 0.7rem;
    font-weight: 700;
    color: var(--text-secondary);
    background: transparent;
    cursor: pointer;
    transition: all 0.3s ease;
}

.lang-btn:hover {
    color: var(--text-primary);
}

.lang-btn.active {
    background: var(--accent);
    color: #fff;
}

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

.hero-profile-img {
    width: 120px;
    height: 160px;
    object-fit: cover;
    border-radius: 16px;
    background: transparent;
    display: inline-block;
    vertical-align: middle;
    margin-right: 12px;
    border: 2px solid rgba(255, 255, 255, 0.05);
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
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

.hero-right {
    display: flex;
    flex-direction: column;
    gap: 24px;
}

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
    margin-left: 20px;
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
    user-select: none;
    -webkit-user-select: none;
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
    pointer-events: none;
}

.project-card.active:hover .project-image {
    transform: scale(1.05);
}

.image-overlay {
    position: absolute;
    inset: 0;
    background: linear-gradient(180deg, transparent 40%, rgba(10, 10, 15, 0.6));
    pointer-events: none;
    z-index: 1;
}

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
    z-index: 9999 !important;
    display: grid;
    place-items: center;
    padding: 24px;
    background: rgba(5, 5, 10, 0.85);
    backdrop-filter: blur(20px);
    animation: fadeInBackdrop 0.3s ease;
}

@keyframes fadeInBackdrop {
    from { opacity: 0; }
    to { opacity: 1; }
}

.modal-card {
    position: relative;
    z-index: 10000 !important;
    width: min(900px, 100%);
    max-height: min(86vh, 800px);
    overflow-y: auto;
    border-radius: 24px;
    background: var(--bg-secondary);
    border: 1px solid var(--border-color);
    box-shadow: 0 25px 80px rgba(0, 0, 0, 0.5), 0 0 0 1px rgba(255, 255, 255, 0.05);
    animation: slideUpModal 0.4s cubic-bezier(0.16, 1, 0.3, 1);
}

@keyframes slideUpModal {
    from { 
        transform: scale(0.85) translateY(30px);
        opacity: 0;
    }
    to { 
        transform: scale(1) translateY(0);
        opacity: 1;
    }
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
    z-index: 10;
}

.modal-close-btn:hover { 
    background: rgba(255, 255, 255, 0.14);
    transform: scale(1.1);
}

.modal-close-btn:active {
    transform: scale(0.95);
}

.modal-slider {
    position: relative;
    width: 100%;
    overflow: hidden;
}

.modal-image {
    width: calc(100% - 32px);
    max-height: 450px;
    margin: 16px auto;
    display: block;
    border-radius: 16px;
    object-fit: cover;
}

.modal-slider-btn {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    width: 40px;
    height: 40px;
    border-radius: 50%;
    background: rgba(10, 10, 15, 0.6);
    backdrop-filter: blur(10px);
    border: 1px solid var(--border-color);
    color: #fff;
    font-size: 1.4rem;
    font-weight: 700;
    cursor: pointer;
    display: grid;
    place-items: center;
    transition: all 0.3s ease;
    z-index: 5;
}

.modal-slider-btn:hover {
    background: rgba(124, 58, 237, 0.3);
    border-color: var(--accent);
    transform: translateY(-50%) scale(1.1);
}

.modal-slider-prev { left: 12px; }
.modal-slider-next { right: 12px; }

.modal-dots {
    display: flex;
    justify-content: center;
    gap: 8px;
    padding: 16px 0 8px;
}

.modal-dots button {
    width: 8px;
    height: 8px;
    border-radius: 999px;
    background: rgba(255, 255, 255, 0.2);
    border: none;
    cursor: pointer;
    transition: all 0.3s ease;
    padding: 0;
}

.modal-dots button:hover { background: rgba(255, 255, 255, 0.4); }

.modal-dots button.active {
    width: 28px;
    background: var(--accent);
}

.modal-thumbnails {
    display: flex;
    gap: 8px;
    padding: 12px 16px 16px;
    justify-content: center;
    flex-wrap: wrap;
}

.modal-thumbnails button {
    width: 56px;
    height: 40px;
    border-radius: 8px;
    overflow: hidden;
    padding: 0;
    background: transparent;
    border: 2px solid transparent;
    cursor: pointer;
    transition: all 0.3s ease;
}

.modal-thumbnails button:hover {
    border-color: rgba(255, 255, 255, 0.2);
    transform: translateY(-2px);
}

.modal-thumbnails button.active {
    border-color: var(--accent);
    box-shadow: 0 0 0 2px rgba(124, 58, 237, 0.2);
}

.modal-thumbnails img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
    border-radius: 6px;
}

.modal-body { padding: 0 28px 32px; }

.modal-tags { margin-top: 20px; }

.project-modal-enter-active,
.project-modal-leave-active {
    transition: opacity 0.3s ease;
}

.project-modal-enter-from,
.project-modal-leave-to {
    opacity: 0;
}

.project-modal-enter-active .modal-card {
    animation: slideUpModal 0.4s cubic-bezier(0.16, 1, 0.3, 1);
}

.project-modal-leave-active .modal-card {
    animation: slideDownModal 0.3s cubic-bezier(0.16, 1, 0.3, 1);
}

@keyframes slideDownModal {
    from { 
        transform: scale(1) translateY(0);
        opacity: 1;
    }
    to { 
        transform: scale(0.95) translateY(20px);
        opacity: 0;
    }
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
    .brand-name { display: none; }

    .hero-title { font-size: clamp(2.4rem, 8vw, 3.5rem); }
    .hero-profile-img {
        width: 80px;
        height: 107px;
        margin-right: 8px;
    }
    .section-title { font-size: clamp(1.6rem, 5vw, 2.2rem); }

    .project-slider { min-height: 600px; }
    .tech-card-grid { grid-template-columns: 1fr; }
    .dashboard-showcase { grid-template-columns: repeat(5, 120px); gap: 8px; padding: 10px; }
    .stats-grid { grid-template-columns: 1fr 1fr; }

    .slider-btn { width: 40px; height: 40px; font-size: 1rem; }
    
    .modal-card { max-height: 90vh; }
    .modal-image { max-height: 200px; }
    .modal-body { padding: 0 16px 24px; }

    .modal-slider-btn { width: 32px; height: 32px; font-size: 1rem; }
    .modal-thumbnails button { width: 44px; height: 32px; }
}
</style>