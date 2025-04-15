<template>
  <div id="app">
    <!-- Header Section -->
    <header class="app-header">
      <a href="https://cancerfree.io/en/" target="_blank" rel="noopener">
        <img src="./assets/cancerfree.svg" alt="Logo" class="logo" />
      </a>
      <h1 class="desktop-title">CancerFree AACR 2025 Poster</h1>
      <h2 class="mobile-title">CancerFree <br> AACR 2025 Poster</h2>
    </header>

    <!-- Poster Images with Hover Info -->
    <div class="poster-container">
      <div 
        class="poster-wrapper" 
        v-for="(poster, index) in posters" 
        :key="index"
        @mouseover="hoveringPoster = index"
        @mouseleave="hoveringPoster = null"
      >
        <img 
          :src="poster.src" 
          alt="Poster" 
          class="poster-image" 
          @click="openPoster(poster)"
        />
        <div class="poster-info" v-if="hoveringPoster === index">
          <p>{{ poster.info }}</p>
        </div>
      </div>
    </div>

    <!-- Fullscreen Poster Modal -->
    <div v-if="activePoster" class="modal-overlay" @click="activePoster = null">
      <div class="modal-content" @click.stop>
        <img 
          :src="activePoster.src" 
          alt="Poster" 
          class="fullscreen-poster"
        />
        <div class="poster-details">
          <h3>Poster Information</h3>
          <p>{{ activePoster.details }}</p>
        </div>
      </div>
    </div>

    <!-- Contact Us Button with Arrow and Text -->
    <div class="contact-us-container">
      <button @click="showContactForm = true" class="contact-button">Contact Us</button>
      <div class="contact-us-arrow">
        <span class="bounce-text">
          Contact us
          <span class="mobile-break"></span>
          to get more information
        </span>
      </div>
    </div>

    <!-- Contact Us Modal -->
    <div v-if="showContactForm" class="modal-overlay">
      <div class="modal">
        <form @submit.prevent="submitContactInfo">
          <label for="name">Name:</label>
          <input type="text" id="name" v-model="contactInfo.name" required />

          <label for="email">Email:</label>
          <input type="email" id="email" v-model="contactInfo.email" required />

          <label for="company">Company:</label>
          <input type="text" id="company" v-model="contactInfo.company" />

          <label for="title">Title:</label>
          <input type="text" id="title" v-model="contactInfo.title" />

          <label for="message">Message:</label>
          <textarea id="message" v-model="contactInfo.message"></textarea>

          <button type="submit">Send</button>
          <button type="button" @click="showContactForm = false">Cancel</button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // 每一筆包含圖片來源、hover 時顯示的短描述 info 與展開後的詳細描述 details
      posters: [
        {
          src: "/posters/IT-CF-Justin_AACR2025_poster-250304-4-010_1.png",
          info: "Automated real-time monitoring.",
          details: "An Automated Approach for Real-time Monitoring of CTC Tumoroid Development. Click to view details."
        },
        {
          src: "/posters/LAB-poster-AACR2025-250408-3-010_1.png",
          info: "Translational platform overview. Click to view details.",
          details: "A translational circulating tumor cell platform integrating drug sensitivity screening and genomic profiling: insights from Antrodia cinnamomea in high-TMB brain tumors."
        }
      ],
      hoveringPoster: null,
      activePoster: null,
      showContactForm: false,
      contactInfo: {
        name: '',
        email: '',
        company: '',
        title: '',
        message: ''
      },
      webhookUrl: import.meta.env.VITE_WEBHOOK_URL,
      webhookSecret: import.meta.env.VITE_WEBHOOK_SECRET
    };
  },
  methods: {
    openPoster(poster) {
      this.activePoster = poster;
    },
    async submitContactInfo() {
      try {
        const response = await fetch(this.webhookUrl, {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
            'x-webhook-secret': this.webhookSecret, // 傳入 webhook secret 供 n8n 驗證
            'source-url': window.location.href // 傳入來源網址
          },
          body: JSON.stringify(this.contactInfo)
        });
        const result = await response.json();
        if (result.message === "Workflow was started") {
          alert('Contact information submitted successfully!');
          this.showContactForm = false;
        } else {
          alert('Unexpected response: ' + result.message);
        }
      } catch (error) {
        console.error('Error submitting contact info:', error);
        alert('Failed to submit contact information.');
      }
    }
  }
};
</script>