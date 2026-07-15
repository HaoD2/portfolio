<template>
    <Teleport to="body">
        <!-- PAGE LOADER -->
        <div class="page-loader" :class="{ 'is-hidden': appLoaded }">
            <span class="loader-mark">KO</span>
            <span class="loader-bar"><span class="loader-bar-fill"></span></span>
        </div>
        <!-- LEAVE / UNLOAD OVERLAY -->
        <div class="page-leave-overlay" :class="{ 'is-active': isLeaving }"></div>
    </Teleport>

    <main class="portfolio-shell" :class="{ 'app-loaded': appLoaded }">
        <!-- PARTICLES BACKGROUND -->
        <div id="particles-bg" class="particles-bg" aria-hidden="true"></div>

        <!-- NAV -->
        <nav class="nav-bar" ref="navBar">
            
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
        <section id="about" class="hero-section section-reveal" data-section="about">
            <div class="hero-grid">
                <div class="hero-left">
                    <span class="hero-eyebrow">✦ {{ t('about.eyebrow') }}</span>
                    <div class="hero-title-wrapper">
                        <h1 class="hero-title">
                            <span class="name-line">Kevin Octavius</span>
                        </h1>
                    </div>
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
                        <p>Don't be afraid of the storm, be afraid of the sky that never changes color. Try something new today</p>
                        <span class="quote-author">— Vino</span>
                    </div>
                </div>
            </div>
        </section>

        <!-- ====== PROJECTS ====== -->
        <section id="projects" class="projects-section section-reveal" data-section="projects">
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
                             @click="handleProjectClick(index)"
                             @mouseenter="handleCardHover(index, true)"
                             @mouseleave="handleCardHover(index, false)">

                        <div class="project-image-wrap">
                            <img 
                                :src="project.image" 
                                :alt="project.title" 
                                class="project-image" 
                                loading="lazy"
                                @error="handleImageError"
                            />
                            <div class="image-overlay"></div>
                            <span class="project-type-badge">{{ getProjectType(project.type) }}</span>
                        </div>

                        <div class="project-content">
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
        <section id="skills" class="skills-section section-reveal" data-section="skills">
            <div class="section-header">
                <span class="section-eyebrow">{{ t('skills.eyebrow') }}</span>
                <h2 class="section-title">{{ t('skills.title') }}</h2>
            </div>

            <div class="tech-card-grid">
                <article v-for="(tech, index) in techCards" :key="tech.name" 
                        class="tech-card premium-card" 
                        :data-index="index"
                        :data-logo="tech.logo"
                        @click="handleCardClick"
                        @mouseenter="handleTechHover(index, true)"
                        @mouseleave="handleTechHover(index, false)">
                    <div class="tech-icon">
                        <img 
                            :src="getIconPath(tech.iconFile)" 
                            :alt="tech.name" 
                            class="tech-icon-img" 
                            loading="lazy"
                            @error="handleIconError"
                        />
                    </div>
                    <h3>{{ tech.name }}</h3>
                    <p>{{ getTechDescription(tech.description) }}</p>
                </article>
            </div>
        </section>


        <!-- SECTION DIVIDER -->
        <div class="section-divider" aria-hidden="true"></div>

        <!-- ====== CONTACT ====== -->
        <section id="contact" class="contact-section section-reveal" data-section="contact">
            <div class="contact-grid">
                <div class="contact-left">
                    <span class="section-eyebrow">{{ t('contact.eyebrow') }}</span>
                    <h2 class="section-title">{{ t('contact.title') }}</h2>
                    <p class="contact-copy">
                        {{ t('contact.copy') }}
                    </p>
                </div>

                <div class="contact-right">
                    <!-- Link yang bisa diklik (GitHub, LinkedIn, WhatsApp) -->
                    <a v-for="(social, index) in contactLinks.filter(l => !l.isEmail)" 
                    :key="`contact-${social.label}`"
                    :href="social.href"
                    :target="social.external ? '_blank' : undefined"
                    :rel="social.external ? 'noreferrer' : undefined"
                    class="contact-link"
                    :data-index="index"
                    @mouseenter="handleContactHover(index, true)"
                    @mouseleave="handleContactHover(index, false)">
                        <span class="social-icon" v-html="social.icon"></span>
                        {{ social.label }}
                    </a>

                    <!-- Email (tidak bisa diklik, hanya teks) -->
                    <div class="contact-link is-email">
                        <span class="social-icon" v-html="emailIcon"></span>
                        <span class="email-text">kevinocthao@gmail.com</span>
                    </div>
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
import { computed, ref, onMounted, onUnmounted, nextTick } from 'vue';

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
            title: 'Showcase proyek Utama.'
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
            'Web Dashboard': 'Web Dashboard',
            'IoT System': 'IoT System',
            'IoT Dashboard': 'IoT Dashboard'
        },
        projectShorts: {
            'Dashboard monitoring real-time dengan visual data yang ringkas dan mudah ditindaklanjuti.': 'Dashboard monitoring real-time dengan visual data yang ringkas dan mudah ditindaklanjuti.',
            'Sistem monitoring KWH berbasis MQTT untuk membaca data listrik secara real-time.': 'Sistem monitoring KWH berbasis MQTT untuk membaca data listrik secara real-time.',
            'Dashboard monitoring ECU untuk memantau data perangkat secara real-time.': 'Dashboard monitoring ECU untuk memantau data perangkat secara real-time.',
            'Dashboard monitoring untuk memantau Trip atau gangguan pada Sutet atau kabel secara real-time.': 'Dashboard monitoring untuk memantau Trip atau gangguan pada Sutet atau kabel secara real-time.'
        },
        projectDescriptions: {
            'DMS Monitoring Dashboard menampilkan data operasional secara real-time dengan tampilan ringkas, status perangkat, dan insight yang mudah dibaca oleh user.': 'DMS Monitoring Dashboard menampilkan data operasional secara real-time dengan tampilan ringkas, status perangkat, dan insight yang mudah dibaca oleh user.',
            'Project ini menghubungkan perangkat KWH dengan MQTT untuk membaca data listrik, menampilkan grafik, dan membantu proses analisis konsumsi daya.': 'Project ini menghubungkan perangkat KWH dengan MQTT untuk membaca data listrik, menampilkan grafik, dan membantu proses analisis konsumsi daya.',
            'ECU Monitoring Dashboard menampilkan data operasional perangkat secara real-time dengan tampilan yang ringkas dan mudah dipahami.': 'ECU Monitoring Dashboard menampilkan data operasional perangkat secara real-time dengan tampilan yang ringkas dan mudah dipahami.',
            'Dashboard monitoring untuk memantau Trip atau gangguan pada Sutet atau kabel secara real-time.': 'Dashboard monitoring untuk memantau Trip atau gangguan pada Sutet atau kabel secara real-time.'
        },
        projectTags: {
            'EJS': 'EJS',
            'Node.js': 'Node.js',
            'MySQL': 'MySQL',
            'MQTT': 'MQTT',
            'IoT': 'IoT',
            'Dashboard': 'Dashboard'
        },
        techDescriptions: {
            'Membangun logic frontend dan backend yang dinamis, clean, dan mudah dikembangkan.': 'Membangun logic frontend dan backend yang dinamis, clean, dan mudah dikembangkan.',
            'Membangun struktur dan styling website yang responsive, modern, dan accessible dengan best practices.': 'Membangun struktur dan styling website yang responsive, modern, dan accessible dengan best practices.',
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
            title: 'Main Project showcase.'
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
            'IoT Dashboard': 'IoT Dashboard'
        },
        projectShorts: {
            'Dashboard monitoring real-time dengan visual data yang ringkas dan mudah ditindaklanjuti.': 'Real-time monitoring dashboard with concise and actionable data visualization.',
            'Sistem monitoring KWH berbasis MQTT untuk membaca data listrik secara real-time.': 'MQTT-based KWH monitoring system for real-time electricity data reading.',
            'Dashboard monitoring ECU untuk memantau data perangkat secara real-time.': 'ECU monitoring dashboard for real-time device data monitoring.',
            'Dashboard monitoring untuk memantau Trip atau gangguan pada Sutet atau kabel secara real-time.': 'Real-time monitoring dashboard for Trip or disturbance on Sutet or cables.'
        },
        projectDescriptions: {
            'DMS Monitoring Dashboard menampilkan data operasional secara real-time dengan tampilan ringkas, status perangkat, dan insight yang mudah dibaca oleh user.': 'DMS Monitoring Dashboard displays operational data in real-time with a concise layout, device status, and easy-to-read insights for users.',
            'Project ini menghubungkan perangkat KWH dengan MQTT untuk membaca data listrik, menampilkan grafik, dan membantu proses analisis konsumsi daya.': 'This project connects KWH devices with MQTT to read electricity data, display graphs, and assist in power consumption analysis.',
            'ECU Monitoring Dashboard menampilkan data operasional perangkat secara real-time dengan tampilan yang ringkas dan mudah dipahami.': 'ECU Monitoring Dashboard displays device operational data in real-time with a concise and easy-to-understand layout.',
            'Dashboard monitoring untuk memantau Trip atau gangguan pada Sutet atau kabel secara real-time.': 'Real-time monitoring dashboard for Trip or disturbance on Sutet or cables.'
        },
        projectTags: {
            'EJS': 'EJS',
            'Node.js': 'Node.js',
            'MySQL': 'MySQL',
            'MQTT': 'MQTT',
            'IoT': 'IoT',
            'Dashboard': 'Dashboard'
        },
        techDescriptions: {
            'Membangun logic frontend dan backend yang dinamis, clean, dan mudah dikembangkan.': 'Building dynamic, clean, and maintainable frontend and backend logic.',
            'Membangun struktur dan styling website yang responsive, modern, dan accessible dengan best practices.': 'Building responsive, modern, and accessible website structures and styling with best practices.',
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
const BASE_URL = '/portfolio'; // karena server menggunakan /portfolio/

function getImagePath(path) {
    return `${BASE_URL}${path}`;
}

// ─── ICON PATH ──────────────────────────────────────────────────────
function getIconPath(iconFile) {
    if (!iconFile) return '';
    return `${BASE_URL}/images/icons/${iconFile}`;
}

// ─── ICON ERROR HANDLING ──────────────────────────────────────────
function handleIconError(event) {
    const img = event.target;
    // Fallback ke teks jika gambar gagal load
    img.style.display = 'none';
    const parent = img.parentElement;
    parent.textContent = img.alt.charAt(0).toUpperCase();
    parent.style.color = 'var(--accent-light)';
    parent.style.fontFamily = 'var(--font-mono)';
    parent.style.fontWeight = '900';
    parent.style.fontSize = '0.8rem';
    parent.style.display = 'grid';
    parent.style.placeItems = 'center';
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
    { primary: '#0d9488', dark: '#0f766e' }
];

// projectData
const projectData = [
    { 
        title: 'Device Monitoring System', 
        type: 'Web Dashboard', 
        short: 'Dashboard monitoring real-time dengan visual data yang ringkas dan mudah ditindaklanjuti.', 
        description: 'DMS Monitoring Dashboard menampilkan data operasional secara real-time dengan tampilan ringkas, status perangkat, dan insight yang mudah dibaca oleh user.', 
        tags: ['Node.js', 'MQTT', 'Telegram', 'EJS', 'MySQL', 'IoT', 'HTML', 'CSS'] 
    },
    { 
        title: 'Monitoring KWH', 
        type: 'IoT System', 
        short: 'Sistem monitoring KWH berbasis MQTT untuk membaca data listrik secara real-time.', 
        description: 'Project ini menghubungkan perangkat KWH dengan MQTT untuk membaca data listrik, menampilkan grafik, dan membantu proses analisis konsumsi daya.', 
        tags: ['Node.js', 'MQTT', 'EJS', 'MySQL', 'IoT', 'HTML', 'CSS'] 
    },
    { 
        title: 'ECU', 
        type: 'IoT Dashboard', 
        short: 'Dashboard monitoring ECU untuk memantau data perangkat secara real-time.', 
        description: 'ECU Monitoring Dashboard menampilkan data operasional perangkat secara real-time dengan tampilan yang ringkas dan mudah dipahami.', 
        tags: ['Node.js', 'MQTT', 'EJS', 'IoT', 'HTML', 'CSS', 'NoSQL'] 
    },
    { 
        title: 'TCC PLN', 
        type: 'IoT Dashboard', 
        short: 'Dashboard monitoring untuk memantau Trip atau gangguan pada Sutet atau kabel secara real-time.', 
        description: 'Dashboard monitoring untuk memantau Trip atau gangguan pada Sutet atau kabel secara real-time.', 
        tags: ['Cesium JS', 'HTML', 'CSS', 'NoSQL'] 
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
        mainImage: getImagePath('/images/tcc/tcc-main.PNG'),
        subImages: [
            getImagePath('/images/tcc/tcc-sub-1.PNG'),
            getImagePath('/images/tcc/tcc-sub-2.PNG'),
            getImagePath('/images/tcc/tcc-sub-3.PNG'),
        ],
    }
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

// ─── FIX: NAVIGATION STATE ──────────────────────────────────────────
const isNavigating = ref(false);
const navTimeout = ref(null);

// ─── LOAD / UNLOAD ANIMATION STATE ─────────────────────────────────
const appLoaded = ref(false);
const isLeaving = ref(false);

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

// ─── FIX: SCROLL TO ──────────────────────────────────────────────
function scrollTo(selector) {
    const target = document.querySelector(selector);
    if (target) {
        const navHeight = navBar.value?.offsetHeight || 80;
        const targetTop = target.getBoundingClientRect().top + window.pageYOffset;
        const offset = targetTop - navHeight - 40;
        window.scrollTo({ 
            top: offset, 
            behavior: 'smooth' 
        });
    }
}

// ─── FIX: NAVIGATION ──────────────────────────────────────────────
function handleNavClick(section) {
    // Cegah navigasi ganda
    if (isNavigating.value) return;
    isNavigating.value = true;
    
    // Clear timeout sebelumnya
    if (navTimeout.value) {
        clearTimeout(navTimeout.value);
        navTimeout.value = null;
    }
    
    // Update active section langsung
    activeSection.value = section;
    
    // Scroll ke section (tanpa reload animasi)
    scrollTo(`#${section}`);
    
    // Tunggu scroll selesai
    navTimeout.value = setTimeout(() => {
        // Force update active section setelah scroll
        updateActiveSection();
        
        // Izinkan navigasi lagi setelah delay
        setTimeout(() => {
            isNavigating.value = false;
            navTimeout.value = null;
        }, 300);
    }, 700);
}

// ─── FIX: UPDATE ACTIVE SECTION ──────────────────────────────────
function updateActiveSection() {
    // Jika sedang navigasi, jangan update
    if (isNavigating.value) return;
    
    const sections = navItems.map(item => item.href.slice(1));
    const scrollY = window.scrollY + 150;
    let foundSection = null;
    
    for (const section of sections) {
        const el = document.getElementById(section);
        if (el) {
            const rect = el.getBoundingClientRect();
            const top = rect.top + window.pageYOffset;
            const bottom = rect.bottom + window.pageYOffset;
            
            // Cek apakah section terlihat di viewport
            if (scrollY >= top && scrollY < bottom) {
                foundSection = section;
                break;
            }
        }
    }
    
    // Jika tidak ada section yang terdeteksi, cari yang terdekat
    if (!foundSection) {
        let minDistance = Infinity;
        for (const section of sections) {
            const el = document.getElementById(section);
            if (el) {
                const top = el.getBoundingClientRect().top + window.pageYOffset;
                const distance = Math.abs(scrollY - top);
                if (distance < minDistance) {
                    minDistance = distance;
                    foundSection = section;
                }
            }
        }
    }
    
    if (foundSection) {
        activeSection.value = foundSection;
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

// ─── REDUCED MOTION ─────────────────────────────────────────────────
const prefersReducedMotion = typeof window !== 'undefined'
    && window.matchMedia
    && window.matchMedia('(prefers-reduced-motion: reduce)').matches;

// ─── ANIME.JS ──────────────────────────────────────────────────────
let animeLoaded = false;
let animeReady = false;
let aboutAnimations = null;
let hoverAnimations = {};

function loadAnimeScript() {
    return new Promise((resolve, reject) => {
        if (window.anime) {
            animeLoaded = true;
            animeReady = true;
            resolve();
            return;
        }
        const existing = document.querySelector('script[data-anime-js]');
        if (existing) {
            existing.addEventListener('load', () => {
                animeLoaded = true;
                animeReady = true;
                resolve();
            });
            existing.addEventListener('error', reject);
            return;
        }
        const script = document.createElement('script');
        script.src = 'https://cdnjs.cloudflare.com/ajax/libs/animejs/3.2.2/anime.min.js';
        script.async = true;
        script.dataset.animeJs = 'true';
        script.addEventListener('load', () => {
            animeLoaded = true;
            animeReady = true;
            resolve();
        });
        script.addEventListener('error', reject);
        document.head.appendChild(script);
    });
}

// ─── ANIMASI NAVBAR ────────────────────────────────────────────────
function animateNavbar() {
    const nav = document.querySelector('.nav-bar');
    if (!nav || !animeReady || !window.anime) return;
    
    if (prefersReducedMotion) {
        nav.style.opacity = '1';
        nav.style.transform = 'none';
        return;
    }

    try {
        window.anime({
            targets: nav,
            opacity: [0, 1],
            translateY: [-30, 0],
            duration: 800,
            easing: 'easeOutExpo',
            delay: 300
        });
    } catch (error) {
        console.warn('Navbar animation error:', error);
        nav.style.opacity = '1';
        nav.style.transform = 'none';
    }
}

// ─── ANIMASI HERO STATS ────────────────────────────────────────────
function animateHeroStats() {
    const stats = document.querySelectorAll('.stat-item');
    if (!stats.length || !animeReady || !window.anime) return;
    
    if (prefersReducedMotion) {
        stats.forEach(el => {
            el.style.opacity = '1';
            el.style.transform = 'none';
        });
        return;
    }

    try {
        stats.forEach(el => {
            el.style.opacity = '0';
            el.style.transform = 'scale(0.8)';
        });

        window.anime({
            targets: stats,
            opacity: [0, 1],
            scale: [0.8, 1],
            duration: 600,
            delay: window.anime.stagger(100),
            easing: 'easeOutBack'
        });
    } catch (error) {
        console.warn('Hero stats animation error:', error);
        stats.forEach(el => {
            el.style.opacity = '1';
            el.style.transform = 'none';
        });
    }
}

// ─── FIX: ANIMASI KHUSUS ABOUT ──────────────────────────────────────────
function animateAboutText() {
    const aboutSection = document.getElementById('about');
    if (!aboutSection) return;
    
    // FIX: Ambil elemen yang benar
    const titleEl = aboutSection.querySelector('.hero-title');
    const subtitleEl = aboutSection.querySelector('.hero-subtitle');
    const descEl = aboutSection.querySelector('.hero-description');
    const textElements = [titleEl, subtitleEl, descEl].filter(el => el !== null);
    
    if (!animeReady || !window.anime || prefersReducedMotion) {
        textElements.forEach(el => {
            el.style.opacity = '1';
            el.style.transform = 'none';
            const spans = el.querySelectorAll('span');
            spans.forEach(span => {
                span.style.opacity = '1';
                span.style.transform = 'none';
            });
        });
        return;
    }

    try {
        if (aboutAnimations) {
            aboutAnimations.pause();
            aboutAnimations = null;
        }

        // Proses setiap elemen text
        textElements.forEach(el => {
            if (el.dataset.split !== 'true') {
                const text = el.textContent;
                el.textContent = '';
                el.dataset.split = 'true';
                

                const words = text.split(/(\s+)/);
                words.forEach((chunk) => {
                    if (!chunk.length) return;
                    const span = document.createElement('span');
                    span.textContent = chunk;
                    span.style.display = 'inline-block';
                    span.style.opacity = '0';
                    span.style.transform = 'translateY(30px)';
                    // FIX: Jika chunk adalah spasi, beri lebar khusus
                    if (chunk.trim().length === 0) {
                        span.style.width = '0.15em';
                        span.style.minWidth = '0.15em';
                    }
                    el.appendChild(span);
                });
            }
        });

        // FIX: Kumpulkan semua spans dari semua elemen text
        const allSpans = aboutSection.querySelectorAll(
            '.hero-title span, .hero-subtitle span, .hero-description span'
        );
        
        if (allSpans.length > 0) {
            aboutAnimations = window.anime({
                targets: allSpans,
                opacity: [0, 1],
                translateY: [30, 0],
                duration: 800,
                delay: window.anime.stagger(50),
                easing: 'easeOutCubic',
                loop: false
            });
        }

        animateHeroStats();
    } catch (error) {
        console.warn('Anime.js error in about animation:', error);
        textElements.forEach(el => {
            el.style.opacity = '1';
            el.style.transform = 'none';
            const spans = el.querySelectorAll('span');
            spans.forEach(span => {
                span.style.opacity = '1';
                span.style.transform = 'none';
            });
        });
    }
}

// ─── ANIMASI SECTION LAIN ──────────────────────────────────────────
function splitIntoWords(el) {
    if (!el || el.dataset.split === 'true') return [];
    const text = el.textContent;
    el.textContent = '';
    el.dataset.split = 'true';
    const spans = [];
    
    const words = text.split(/(\s+)/);
    words.forEach((chunk) => {
        if (!chunk.length) return;
        const span = document.createElement('span');
        span.textContent = chunk;
        span.style.display = 'inline-block';
        span.style.opacity = '0';
        span.style.transform = 'translateY(18px)';
        if (chunk.trim().length === 0) {
            span.style.width = '0.25em';
            span.style.minWidth = '0.25em';
        }
        spans.push(span);
        el.appendChild(span);
    });
    return spans;
}

function playSectionTextReveal(section) {
    if (!section) return;
    
    if (section.dataset.animated === 'true') {
        return;
    }
    
    if (!animeReady || !window.anime || prefersReducedMotion) {
        section.style.opacity = '1';
        section.style.transform = 'none';
        section.dataset.animated = 'true';
        return;
    }

    try {
        const sectionId = section.id;
        const eyebrowEl = section.querySelector('.section-eyebrow');
        const titleEl = section.querySelector('.section-title');
        const words = titleEl ? splitIntoWords(titleEl) : [];

        window.anime({
            targets: section,
            opacity: [0, 1],
            translateY: [40, 0],
            duration: 800,
            easing: 'easeOutExpo',
        });

        const tl = window.anime.timeline({ easing: 'easeOutExpo' });
        
        if (eyebrowEl) {
            tl.add({ 
                targets: eyebrowEl, 
                opacity: [0, 1], 
                translateX: [-12, 0], 
                duration: 500 
            });
        }
        
        if (words.length) {
            tl.add({
                targets: words,
                opacity: [0, 1],
                translateY: [18, 0],
                duration: 550,
                delay: window.anime.stagger(30),
            }, eyebrowEl ? '-=280' : 0);
        }

        if (sectionId === 'projects') {
            setTimeout(() => {
                const cards = section.querySelectorAll('.project-card.active');
                if (cards.length && window.anime) {
                    window.anime({
                        targets: cards,
                        opacity: [0, 1],
                        scale: [0.9, 1],
                        duration: 600,
                        delay: window.anime.stagger(100),
                        easing: 'easeOutExpo'
                    });
                }
            }, 400);
        }

        if (sectionId === 'skills') {
            setTimeout(() => {
                const cards = section.querySelectorAll('.tech-card');
                if (cards.length && window.anime) {
                    cards.forEach(card => {
                        card.style.opacity = '0';
                        card.style.transform = 'translateY(30px)';
                    });
                    
                    window.anime({
                        targets: cards,
                        opacity: [0, 1],
                        translateY: [30, 0],
                        duration: 600,
                        delay: window.anime.stagger(80),
                        easing: 'easeOutExpo'
                    });
                }
            }, 400);
        }

        if (sectionId === 'contact') {
            setTimeout(() => {
                const links = section.querySelectorAll('.contact-link');
                if (links.length && window.anime) {
                    links.forEach(link => {
                        link.style.opacity = '0';
                        link.style.transform = 'translateX(30px)';
                    });
                    
                    window.anime({
                        targets: links,
                        opacity: [0, 1],
                        translateX: [30, 0],
                        duration: 600,
                        delay: window.anime.stagger(100),
                        easing: 'easeOutExpo'
                    });
                }
            }, 400);
        }

        section.dataset.animated = 'true';
    } catch (error) {
        console.warn('Anime.js error in section reveal:', error);
        section.style.opacity = '1';
        section.style.transform = 'none';
        section.dataset.animated = 'true';
    }
}

// ─── HOVER ANIMATIONS ──────────────────────────────────────────────
function handleCardHover(index, isEnter) {
    if (!animeReady || !window.anime || prefersReducedMotion) return;
    
    const cards = document.querySelectorAll('.project-card');
    const card = cards[index];
    if (!card) return;
    
    try {
        const key = `project-${index}`;
        if (hoverAnimations[key]) {
            hoverAnimations[key].pause();
            delete hoverAnimations[key];
        }
        
        hoverAnimations[key] = window.anime({
            targets: card,
            scale: isEnter ? 1.03 : 1,
            boxShadow: isEnter ? '0 20px 60px rgba(124, 58, 237, 0.2)' : 'none',
            duration: 300,
            easing: 'easeOutExpo'
        });
    } catch (error) {
        console.warn('Hover animation error:', error);
    }
}

function handleTechHover(index, isEnter) {
    if (!animeReady || !window.anime || prefersReducedMotion) return;
    
    const cards = document.querySelectorAll('.tech-card');
    const card = cards[index];
    if (!card) return;
    
    try {
        const key = `tech-${index}`;
        if (hoverAnimations[key]) {
            hoverAnimations[key].pause();
            delete hoverAnimations[key];
        }
        
        hoverAnimations[key] = window.anime({
            targets: card,
            scale: isEnter ? 1.05 : 1,
            boxShadow: isEnter ? '0 12px 40px rgba(124, 58, 237, 0.15)' : 'none',
            duration: 300,
            easing: 'easeOutExpo'
        });
    } catch (error) {
        console.warn('Tech hover animation error:', error);
    }
}

function handleContactHover(index, isEnter) {
    if (!animeReady || !window.anime || prefersReducedMotion) return;
    
    const links = document.querySelectorAll('.contact-link');
    const link = links[index];
    if (!link) return;
    
    try {
        const key = `contact-${index}`;
        if (hoverAnimations[key]) {
            hoverAnimations[key].pause();
            delete hoverAnimations[key];
        }
        
        hoverAnimations[key] = window.anime({
            targets: link,
            scale: isEnter ? 1.02 : 1,
            translateX: isEnter ? 8 : 0,
            boxShadow: isEnter ? '0 8px 24px rgba(124, 58, 237, 0.2)' : 'none',
            duration: 300,
            easing: 'easeOutExpo'
        });
    } catch (error) {
        console.warn('Contact hover animation error:', error);
    }
}

// ─── ANIMASI FOOTER ────────────────────────────────────────────────
function animateFooter() {
    const footer = document.querySelector('.site-footer');
    if (!footer || !animeReady || !window.anime) return;
    
    if (prefersReducedMotion) {
        footer.style.opacity = '1';
        footer.style.transform = 'none';
        return;
    }

    try {
        window.anime({
            targets: footer,
            opacity: [0, 1],
            translateY: [20, 0],
            duration: 600,
            easing: 'easeOutExpo',
            delay: 800
        });
    } catch (error) {
        console.warn('Footer animation error:', error);
        footer.style.opacity = '1';
        footer.style.transform = 'none';
    }
}

// ─── PARTICLES.JS BACKGROUND ───────────────────────────────────────
let particlesReady = false;
let particlesInstance = null;

function loadParticlesScript() {
    return new Promise((resolve, reject) => {
        if (window.particlesJS) {
            resolve();
            return;
        }
        const existing = document.querySelector('script[data-particles-js]');
        if (existing) {
            existing.addEventListener('load', resolve);
            existing.addEventListener('error', reject);
            return;
        }
        const script = document.createElement('script');
        script.src = 'https://cdn.jsdelivr.net/npm/particles.js';
        script.async = true;
        script.dataset.particlesJs = 'true';
        script.addEventListener('load', resolve);
        script.addEventListener('error', reject);
        document.head.appendChild(script);
    });
}

function initParticles() {
    if (!window.particlesJS || particlesReady) return;
    
    const container = document.getElementById('particles-bg');
    if (!container) {
        console.warn('Particles container not found');
        return;
    }

    particlesReady = true;

    try {
        window.particlesJS('particles-bg', {
            particles: {
                number: { 
                    value: 80,
                    density: { enable: true, value_area: 800 } 
                },
                color: { 
                    value: ['#a78bfa', '#818cf8', '#c084fc', '#e879f9', '#60a5fa']
                },
                shape: { 
                    type: 'circle' 
                },
                opacity: {
                    value: 0.7,
                    random: true,
                    anim: { 
                        enable: true, 
                        speed: 0.6,
                        opacity_min: 0.2,
                        sync: false 
                    }
                },
                size: { 
                    value: 3.5,
                    random: true,
                    anim: {
                        enable: true,
                        speed: 2,
                        size_min: 0.5,
                        sync: false
                    }
                },
                line_linked: {
                    enable: true,
                    distance: 150,
                    color: '#a78bfa',
                    opacity: 0.4,
                    width: 1.5
                },
                move: {
                    enable: true,
                    speed: 1.2,
                    direction: 'none',
                    random: true,
                    straight: false,
                    out_mode: 'out',
                    bounce: false,
                    attract: {
                        enable: true,
                        rotateX: 600,
                        rotateY: 1200
                    }
                }
            },
            interactivity: {
                detect_on: 'window',
                events: {
                    onhover: { 
                        enable: true, 
                        mode: 'grab' 
                    },
                    onclick: { 
                        enable: true, 
                        mode: 'push'
                    },
                    resize: true
                },
                modes: {
                    grab: { 
                        distance: 200, 
                        line_linked: { 
                            opacity: 0.6
                        } 
                    },
                    push: {
                        particles_nb: 6
                    }
                }
            },
            retina_detect: true
        }, function(instance) {
            particlesInstance = instance;
        });
    } catch (error) {
        console.warn('Particles.js error:', error);
        particlesReady = false;
    }
}

function destroyParticles() {
    try {
        if (particlesInstance && typeof particlesInstance.destroy === 'function') {
            particlesInstance.destroy();
            particlesInstance = null;
        }
        
        if (window.pJSDom && window.pJSDom.length) {
            window.pJSDom.forEach((dom) => {
                if (dom?.pJS?.fn?.vendors?.destroypJS) {
                    dom.pJS.fn.vendors.destroypJS();
                }
            });
            window.pJSDom = [];
        }
    } catch (error) {
        console.warn('Error destroying particles:', error);
    }
    particlesReady = false;
}

// ─── PAGE LEAVE / UNLOAD ANIMATION ─────────────────────────────────
function handleBeforeUnload() {
    isLeaving.value = true;
}

let sectionObservers = {};

// ─── LIFECYCLE ─────────────────────────────────────────────────────
onMounted(() => {
    document.addEventListener('keydown', handleKeydown);
    window.addEventListener('scroll', updateActiveSection);
    window.addEventListener('beforeunload', handleBeforeUnload);
    updateActiveSection();

    const loadParticles = loadParticlesScript();
    const loadAnime = loadAnimeScript();

    Promise.all([
        loadParticles,
        loadAnime
    ]).then(() => {
        nextTick(() => {
            initParticles();
        });
        
        setTimeout(() => {
            appLoaded.value = true;
            setTimeout(() => {
                animateNavbar();
                animateAboutText();
                animateFooter();
            }, 100);
        }, 500);
    }).catch((error) => {
        console.warn('Failed to load scripts:', error);
        appLoaded.value = true;
        nextTick(() => {
            document.querySelectorAll('.hero-left > *, .hero-right > *').forEach(el => {
                el.style.opacity = '1';
                el.style.transform = 'none';
            });
        });
    });

    // ─── INTERSECTION OBSERVER UNTUK ANIMASI SCROLL ──────────────
    const sections = ['projects', 'skills', 'contact'];
    
    sections.forEach(sectionId => {
        const section = document.getElementById(sectionId);
        if (!section) return;
        
        section.style.opacity = '0';
        section.style.transform = 'translateY(40px)';
        section.dataset.animated = 'false';
        
        const observer = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting && entry.intersectionRatio > 0.1) {
                    if (section.dataset.animated !== 'true') {
                        playSectionTextReveal(section);
                    }
                }
            });
        }, { 
            threshold: [0.1, 0.3],
            rootMargin: '-50px 0px'
        });
        
        sectionObservers[sectionId] = observer;
        observer.observe(section);
    });
});

onUnmounted(() => {
    document.removeEventListener('keydown', handleKeydown);
    window.removeEventListener('scroll', updateActiveSection);
    window.removeEventListener('beforeunload', handleBeforeUnload);
    document.body.style.userSelect = '';
    document.body.style.cursor = '';
    
    if (navTimeout.value) {
        clearTimeout(navTimeout.value);
        navTimeout.value = null;
    }
    
    Object.values(sectionObservers).forEach(observer => {
        if (observer) observer.disconnect();
    });
    sectionObservers = {};
    
    destroyParticles();
    
    if (aboutAnimations) {
        aboutAnimations.pause();
        aboutAnimations = null;
    }
    
    Object.values(hoverAnimations).forEach(anim => {
        if (anim) anim.pause();
    });
    hoverAnimations = {};
});

// ─── STATIC DATA ───────────────────────────────────────────────────
const githubIcon = `<svg viewBox="0 0 24 24" aria-hidden="true"><path fill="currentColor" d="M12 2a10 10 0 0 0-3.16 19.49c.5.09.68-.22.68-.48v-1.7c-2.78.6-3.37-1.18-3.37-1.18-.45-1.15-1.1-1.45-1.1-1.45-.9-.62.07-.6.07-.6 1 .07 1.53 1.03 1.53 1.03.89 1.52 2.34 1.08 2.9.83.1-.64.35-1.08.63-1.33-2.22-.25-4.55-1.11-4.55-4.94 0-1.09.39-1.98 1.03-2.68-.1-.25-.45-1.27.1-2.65 0 0 .84-.27 2.75 1.02A9.5 9.5 0 0 1 12 7.03c.85 0 1.7.11 2.5.33 1.9-1.29 2.74-1.02 2.74-1.02.55 1.38.2 2.4.1 2.65.64.7 1.03 1.59 1.03 2.68 0 3.84-2.34 4.69-4.57 4.94.36.31.68.92.68 1.86V21c0 .27.18.58.69.48A10 10 0 0 0 12 2Z"/></svg>`;
const linkedinIcon = `<svg viewBox="0 0 24 24" aria-hidden="true"><path fill="currentColor" d="M6.94 8.98H3.82V20h3.12V8.98ZM5.38 4A1.82 1.82 0 1 0 5.4 7.64 1.82 1.82 0 0 0 5.38 4Zm15 9.68c0-3.35-1.78-4.9-4.16-4.9a3.59 3.59 0 0 0-3.23 1.77V8.98H10V20h3.11v-5.45c0-1.44.27-2.83 2.05-2.83 1.76 0 1.79 1.64 1.79 2.92V20h3.11l.32-6.32Z"/></svg>`;
const emailIcon = `<svg viewBox="0 0 24 24" aria-hidden="true"><path fill="currentColor" d="M3 5.5A1.5 1.5 0 0 1 4.5 4h15A1.5 1.5 0 0 1 21 5.5v13a1.5 1.5 0 0 1-1.5 1.5h-15A1.5 1.5 0 0 1 3 18.5v-13Zm2.2.5 6.8 5.44L18.8 6H5.2ZM19.5 8.1l-6.86 5.5a1.5 1.5 0 0 1-1.88 0L4.5 8.1v10.4h15V8.1Z"/></svg>`;
const whatsappIcon = `<svg viewBox="0 0 24 24" aria-hidden="true"><path fill="currentColor" d="M17.47 14.38c-.3-.15-1.76-.87-2.03-.97-.27-.1-.47-.15-.67.15-.2.3-.77.97-.94 1.17-.17.2-.35.22-.65.07-.3-.15-1.24-.46-2.36-1.46-.87-.78-1.46-1.74-1.63-2.04-.17-.3-.02-.46.13-.61.13-.13.3-.35.44-.52.15-.17.2-.3.3-.5.1-.2.05-.37-.02-.52-.07-.15-.67-1.62-.92-2.22-.24-.58-.49-.5-.67-.51h-.57c-.2 0-.52.07-.79.37-.27.3-1.04 1.02-1.04 2.48s1.07 2.88 1.22 3.08c.15.2 2.1 3.2 5.08 4.49.71.31 1.26.49 1.69.62.71.23 1.36.2 1.87.12.57-.08 1.76-.72 2.01-1.42.25-.7.25-1.29.17-1.42-.07-.12-.27-.2-.57-.35ZM12.02 2C6.5 2 2 6.48 2 12c0 1.9.53 3.68 1.44 5.2L2 22l4.94-1.4A9.94 9.94 0 0 0 12.02 22C17.55 22 22 17.52 22 12S17.55 2 12.02 2Zm0 18.1a8.1 8.1 0 0 1-4.13-1.13l-.3-.18-3 .85.83-2.93-.19-.3A8.08 8.08 0 1 1 12.02 20.1Z"/></svg>`;

const socials = [
    { label: 'GitHub', href: 'https://github.com/HaoD2', icon: githubIcon },
    { label: 'LinkedIn', href: 'https://id.linkedin.com/in/kevin-octavius-574aa2137', icon: linkedinIcon },
];

const socialLinks = [
    { label: 'GitHub', href: 'https://github.com/HaoD2', icon: githubIcon, external: true },
    { label: 'LinkedIn', href: 'https://id.linkedin.com/in/kevin-octavius-574aa2137', icon: linkedinIcon, external: true },
    { label: 'WhatsApp', href: 'https://wa.me/6281234567890', icon: whatsappIcon, external: true },
];

const contactLinks = [
    ...socialLinks,
    { 
        label: '', 
        href: '#', 
        icon: emailIcon, 
        external: false,
        isEmail: true,
        emailAddress: 'kevinocthao@gmail.com'
    },
];

const metaItems = [
    { icon: '🇮🇩', title: 'WIB (UTC+7)', desc: 'Surabaya, Indonesia' },
    { icon: '🎓', title: 'S1 Teknik Informatika', desc: "ISTTS — Institut Sains & Teknologi Komputasi\n2019 – 2024" },
];

const aboutStats = [
    { value: '10+', label: 'Projects' },
    { value: '2+', label: 'Years of Experience' },
    { value: '25+', label: 'Cup of Coffe' },
    { value: '8+', label: 'Technical Skills' },
];

// ─── TECH CARDS ──────────────────────────────────────────────────────
const techCards = [
    { 
        name: 'JavaScript', 
        iconFile: 'js.png',
        description: 'Membangun logic frontend dan backend yang dinamis, clean, dan mudah dikembangkan.' 
    },
    { 
        name: 'HTML & CSS', 
        iconFile: 'html.png',
        description: 'Membangun struktur dan styling website yang responsive, modern, dan accessible dengan best practices.' 
    },
    { 
        name: 'EJS', 
        iconFile: 'ejs.png',
        description: 'Membuat interface interaktif, ringan, dan nyaman digunakan untuk dashboard modern.' 
    },
    { 
        name: 'Node.js', 
        iconFile: 'nodejs.png',
        description: 'Mengembangkan API, automation service, dan integrasi data untuk kebutuhan operasional.' 
    },
    { 
        name: 'Flutter', 
        iconFile: 'flutter.png',
        description: 'Membangun aplikasi mobile dengan UI konsisten dan pengalaman pengguna yang smooth.' 
    },
    { 
        name: 'MySQL', 
        iconFile: 'mysql.png',
        description: 'Merancang struktur data dan query untuk kebutuhan aplikasi serta reporting.' 
    },
    { 
        name: 'MQTT', 
        iconFile: 'mqtt.png',
        description: 'Menghubungkan device monitoring secara real-time untuk sistem berbasis IoT.' 
    },
    { 
        name: 'Docker', 
        iconFile: 'docker.png',
        description: 'Menyiapkan environment yang konsisten untuk development dan deployment.' 
    },
    { 
        name: 'GitLab', 
        iconFile: 'gitlab.png',
        description: 'Mengelola version control dan workflow kolaborasi secara rapi.' 
    },
];

</script>

<style scoped>
/* ═══════════════════════════════════════════════════════════════════
   FULL CSS - FINAL VERSION
   ═══════════════════════════════════════════════════════════════════ */

@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500;700&display=swap');

/* Local display font */
@font-face {
    font-family: 'Bowlby One';
    src: url('/portfolio/font/BowlbyOne-Regular.ttf') format('truetype');
    font-weight: 400;
    font-style: normal;
    font-display: swap;
}

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
    --font-body: 'Inter', ui-sans-serif, system-ui, -apple-system, 'Segoe UI', sans-serif;
    --font-display: 'Bowlby One', 'Inter', ui-sans-serif, system-ui, sans-serif;
    --font-mono: 'JetBrains Mono', ui-monospace, 'SFMono-Regular', Menlo, monospace;
}

:global(*) { box-sizing: border-box; margin: 0; padding: 0; }
:global(html) { scroll-behavior: smooth; font-size: 16px; }

:global(body) {
    min-width: 320px;
    background: var(--bg-primary);
    color: var(--text-primary);
    font-family: var(--font-body);
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

/* ═══════════════════ PARTICLES BACKGROUND ═══════════════════ */
.particles-bg {
    position: fixed;
    inset: 0;
    z-index: 0;
    pointer-events: none;
    min-height: 100vh;
    filter: drop-shadow(0 0 10px rgba(167, 139, 250, 0.1));
}

.particles-bg :deep(canvas) {
    display: block;
    width: 100% !important;
    height: 100% !important;
}

.particles-bg::after {
    content: '';
    position: absolute;
    inset: 0;
    background: radial-gradient(ellipse at center, rgba(10, 10, 15, 0.3), rgba(10, 10, 15, 0.7));
    pointer-events: none;
    z-index: 1;
}

/* ═══════════════════ PAGE LOADER ═══════════════════ */
.page-loader {
    position: fixed;
    inset: 0;
    z-index: 20000;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 18px;
    background: var(--bg-primary);
    transition: opacity 0.6s cubic-bezier(0.16, 1, 0.3, 1), visibility 0.6s cubic-bezier(0.16, 1, 0.3, 1);
}

.page-loader.is-hidden {
    opacity: 0;
    visibility: hidden;
    pointer-events: none;
}

.loader-mark {
    font-family: var(--font-mono);
    font-size: 1rem;
    font-weight: 700;
    letter-spacing: 0.4em;
    color: var(--accent-light);
    animation: loader-pulse 1.2s ease-in-out infinite;
}

.loader-bar {
    width: 120px;
    height: 2px;
    border-radius: 999px;
    background: rgba(255, 255, 255, 0.08);
    overflow: hidden;
}

.loader-bar-fill {
    display: block;
    height: 100%;
    width: 40%;
    background: linear-gradient(90deg, var(--accent), var(--accent-light));
    animation: loader-sweep 1.1s ease-in-out infinite;
}

@keyframes loader-pulse {
    0%, 100% { opacity: 0.35; transform: scale(0.94); }
    50% { opacity: 1; transform: scale(1.05); }
}

@keyframes loader-sweep {
    0% { transform: translateX(-120%); }
    100% { transform: translateX(340%); }
}

/* ═══════════════════ PAGE LEAVE OVERLAY ═══════════════════ */
.page-leave-overlay {
    position: fixed;
    inset: 0;
    z-index: 20001;
    background: var(--bg-primary);
    opacity: 0;
    pointer-events: none;
    transition: opacity 0.25s ease;
}

.page-leave-overlay.is-active {
    opacity: 1;
}

/* ═══════════════════ NAVIGATION ═══════════════════ */
.nav-bar {
    position: sticky;
    top: 20px;
    z-index: 1000;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 12px 24px;
    border-radius: 999px;
    background: rgba(15, 15, 22, 0.95);
    backdrop-filter: blur(20px) saturate(1.2);
    border: 1px solid var(--border-color);
    margin-bottom: 60px;
    gap: 16px;
    flex-wrap: wrap;
    opacity: 0;
    transform: translateY(-30px);
    box-shadow: 0 4px 30px rgba(0, 0, 0, 0.3);
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
    font-family: var(--font-mono);
    font-size: 0.7rem;
    font-weight: 800;
}

.brand-name {
    font-family: var(--font-display);
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
    font-family: var(--font-mono);
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
    will-change: transform, opacity;
    transition: opacity 0.6s ease, transform 0.6s ease;
}

#about, #projects, #skills, #contact {
    scroll-margin-top: 100px;
}

/* ═══════════════════ ANIMASI TEXT ABOUT ═══════════════════ */
.hero-title span,
.hero-subtitle span,
.hero-description span {
    display: inline-block;
    opacity: 0;
    transform: translateY(30px);
    transition: none;
}

.section-title span {
    display: inline-block;
    opacity: 0;
    transform: translateY(18px);
    transition: none;
}

/* ═══════════════════ SECTION HEADERS ═══════════════════ */
.section-header {
    margin-bottom: 48px;
}

.section-eyebrow {
    display: block;
    font-family: var(--font-mono);
    font-size: 0.7rem;
    font-weight: 700;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    color: var(--accent-light);
    margin-bottom: 12px;
    opacity: 0;
    transform: translateX(-12px);
}

.section-title {
    font-family: var(--font-display);
    font-size: clamp(1.7rem, 3.4vw, 2.6rem);
    font-weight: 400;
    letter-spacing: -0.01em;
    line-height: 1.15;
    color: var(--text-primary);
}

/* ═══════════════════ SECTION DIVIDER ═══════════════════ */
.section-divider {
    width: 60%;
    height: 1px;
    margin: 60px auto;
    background: linear-gradient(90deg, transparent, var(--border-color), transparent);
    opacity: 0.3;
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
    font-family: var(--font-mono);
    font-size: 0.75rem;
    font-weight: 700;
    letter-spacing: 0.18em;
    text-transform: uppercase;
    color: var(--accent-light);
    display: block;
    margin-bottom: 20px;
}

/* FIX: Hero title wrapper */
.hero-title-wrapper {
    display: flex;
    align-items: center;
    gap: 16px;
    margin-bottom: 16px;
}

.hero-profile-img {
    width: 120px;
    height: 160px;
    object-fit: cover;
    border-radius: 16px;
    border: 2px solid rgba(255, 255, 255, 0.05);
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
    flex-shrink: 0;
}

.hero-title {
    font-family: var(--font-display);
    font-size: clamp(2.6rem, 5.2vw, 4.6rem);
    font-weight: 400;
    letter-spacing: -0.01em;
    line-height: 1.05;
    color: var(--text-primary);
    margin: 0;
    display: flex;
    flex-direction: column;
}

.name-line {
    display: block;
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
    opacity: 0;
    transform: scale(0.8);
}

.stat-item:hover {
    background: var(--bg-card-hover);
    border-color: var(--border-hover);
    transform: translateY(-2px);
}

.stat-value {
    display: block;
    font-family: var(--font-mono);
    font-size: clamp(1.8rem, 3vw, 2.4rem);
    font-weight: 700;
    letter-spacing: -0.02em;
    color: var(--text-primary);
    line-height: 1;
}

.stat-label {
    display: block;
    font-family: var(--font-mono);
    font-size: 0.75rem;
    font-weight: 600;
    color: var(--text-muted);
    letter-spacing: 0.04em;
    text-transform: uppercase;
    margin-top: 6px;
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
    font-family: var(--font-mono);
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
    font-family: var(--font-display);
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
    font-family: var(--font-mono);
    font-size: 0.7rem;
    font-weight: 700;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    color: var(--text-muted);
}

/* ═══════════════════ PROJECTS SECTION ═══════════════════ */
.projects-section { 
    padding: 80px 0; 
}

.project-slider-wrapper { position: relative; }

.project-slider {
    position: relative;
    min-height: 480px;
    perspective: 1400px;
    overflow: hidden;
    touch-action: pan-y;
    cursor: grab;
    user-select: none;
    -webkit-user-select: none;
}

.project-slider:active { cursor: grabbing; }

/* ─── PROJECT CARDS ─── */
.project-card {
    position: absolute;
    inset: 0;
    display: grid;
    grid-template-columns: minmax(0, 1.2fr) minmax(220px, 0.6fr);
    gap: 24px;
    align-items: center;
    padding: 24px;
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
    box-shadow: none;
}

.project-card.active {
    z-index: 3;
    opacity: 1 !important;
    pointer-events: auto;
    transform: translateX(0) scale(1);
    background: rgba(20, 20, 30, 0.85);
    border-color: rgba(255, 255, 255, 0.12);
    box-shadow: 0 20px 60px rgba(0, 0, 0, 0.4);
}

.project-card.next {
    z-index: 2;
    opacity: 0.7 !important;
    transform: translateX(50%) scale(0.88) rotateY(-6deg);
    filter: blur(0.4px);
    background: rgba(15, 15, 25, 0.8);
    border-color: rgba(255, 255, 255, 0.06);
}

.project-card.previous {
    z-index: 1;
    opacity: 0.5 !important;
    transform: translateX(-50%) scale(0.88) rotateY(6deg);
    filter: blur(0.4px);
    background: rgba(15, 15, 25, 0.8);
    border-color: rgba(255, 255, 255, 0.06);
}

.project-card.hidden-card {
    transform: translateX(120%) scale(0.78);
    opacity: 0 !important;
    pointer-events: none;
}

.project-card.next .project-content,
.project-card.previous .project-content {
    opacity: 0.9;
}

.project-card.next .project-image,
.project-card.previous .project-image {
    opacity: 0.85;
}

.project-card.next .project-image-wrap,
.project-card.previous .project-image-wrap {
    background: rgba(0, 0, 0, 0.3);
}

.project-card.active:hover {
    transform: translateX(0) scale(1.02);
    border-color: var(--accent);
    box-shadow: 0 24px 80px rgba(124, 58, 237, 0.15);
    transition: all 0.3s cubic-bezier(0.16, 1, 0.3, 1);
}

.project-card.active:hover .project-image {
    transform: scale(1.05);
}

.project-card .project-content h3 {
    color: var(--text-primary);
    text-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
}

.project-card .project-content p {
    color: var(--text-secondary);
}

.project-card.next .project-content h3,
.project-card.previous .project-content h3 {
    color: rgba(255, 255, 255, 0.9);
}

.project-card.next .project-content p,
.project-card.previous .project-content p {
    color: rgba(255, 255, 255, 0.7);
}

.project-card.next .image-overlay,
.project-card.previous .image-overlay {
    background: linear-gradient(180deg, transparent 30%, rgba(0, 0, 0, 0.7));
}

.project-card.next .project-tags span,
.project-card.previous .project-tags span {
    background: rgba(255, 255, 255, 0.08);
    color: rgba(255, 255, 255, 0.8);
}

.project-image-wrap {
    position: relative;
    overflow: hidden;
    min-height: 320px;
    border-radius: 16px;
    background: rgba(20, 20, 35, 0.6);
}

.project-image {
    width: 100%;
    height: 100%;
    min-height: 320px;
    object-fit: cover;
    display: block;
    transition: transform 0.6s cubic-bezier(0.16, 1, 0.3, 1);
    pointer-events: none;
}

.image-overlay {
    position: absolute;
    inset: 0;
    background: linear-gradient(180deg, transparent 40%, rgba(0, 0, 0, 0.5));
    pointer-events: none;
    z-index: 1;
}

.project-type-badge {
    position: absolute;
    top: 16px;
    left: 16px;
    z-index: 2;
    display: inline-block;
    font-family: var(--font-mono);
    font-size: 0.65rem;
    font-weight: 800;
    letter-spacing: 0.14em;
    text-transform: uppercase;
    color: #fff;
    background: rgba(124, 58, 237, 0.85);
    padding: 4px 14px;
    border-radius: 999px;
    backdrop-filter: blur(8px);
    border: 1px solid rgba(255,255,255,0.1);
}

.project-content h3 {
    font-family: var(--font-display);
    font-size: clamp(1.2rem, 2vw, 1.7rem);
    font-weight: 400;
    letter-spacing: -0.01em;
    line-height: 1.15;
    color: var(--text-primary);
    margin-bottom: 10px;
}

.project-content p {
    font-size: 0.85rem;
    line-height: 1.6;
    color: var(--text-secondary);
    margin-bottom: 12px;
    display: -webkit-box;
    -webkit-line-clamp: 3;
    -webkit-box-orient: vertical;
    overflow: hidden;
}

.project-tags { display: flex; flex-wrap: wrap; gap: 6px; }

.project-tags span {
    padding: 3px 10px;
    border-radius: 999px;
    font-size: 0.65rem;
    font-weight: 600;
    color: var(--text-secondary);
    background: rgba(255, 255, 255, 0.04);
    border: 1px solid var(--border-color);
}

/* ─── SLIDER CONTROLS ─── */
.slider-controls {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 20px;
    margin-top: 28px;
}

.slider-btn {
    width: 44px;
    height: 44px;
    border-radius: 50%;
    background: var(--bg-card);
    border: 1px solid var(--border-color);
    color: var(--text-primary);
    cursor: pointer;
    display: grid;
    place-items: center;
    font-size: 1.1rem;
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

/* ─── DASHBOARD SHOWCASE ─── */
.dashboard-showcase {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 12px;
    margin-top: 20px;
    padding: 16px;
    border-radius: 20px;
    background: rgba(20, 20, 35, 0.6);
    border: 1px solid var(--border-color);
    max-width: 800px;
    margin-left: auto;
    margin-right: auto;
    backdrop-filter: blur(10px);
}

.dashboard-thumb {
    width: 100%;
    max-width: 180px;
    margin: 0 auto;
    border: none;
    border-radius: 12px;
    padding: 6px;
    background: rgba(255, 255, 255, 0.03);
    cursor: pointer;
    transition: all 0.3s ease;
    position: relative;
}

.dashboard-thumb::after {
    content: '';
    position: absolute;
    inset: 0;
    border-radius: 12px;
    background: linear-gradient(180deg, transparent 60%, rgba(0, 0, 0, 0.3));
    pointer-events: none;
    opacity: 0;
    transition: opacity 0.3s ease;
}

.dashboard-thumb:hover { 
    transform: translateY(-4px); 
    background: rgba(255, 255, 255, 0.06);
}

.dashboard-thumb.active {
    transform: translateY(-4px);
    background: rgba(124, 58, 237, 0.1);
}

.dashboard-thumb.active img {
    box-shadow: 0 0 0 2px var(--accent), 0 8px 24px rgba(124, 58, 237, 0.2);
    border-radius: 8px;
}

.dashboard-thumb img {
    width: 100%;
    height: 80px;
    display: block;
    object-fit: cover;
    border-radius: 8px;
    transition: all 0.3s ease;
}

.dashboard-thumb.active::after {
    opacity: 1;
}

/* ═══════════════════ MODAL ═══════════════════ */
.modal-backdrop {
    position: fixed;
    inset: 0;
    z-index: 9999 !important;
    display: grid;
    place-items: center;
    padding: 20px;
    background: rgba(5, 5, 10, 0.88);
    backdrop-filter: blur(24px);
    animation: fadeInBackdrop 0.3s ease;
}

@keyframes fadeInBackdrop {
    from { opacity: 0; }
    to { opacity: 1; }
}

.modal-card {
    position: relative;
    z-index: 10000 !important;
    width: min(1100px, 94vw);
    max-height: min(92vh, 900px);
    overflow-y: auto;
    border-radius: 28px;
    background: var(--bg-secondary);
    border: 1px solid rgba(255, 255, 255, 0.08);
    box-shadow: 0 40px 100px rgba(0, 0, 0, 0.6), 0 0 0 1px rgba(255, 255, 255, 0.04);
    animation: slideUpModal 0.4s cubic-bezier(0.16, 1, 0.3, 1);
}

@keyframes slideUpModal {
    from { 
        transform: scale(0.92) translateY(40px);
        opacity: 0;
    }
    to { 
        transform: scale(1) translateY(0);
        opacity: 1;
    }
}

.modal-close-btn {
    position: absolute;
    top: 20px;
    right: 20px;
    width: 44px;
    height: 44px;
    border-radius: 50%;
    background: rgba(255, 255, 255, 0.06);
    border: 1px solid rgba(255, 255, 255, 0.08);
    color: var(--text-primary);
    font-size: 1.2rem;
    cursor: pointer;
    display: grid;
    place-items: center;
    transition: all 0.3s ease;
    z-index: 10;
}

.modal-close-btn:hover { 
    background: rgba(255, 255, 255, 0.14);
    transform: scale(1.08);
}

.modal-close-btn:active {
    transform: scale(0.92);
}

.modal-slider {
    position: relative;
    width: 100%;
    overflow: hidden;
    padding: 20px 20px 0;
}

.modal-image {
    width: 100%;
    max-height: 550px;
    min-height: 300px;
    margin: 0 auto;
    display: block;
    border-radius: 16px;
    object-fit: contain;
    background: rgba(0, 0, 0, 0.3);
}

.modal-slider-btn {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    width: 48px;
    height: 48px;
    border-radius: 50%;
    background: rgba(10, 10, 15, 0.7);
    backdrop-filter: blur(12px);
    border: 1px solid rgba(255, 255, 255, 0.1);
    color: #fff;
    font-size: 1.6rem;
    font-weight: 700;
    cursor: pointer;
    display: grid;
    place-items: center;
    transition: all 0.3s ease;
    z-index: 5;
}

.modal-slider-btn:hover {
    background: rgba(124, 58, 237, 0.35);
    border-color: var(--accent);
    transform: translateY(-50%) scale(1.1);
}

.modal-slider-prev { left: 16px; }
.modal-slider-next { right: 16px; }

.modal-dots {
    display: flex;
    justify-content: center;
    gap: 10px;
    padding: 18px 0 10px;
}

.modal-dots button {
    width: 10px;
    height: 10px;
    border-radius: 999px;
    background: rgba(255, 255, 255, 0.15);
    border: none;
    cursor: pointer;
    transition: all 0.3s ease;
    padding: 0;
}

.modal-dots button:hover { background: rgba(255, 255, 255, 0.35); }

.modal-dots button.active {
    width: 36px;
    background: var(--accent);
}

.modal-thumbnails {
    display: flex;
    gap: 10px;
    padding: 14px 20px 20px;
    justify-content: center;
    flex-wrap: wrap;
}

.modal-thumbnails button {
    width: 72px;
    height: 52px;
    border-radius: 10px;
    overflow: hidden;
    padding: 0;
    background: rgba(255, 255, 255, 0.03);
    border: 2px solid transparent;
    cursor: pointer;
    transition: all 0.3s ease;
}

.modal-thumbnails button:hover {
    border-color: rgba(255, 255, 255, 0.2);
    transform: translateY(-3px);
}

.modal-thumbnails button.active {
    border-color: var(--accent);
    box-shadow: 0 0 0 3px rgba(124, 58, 237, 0.25);
}

.modal-thumbnails img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
    border-radius: 8px;
}

.modal-body { 
    padding: 8px 32px 32px; 
}

.modal-body .project-type {
    display: inline-block;
    font-family: var(--font-mono);
    font-size: 0.75rem;
    font-weight: 800;
    letter-spacing: 0.14em;
    text-transform: uppercase;
    color: var(--accent-light);
    margin-bottom: 8px;
}

.modal-body h2 {
    font-family: var(--font-display);
    font-size: clamp(1.6rem, 2.8vw, 2.4rem);
    font-weight: 400;
    letter-spacing: -0.01em;
    line-height: 1.15;
    color: var(--text-primary);
    margin-bottom: 12px;
}

.modal-body p {
    font-size: 1rem;
    line-height: 1.8;
    color: var(--text-secondary);
    max-width: 680px;
}

.modal-tags { 
    margin-top: 20px; 
}

.modal-tags span {
    padding: 6px 14px;
    border-radius: 999px;
    font-size: 0.75rem;
    font-weight: 600;
    color: var(--text-secondary);
    background: rgba(255, 255, 255, 0.05);
    border: 1px solid var(--border-color);
}

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
        transform: scale(0.95) translateY(30px);
        opacity: 0;
    }
}

/* Responsive untuk modal */
@media (max-width: 768px) {
    .modal-card {
        width: 96vw;
        max-height: 94vh;
        border-radius: 20px;
    }
    
    .modal-image {
        max-height: 350px;
        min-height: 200px;
    }
    
    .modal-slider-btn {
        width: 38px;
        height: 38px;
        font-size: 1.2rem;
    }
    
    .modal-slider-prev { left: 8px; }
    .modal-slider-next { right: 8px; }
    
    .modal-body {
        padding: 4px 18px 24px;
    }
    
    .modal-body p {
        font-size: 0.92rem;
    }
    
    .modal-thumbnails button {
        width: 56px;
        height: 40px;
    }
    
    .modal-close-btn {
        width: 38px;
        height: 38px;
        font-size: 1rem;
        top: 14px;
        right: 14px;
    }
}

@media (max-width: 480px) {
    .modal-card {
        max-height: 96vh;
        border-radius: 16px;
    }
    
    .modal-image {
        max-height: 240px;
        min-height: 150px;
    }
    
    .modal-slider-btn {
        width: 32px;
        height: 32px;
        font-size: 1rem;
    }
    
    .modal-body {
        padding: 2px 14px 20px;
    }
    
    .modal-body h2 {
        font-size: 1.3rem;
    }
    
    .modal-body p {
        font-size: 0.85rem;
        line-height: 1.6;
    }
    
    .modal-thumbnails button {
        width: 44px;
        height: 34px;
    }
    
    .modal-thumbnails {
        gap: 6px;
        padding: 10px 12px 16px;
    }
    
    .modal-dots {
        gap: 6px;
        padding: 12px 0 6px;
    }
    
    .modal-dots button {
        width: 8px;
        height: 8px;
    }
    
    .modal-dots button.active {
        width: 28px;
    }
}


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

.slide-fade-enter-active,
.slide-fade-leave-active {
    transition: opacity 0.35s ease, transform 0.35s ease;
}

.slide-fade-enter-from {
    opacity: 0;
    transform: translateX(16px);
}

.slide-fade-leave-to {
    opacity: 0;
    transform: translateX(-16px);
}

/* ═══════════════════ SKILLS SECTION ═══════════════════ */
.skills-section { 
    padding: 80px 0 100px; 
    position: relative;
    z-index: 1;
}

.tech-card-grid {
    display: grid;
    grid-template-columns: repeat(3, minmax(0, 1fr));
    gap: 14px;
}


.tech-card {
    padding: 24px;
    border-radius: 18px;
    background: var(--bg-card);
    border: 1px solid var(--border-color);
    transition: all 0.35s cubic-bezier(0.16, 1, 0.3, 1);
    cursor: pointer;
    position: relative;
    z-index: 1;
    opacity: 0;
    transform: translateY(30px);
    box-shadow: none;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
}

.tech-card:hover {
    background: var(--bg-card-hover);
    border-color: var(--border-hover);
    transform: translateY(-4px);
    z-index: 2;
}


.tech-card:nth-child(2) .tech-icon {
    background: linear-gradient(135deg, rgba(255, 99, 71, 0.15), rgba(49, 158, 255, 0.15));
    color: #ff6347;
    font-weight: 900;
}

.tech-icon {
    display: grid;
    place-items: center;
    width: 52px;
    height: 52px;
    margin-bottom: 16px;
    border-radius: 12px;
    background: rgba(124, 58, 237, 0.08);
    border: 1px solid rgba(124, 58, 237, 0.06);
    overflow: hidden;
    flex-shrink: 0;
}

.tech-icon-img {
    width: 32px;
    height: 32px;
    object-fit: contain;
    display: block;
}

.tech-card h3 {
    font-family: var(--font-display);
    font-size: 0.95rem;
    font-weight: 400;
    letter-spacing: 0.01em;
    color: var(--text-primary);
    margin-bottom: 8px;
}

.tech-card p {
    font-size: 0.85rem;
    line-height: 1.6;
    color: var(--text-secondary);
}

.tech-icon:not(:has(img)) {
    color: var(--accent-light);
    font-family: var(--font-mono);
    font-weight: 900;
    font-size: 0.8rem;
}



/* ═══════════════════ CONTACT SECTION ═══════════════════ */
.contact-section { 
    padding: 100px 0 120px; 
    position: relative;
    z-index: 10;
    margin-top: 20px;
}

.contact-section::before {
    content: '';
    display: block;
    height: 40px;
    width: 100%;
    background: transparent;
}

.contact-grid {
    display: grid;
    grid-template-columns: 1.2fr 1fr;
    gap: 60px;
    align-items: start;
    position: relative;
    z-index: 10;
    padding: 20px 0;
}

.contact-copy {
    font-size: 0.95rem;
    line-height: 1.8;
    color: var(--text-secondary);
    max-width: 480px;
}

.contact-right { 
    display: flex; 
    flex-direction: column; 
    gap: 16px;
    position: relative;
    z-index: 10;
    padding: 10px 0;
}

.contact-link {
    display: inline-flex;
    align-items: center;
    gap: 14px;
    padding: 18px 28px;
    border-radius: 14px;
    background: var(--bg-card);
    border: 1px solid var(--border-color);
    color: var(--text-primary);
    text-decoration: none;
    font-size: 0.95rem;
    font-weight: 600;
    transition: all 0.3s ease;
    position: relative;
    z-index: 5;
    cursor: pointer;
    opacity: 0;
    transform: translateX(30px);
    box-shadow: none;
}

.contact-link:hover {
    background: var(--bg-card-hover);
    border-color: var(--border-hover);
    transform: translateX(8px) translateY(-3px);
    box-shadow: 0 8px 32px rgba(124, 58, 237, 0.2);
    z-index: 10;
}

.social-icon {
    display: inline-grid;
    place-items: center;
    width: 24px;
    height: 24px;
    color: var(--accent-light);
}

.social-icon :deep(svg) { 
    width: 24px; 
    height: 24px; 
}

/* ═══════════════════ FOOTER ═══════════════════ */
.site-footer {
    text-align: center;
    padding: 40px 0;
    border-top: 1px solid var(--border-color);
    margin-top: 40px;
    opacity: 0;
    transform: translateY(20px);
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

/* ═══════════════════ REDUCED MOTION ═══════════════════ */
@media (prefers-reduced-motion: reduce) {
    .page-loader,
    .page-leave-overlay,
    .loader-mark,
    .loader-bar-fill,
    .avail-dot {
        transition-duration: 0.01ms !important;
        animation-duration: 0.01ms !important;
    }
    
    * {
        animation-duration: 0.01ms !important;
        transition-duration: 0.01ms !important;
    }
}

/* ═══════════════════ RESPONSIVE ═══════════════════ */
@media (max-width: 900px) {
    .hero-grid { grid-template-columns: 1fr; gap: 40px; }
    .hero-right { order: -1; }
    
    .contact-grid { 
        grid-template-columns: 1fr; 
        gap: 40px;
        padding: 10px 0;
    }
    
    .contact-section {
        padding: 80px 0 100px;
    }
    
    .contact-right {
        gap: 14px;
    }
    
    .contact-link {
        padding: 16px 24px;
        font-size: 0.9rem;
    }
    
    .social-icon {
        width: 20px;
        height: 20px;
    }
    
    .social-icon :deep(svg) {
        width: 20px;
        height: 20px;
    }

    .project-card {
        grid-template-columns: 1fr;
        padding: 20px;
        background: rgba(20, 20, 35, 0.9);
    }
    
    .project-card.next {
        opacity: 0.6 !important;
        transform: translateX(45%) scale(0.85) rotateY(-5deg);
    }
    
    .project-card.previous {
        opacity: 0.4 !important;
        transform: translateX(-45%) scale(0.85) rotateY(5deg);
    }
    
    .project-image,
    .project-image-wrap { 
        min-height: 220px; 
    }
    
    .project-card.active {
        background: rgba(20, 20, 35, 0.95);
    }

    .tech-card-grid { 
        grid-template-columns: repeat(2, minmax(0, 1fr)); 
    }

    .dashboard-showcase { 
        grid-template-columns: repeat(4, 1fr);
        max-width: 100%;
        gap: 10px;
        padding: 14px;
    }

    .nav-bar {
        border-radius: 20px;
        flex-wrap: wrap;
        justify-content: center;
        gap: 8px;
    }

    .meta-row { grid-template-columns: 1fr; }
    
    .section-divider {
        width: 80%;
        margin: 40px auto;
    }
}

@media (max-width: 560px) {
    .portfolio-shell { width: min(100% - 20px); padding: 0 0 40px; }
    .nav-bar { margin-bottom: 32px; padding: 10px 16px; }
    .nav-link { padding: 6px 10px; font-size: 0.78rem; }
    .brand-name { display: none; }

    .hero-title-wrapper {
        flex-direction: column;
        align-items: flex-start;
    }
    
    .hero-profile-img {
        width: 80px;
        height: 107px;
    }
    
    .hero-title { font-size: clamp(2.4rem, 8vw, 3.5rem); }
    .section-title { font-size: clamp(1.6rem, 5vw, 2.2rem); }

    .project-slider { min-height: 540px; }
    
    .project-card {
        padding: 16px;
        background: rgba(20, 20, 35, 0.95);
    }
    
    .project-card.next {
        opacity: 0.5 !important;
        transform: translateX(35%) scale(0.82) rotateY(-3deg);
    }
    
    .project-card.previous {
        opacity: 0.3 !important;
        transform: translateX(-35%) scale(0.82) rotateY(3deg);
    }
    
    .project-image,
    .project-image-wrap { 
        min-height: 160px; 
    }
    
    .tech-card-grid { 
        grid-template-columns: 1fr; 
    }
    
    .dashboard-showcase { 
        grid-template-columns: repeat(2, 1fr);
        gap: 8px; 
        padding: 12px;
        max-width: 100%;
    }
    
    .dashboard-thumb {
        padding: 4px;
        max-width: 100%;
    }
    
    .dashboard-thumb img {
        height: 60px;
    }
    
    .stats-grid { grid-template-columns: 1fr 1fr; }

    .slider-btn { width: 40px; height: 40px; font-size: 1rem; }
    
    .modal-card { max-height: 90vh; }
    .modal-image { max-height: 200px; }
    .modal-body { padding: 0 16px 24px; }

    .modal-slider-btn { width: 32px; height: 32px; font-size: 1rem; }
    .modal-thumbnails button { width: 44px; height: 32px; }
    
    .skills-section { padding: 60px 0 80px; }
    .contact-section { 
        padding: 60px 0 80px;
    }
    
    .contact-section::before {
        height: 20px;
    }
    
    .contact-grid {
        gap: 30px;
        padding: 0;
    }
    
    .contact-right {
        gap: 12px;
    }
    
    .contact-link {
        padding: 14px 20px;
        font-size: 0.85rem;
        gap: 12px;
    }
    
    .social-icon {
        width: 20px;
        height: 20px;
    }
    
    .social-icon :deep(svg) {
        width: 20px;
        height: 20px;
    }
    
    .section-divider {
        width: 90%;
        margin: 30px auto;
    }
}
</style>