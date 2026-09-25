<template>
  <div class="app-shell" :class="{ 'theme-light': theme === 'light' }">
    <div class="ambient ambient-one" aria-hidden="true"></div>
    <div class="ambient ambient-two" aria-hidden="true"></div>

    <header class="site-header glass-panel">
      <div class="nav-inner">
        <button class="brand" type="button" aria-label="Back to home" @click="redirectToHome">
          <span class="brand-mark">R</span>
          <span>RioAdriana<span class="brand-caret">();</span></span>
        </button>

        <nav class="desktop-nav" aria-label="Main navigation">
          <router-link v-for="item in navItems" :key="item.path" :to="item.path">
            {{ item.label }}
          </router-link>
        </nav>

        <div class="header-actions">
          <a class="icon-button github-button" href="https://github.com/RioAdrna" target="_blank" rel="noopener noreferrer" aria-label="Open GitHub profile">
            <svg viewBox="0 0 24 24" aria-hidden="true"><path fill="currentColor" d="M12 .5a12 12 0 0 0-3.79 23.39c.6.11.82-.26.82-.58v-2.25c-3.34.73-4.04-1.61-4.04-1.61-.55-1.39-1.34-1.76-1.34-1.76-1.09-.75.08-.74.08-.74 1.2.09 1.84 1.23 1.84 1.23 1.07 1.83 2.8 1.3 3.49.99.11-.77.42-1.3.76-1.6-2.67-.3-5.47-1.34-5.47-5.95 0-1.31.47-2.38 1.24-3.22-.12-.3-.54-1.52.12-3.17 0 0 1.01-.32 3.3 1.23a11.46 11.46 0 0 1 6 0c2.29-1.55 3.3-1.23 3.3-1.23.66 1.65.24 2.87.12 3.17.77.84 1.24 1.91 1.24 3.22 0 4.62-2.8 5.64-5.48 5.94.43.37.81 1.1.81 2.22v3.29c0 .32.22.69.83.57A12 12 0 0 0 12 .5Z"/></svg>
          </a>
          <button class="theme-toggle" type="button" :aria-label="theme === 'dark' ? 'Switch to light mode' : 'Switch to dark mode'" @click="toggleTheme">
            <span class="theme-toggle-thumb" :class="{ shifted: theme === 'light' }">
              <svg v-if="theme === 'dark'" viewBox="0 0 24 24" aria-hidden="true"><path fill="currentColor" d="M21 15.18A9.72 9.72 0 0 1 8.82 3a9.72 9.72 0 1 0 12.36 12.36A9.8 9.8 0 0 1 21 15.18Z"/></svg>
              <svg v-else viewBox="0 0 24 24" aria-hidden="true"><path fill="currentColor" d="M12 18a6 6 0 1 0 0-12 6 6 0 0 0 0 12Zm0-16a1 1 0 0 1 1 1v1a1 1 0 1 1-2 0V3a1 1 0 0 1 1-1Zm0 18a1 1 0 0 1 1 1v1a1 1 0 1 1-2 0v-1a1 1 0 0 1 1-1ZM4.22 5.64a1 1 0 0 1 1.42-1.42l.7.7a1 1 0 0 1-1.42 1.42l-.7-.7Zm13.44 13.44a1 1 0 0 1 1.42-1.42l.7.7a1 1 0 0 1-1.42 1.42l-.7-.7ZM2 12a1 1 0 0 1 1-1h1a1 1 0 1 1 0 2H3a1 1 0 0 1-1-1Zm18 0a1 1 0 0 1 1-1h1a1 1 0 0 1 0 2h-1a1 1 0 0 1-1-1ZM4.22 18.36l.7-.7a1 1 0 0 1 1.42 1.42l-.7.7a1 1 0 0 1-1.42-1.42Zm13.44-12.02.7-.7a1 1 0 0 1 1.42 1.42l-.7.7a1 1 0 0 1-1.42-1.42Z"/></svg>
            </span>
          </button>
        </div>
      </div>
    </header>

    <main class="page-container">
      <router-view v-slot="{ Component }">
        <Transition name="page" mode="out-in">
          <component :is="Component" />
        </Transition>
      </router-view>
    </main>

    <footer class="mobile-dock glass-panel">
      <router-link v-for="item in navItems" :key="item.path" :to="item.path">
        <span class="dock-icon" aria-hidden="true">
          <svg v-if="item.icon === 'home'" viewBox="0 0 24 24"><path d="m3 10 9-7 9 7v10a1 1 0 0 1-1 1h-5v-6H9v6H4a1 1 0 0 1-1-1V10Z"/></svg>
          <svg v-else-if="item.icon === 'about'" viewBox="0 0 24 24"><circle cx="12" cy="8" r="3"/><path d="M5 21a7 7 0 0 1 14 0"/></svg>
          <svg v-else-if="item.icon === 'portfolio'" viewBox="0 0 24 24"><rect x="3" y="5" width="18" height="14" rx="3"/><path d="M8 5V3h8v2M3 11h18M10 11v2h4v-2"/></svg>
          <svg v-else viewBox="0 0 24 24"><path d="M4 5h16v14H4z"/><path d="m4 6 8 6 8-6M8 16h.01M12 16h.01M16 16h.01"/></svg>
        </span>
        <span>{{ item.label }}</span>
      </router-link>
    </footer>
  </div>
</template>

<script>
export default {
  data() {
    return {
      theme: 'dark',
      navItems: [
        { path: '/', label: 'Home', icon: 'home' },
        { path: '/about', label: 'About', icon: 'about' },
        { path: '/portfolio', label: 'Work', icon: 'portfolio' },
        { path: '/contact', label: 'Contact', icon: 'contact' },
      ],
    };
  },
  mounted() {
    const savedTheme = window.localStorage.getItem('rio-theme');
    this.theme = savedTheme || (window.matchMedia('(prefers-color-scheme: light)').matches ? 'light' : 'dark');
    document.documentElement.style.colorScheme = this.theme;
  },
  watch: {
    theme(value) {
      document.documentElement.style.colorScheme = value;
      window.localStorage.setItem('rio-theme', value);
    },
  },
  methods: {
    redirectToHome() {
      this.$router.push('/');
    },
    toggleTheme() {
      this.theme = this.theme === 'dark' ? 'light' : 'dark';
    },
  },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap');

:root {
  color-scheme: dark;
  --bg: #0c0d0f; --bg-soft: #151619; --surface: rgba(28, 29, 31, .62); --surface-strong: rgba(35, 36, 38, .86); --surface-hover: rgba(52, 52, 53, .8); --line: rgba(255, 255, 255, .12); --line-strong: rgba(255, 219, 112, .4); --text: #f5f5f3; --muted: #a7a7a2; --muted-strong: #deded9; --accent: #ffdb70; --accent-2: #ffe9a9; --accent-ink: #201b0d; --shadow: 0 18px 50px rgba(0, 0, 0, .24);
}
.theme-light { --bg: #f2f0ea; --bg-soft: #e7e3d9; --surface: rgba(255, 255, 255, .58); --surface-strong: rgba(255, 255, 255, .84); --surface-hover: rgba(255, 255, 255, .92); --line: rgba(36, 35, 31, .14); --line-strong: rgba(168, 126, 23, .4); --text: #25241f; --muted: #77746b; --muted-strong: #4a4840; --accent: #b17b00; --accent-2: #e2b739; --accent-ink: #fff; --shadow: 0 18px 50px rgba(83, 75, 50, .13); }
*, *::before, *::after { box-sizing: border-box; }
html { scroll-behavior: smooth; }
body { margin: 0; min-width: 320px; background: var(--bg); color: var(--text); font-family: 'Poppins', sans-serif; -webkit-font-smoothing: antialiased; }
button, input, textarea { font: inherit; } button, a { -webkit-tap-highlight-color: transparent; } a { color: inherit; text-decoration: none; }
.app-shell { position: relative; min-height: 100vh; overflow: hidden; isolation: isolate; background: var(--bg); transition: background .55s ease, color .55s ease; }
.app-shell::before { content: ''; position: fixed; z-index: -2; inset: 0; pointer-events: none; opacity: .38; background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 160 160' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='.03'/%3E%3C/svg%3E"); }
.ambient { position: fixed; z-index: -1; width: 35rem; height: 35rem; border-radius: 50%; filter: blur(80px); pointer-events: none; opacity: .2; transition: opacity .55s ease, transform .8s ease; }
.ambient-one { top: 8rem; left: -22rem; background: #ffdb70; } .ambient-two { right: -22rem; bottom: -6rem; background: #b99645; } .theme-light .ambient { opacity: .1; }
.glass-panel { background: var(--surface); border: 1px solid var(--line); box-shadow: var(--shadow); backdrop-filter: blur(24px) saturate(145%); -webkit-backdrop-filter: blur(24px) saturate(145%); }
.site-header { position: sticky; z-index: 20; top: 1rem; width: min(1120px, calc(100% - 2rem)); margin: 1rem auto 0; border-radius: 22px; }
.nav-inner { display: flex; align-items: center; justify-content: space-between; gap: 1rem; min-height: 68px; padding: .65rem .7rem .65rem 1rem; }
.brand { display: inline-flex; align-items: center; gap: .6rem; padding: 0; border: 0; color: var(--text); background: transparent; cursor: pointer; font-family: 'Poppins', sans-serif; font-weight: 600; letter-spacing: -.04em; font-size: .96rem; }
.brand-mark { display: grid; place-items: center; width: 32px; height: 32px; border-radius: 11px; color: #17140c; background: var(--accent); box-shadow: 0 6px 17px rgba(255, 219, 112, .18); font-size: .9rem; }
.brand-caret { color: var(--accent); } .desktop-nav { display: flex; align-items: center; gap: .2rem; padding: .25rem; border: 1px solid var(--line); border-radius: 15px; background: rgba(0, 0, 0, .08); }
.desktop-nav a { padding: .55rem .9rem; border-radius: 11px; color: var(--muted); font-size: .82rem; font-weight: 600; transition: color .3s ease, background .3s ease, transform .3s ease; }
.desktop-nav a:hover { color: var(--text); transform: translateY(-1px); } .desktop-nav a.router-link-exact-active { color: var(--text); background: var(--surface-hover); box-shadow: 0 3px 12px rgba(0,0,0,.08); }
.header-actions { display: flex; align-items: center; gap: .55rem; } .icon-button, .theme-toggle { display: grid; place-items: center; border: 1px solid var(--line); background: rgba(255,255,255,.04); color: var(--muted-strong); cursor: pointer; transition: transform .3s ease, border-color .3s ease, background .3s ease; }
.icon-button:hover, .theme-toggle:hover { transform: translateY(-2px); border-color: var(--line-strong); background: var(--surface-hover); } .icon-button { width: 37px; height: 37px; border-radius: 12px; } .icon-button svg { width: 17px; height: 17px; }
.theme-toggle { position: relative; width: 50px; height: 29px; padding: 3px; border-radius: 99px; justify-content: flex-start; background: rgba(0,0,0,.18); } .theme-toggle-thumb { display: grid; place-items: center; width: 21px; height: 21px; border-radius: 50%; color: #201b0d; background: var(--accent); box-shadow: 0 3px 9px rgba(0,0,0,.24); transition: transform .38s cubic-bezier(.2,.8,.2,1); } .theme-toggle-thumb.shifted { transform: translateX(21px); color: #a06d00; background: #fff; } .theme-toggle-thumb svg { width: 13px; height: 13px; }
.page-container { position: relative; z-index: 1; width: min(1120px, calc(100% - 2rem)); min-height: calc(100vh - 120px); margin: 0 auto; padding: 3.5rem 0 4rem; }
.page-enter-active, .page-leave-active { transition: opacity .35s ease, transform .35s ease; } .page-enter-from { opacity: 0; transform: translateY(14px); } .page-leave-to { opacity: 0; transform: translateY(-10px); } .mobile-dock { display: none; }
@media (max-width: 700px) { .site-header { top: .65rem; margin-top: .65rem; border-radius: 20px; } .nav-inner { min-height: 61px; padding-left: .8rem; } .desktop-nav, .github-button { display: none; } .page-container { width: min(100% - 1.2rem, 560px); padding-top: 2rem; padding-bottom: 6rem; } .mobile-dock { position: fixed; z-index: 30; display: grid; grid-template-columns: repeat(4, 1fr); right: .65rem; bottom: .65rem; left: .65rem; padding: .45rem; border-radius: 22px; } .mobile-dock a { display: flex; flex-direction: column; align-items: center; gap: .2rem; padding: .45rem .25rem; border-radius: 16px; color: var(--muted); font-size: .64rem; font-weight: 600; transition: color .3s ease, background .3s ease, transform .3s ease; } .mobile-dock a.router-link-exact-active { color: var(--text); background: var(--surface-hover); } .dock-icon svg { width: 18px; height: 18px; fill: none; stroke: currentColor; stroke-width: 1.8; stroke-linecap: round; stroke-linejoin: round; } }
@media (prefers-reduced-motion: reduce) { *, *::before, *::after { scroll-behavior: auto !important; animation-duration: .01ms !important; transition-duration: .01ms !important; } }
</style>
