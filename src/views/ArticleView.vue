<template>
  <main class="w-full md:w-3/5 mx-auto md:mt-5 pb-24">
    <div v-if="isLoading" class="p-10 text-center text-white">Loading article...</div>
    <div v-else-if="error" class="p-10 text-center text-red-300">{{ error }}</div>
    <article v-else class="bg-white rounded-xl mx-3 p-5 md:p-10">
      <h1 class="text-xl md:text-4xl text-black text-left font-bold leading-relaxed">{{ title }}</h1>
      <div class="mt-3 text-left text-gray-800 text-sm">Published at <span>{{ date }}</span></div>
      <div class="h-[2px] w-20 my-5 md:my-10 bg-[#ffdb70] md:w-1/3"></div>
      <div class="relative w-full" style="padding-top: 50%;">
        <img :src="image" class="absolute top-0 left-0 rounded-lg w-full h-full object-cover" alt="Article thumbnail">
      </div>
      <div class="text-left text-black mt-8" v-html="content"></div>
    </article>
  </main>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      title: '', image: '', date: '', content: '', isLoading: true, error: ''
    };
  },
  mounted() {
    this.getDetails();
  },
  methods: {
    async getDetails() {
      try {
        const { id } = this.$route.params;
        const response = await axios.get(`https://64a38c9cc3b509573b564183.mockapi.io/api/blog/all/${encodeURIComponent(id)}`);
        this.title = response.data.title;
        this.image = response.data.image;
        this.date = response.data.date;
        this.content = response.data.content;
      } catch (error) {
        this.error = 'Artikel tidak dapat dimuat. Silakan coba lagi.';
      } finally {
        this.isLoading = false;
      }
    }
  }
};
</script>