<template>
  <div class="container mx-auto p-3 md:p-8">
    <div class="flex flex-col-reverse md:flex-row relative">
      <div class="w-full md:w-2/3">
        <div class="flex flex-col gap-4 md:px-20 fade-zoom-up">
          <div class="bg-[#1e1e1f] border border-[#383838] rounded-xl p-6 md:p-10 text-white">
            <h2 class="text-2xl font-bold text-amber-200 mb-6">Send Me a Message</h2>
            
<form 
    action="https://formspree.io/f/mwvwrbev" 
    method="POST" 
    @submit.prevent="sendMessage" 
    class="space-y-4"
  >
    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
      <div>
        <label class="text-xs text-slate-400 block mb-2">Full Name</label>
        <input type="text" name="name" v-model="form.name" required placeholder="Your Name" 
          class="w-full bg-[#282828] border border-[#383838] rounded-lg p-3 focus:outline-none focus:border-amber-200 text-sm text-white">
      </div>
      <div>
        <label class="text-xs text-slate-400 block mb-2">Email Address</label>
        <input type="email" name="email" v-model="form.email" required placeholder="Email Address" 
          class="w-full bg-[#282828] border border-[#383838] rounded-lg p-3 focus:outline-none focus:border-amber-200 text-sm text-white">
      </div>
    </div>
    
    <div>
      <label class="text-xs text-slate-400 block mb-2">Message</label>
      <textarea name="message" v-model="form.message" required rows="5" placeholder="Your Message" 
        class="w-full bg-[#282828] border border-[#383838] rounded-lg p-3 focus:outline-none focus:border-amber-200 text-sm text-white"></textarea>
    </div>

    <button type="submit" :disabled="isSending"
      class="py-3 px-6 rounded-lg bg-amber-200 text-black font-bold text-sm hover:bg-amber-300 transition duration-300 w-full md:w-fit disabled:opacity-50">
      {{ isSending ? 'Sending...' : 'Send Message' }}
    </button>
  </form>
          </div>
        </div>
      </div>

      <div class="w-full md:w-1/3 h-fit p-8 md:sticky md:top-24">
        <div class="flex flex-col text-left">
          <div class="bg-clip-text bg-gradient-to-r from-slate-100 to-amber-300 text-transparent font-semibold text-lg">
            Let's build something great together.
          </div>
          <p class="text-slate-400 text-sm mt-2">I'm open for collaboration, freelance projects, or just a friendly chat about tech.</p>
          
          <div class="h-[1px] mt-7 mb-7 w-20 bg-amber-200"></div>
          
          <div class="space-y-4">
            <div class="text-white text-md font-semibold">Contact Info</div>
            
            <div class="flex items-center gap-3 text-slate-300 hover:text-amber-200 cursor-pointer text-sm">
              <span class="p-2 bg-[#1e1e1f] rounded-lg">📧</span>
              <span>rioadrianaaa12@gmail.com</span>
            </div>
            
            <div class="flex items-center gap-3 text-slate-300 hover:text-amber-200 cursor-pointer text-sm">
              <span class="p-2 bg-[#1e1e1f] rounded-lg">📍</span>
              <span>Bandung, Indonesia</span>
            </div>

            <div class="h-[1px] mt-7 mb-7 w-20 bg-amber-200"></div>
            
            <div class="text-white text-md font-semibold">Socials</div>
            <div class="mt-3 flex flex-wrap gap-2">
              <a href="https://github.com/RioAdrna" target="_blank" class="py-2 px-3 rounded-2xl bg-[#1e1e1f] hover:bg-white/20 text-white text-xs">GitHub</a>
              <a href="https://www.linkedin.com/in/rio-adriana-548416215/" target="_blank" class="py-2 px-3 rounded-2xl bg-[#1e1e1f] hover:bg-white/20 text-white text-xs">LinkedIn</a>
              <a href="https://www.instagram.com/rdrnnn._/" target="_blank" class="py-2 px-3 rounded-2xl bg-[#1e1e1f] hover:bg-white/20 text-white text-xs">Instagram</a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import Swal from 'sweetalert2';

export default {
  data() {
    return {
      isSending: false,
      form: {
        name: '',
        email: '',
        message: ''
      }
    }
  },
  methods: {
    async sendMessage() {
      this.isSending = true;

      Swal.fire({
        title: 'Sending Message...',
        text: 'Please wait a moment',
        background: '#1e1e1f',
        color: '#fff',
        allowOutsideClick: false,
        showConfirmButton: false,
        didOpen: () => {
          Swal.showLoading();
          const loader = Swal.getHtmlContainer().querySelector('.swal2-loader');
          if (loader) loader.style.borderTopColor = '#fde68a'; // Warna amber-200
        }
      });

      try {
        const response = await axios.post('https://formspree.io/f/mwvwrbev', this.form);
        
        if (response.status === 200) {
         Swal.fire({
  title: 'Success!',
  text: `Terima kasih ${this.form.name}, pesan kamu sudah Rio terima.`,
  icon: 'success',
  background: '#1e1e1f',
  color: '#fff',
  iconColor: '#fde68a',
  confirmButtonText: 'Ok!',
  buttonsStyling: true, 
  customClass: {
    popup: 'rounded-xl border border-[#383838]',
    confirmButton: 'custom-swal-button' 
  },
  didOpen: () => {
    const confirmButton = Swal.getConfirmButton();
    confirmButton.style.backgroundColor = '#fde68a';
    confirmButton.style.color = '#000';
    confirmButton.style.fontWeight = 'bold';
    confirmButton.style.border = 'none';
    confirmButton.style.boxShadow = 'none'; 
  }
});
          this.form = { name: '', email: '', message: '' };
        }
      } catch (error) {
        Swal.fire({
          title: 'Error!',
          text: 'Gagal mengirim pesan. Coba lagi nanti ya.',
          icon: 'error',
          background: '#1e1e1f',
          color: '#fff',
          confirmButtonColor: '#ef4444',
        });
      } finally {
        this.isSending = false;
      }
    }
  }
}
</script>

<style scoped>
@keyframes fadeZoomUp {
  0% {
    opacity: 0;
    transform: scale(0.95) translateY(20px);
  }
  100% {
    opacity: 1;
    transform: scale(1) translateY(0);
  }
}
.fade-zoom-up {
  animation: fadeZoomUp 0.8s ease-out;
}
</style>
