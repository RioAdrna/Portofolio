<template>
  <main class="article-page">
    <div v-if="isLoading" class="state-card glass-panel">Loading article<span class="loading-dots">...</span></div>
    <div v-else-if="error" class="state-card glass-panel error-state">{{ error }}</div>
    <article v-else class="article-card glass-panel"><router-link to="/portfolio" class="back-link"><svg class="arrow-icon" viewBox="0 0 16 16" aria-hidden="true"><path d="M13 8H3M7 4l-4 4 4 4" /></svg> Back to work</router-link><p class="article-date">{{ date }}</p><h1>{{ title }}</h1><div class="article-image"><img :src="image" alt="Article thumbnail" /></div><div class="article-content" v-html="content"></div></article>
  </main>
</template>

<script>
import axios from 'axios';
export default { name: 'ArticleView', data() { return { title: '', image: '', date: '', content: '', isLoading: true, error: '' }; }, mounted() { this.getDetails(); }, methods: { async getDetails() { try { const { id } = this.$route.params; const response = await axios.get(`https://64a38c9cc3b509573b564183.mockapi.io/api/blog/all/${encodeURIComponent(id)}`); this.title = response.data.title; this.image = response.data.image; this.date = response.data.date; this.content = response.data.content; } catch (error) { this.error = 'This article could not be loaded. Please try again.'; } finally { this.isLoading = false; } } } };
</script>

<style scoped>
.article-page { max-width: 820px; margin: 0 auto; padding: 1rem 0 3rem; }.state-card { padding: 3rem; border-radius: 22px; color: var(--muted); text-align: center; }.error-state { color: #ff9aab; }.loading-dots { color: var(--accent); animation: dots 1.2s infinite; }.article-card { padding: clamp(1.2rem, 4vw, 3rem); border-radius: 28px; text-align: left; }.back-link { display: inline-block; margin-bottom: 2.5rem; color: var(--accent); font-size: .75rem; font-weight: 700; transition: transform .3s ease; }.back-link:hover { transform: translateX(-4px); }.article-label { margin: 0 0 .65rem; color: var(--muted); font-size: .7rem; letter-spacing: .1em; text-transform: uppercase; }.article-card h1 { max-width: 100%; font-size: clamp(2rem, 5vw, 4rem); line-height: 1.05; }.article-line { width: 100px; height: 3px; margin: 1.5rem 0 2rem; border-radius: 99px; background: linear-gradient(90deg, var(--accent), var(--accent-2)); }.article-image { overflow: hidden; aspect-ratio: 1.8; border-radius: 18px; background: var(--surface-strong); }.article-image img { width: 100%; height: 100%; object-fit: cover; }.article-content { margin-top: 2rem; color: var(--muted-strong); line-height: 1.85; }.article-content :deep(h2), .article-content :deep(h3) { color: var(--text); font-family: 'Plus Jakarta Sans'; }.article-content :deep(a) { color: var(--accent); }.article-content :deep(img) { max-width: 100%; border-radius: 15px; } @keyframes dots { 50% { opacity: .2; } }
</style>
