<template>
  <div id="app">
    <!-- Header Section -->
    <header class="app-header">
      <img src="./assets/cancerfree.svg" alt="Logo" class="logo" />
      <h1>CancerFree AACR 2025 Poster</h1>
    </header>

    <!-- Poster Image with Hover Info -->
    <div class="poster-container">
      <div class="poster-wrapper">
        <img 
          src="/posters/IT-CF-Justin_AACR2025_poster-250304-4-010_1.png" 
          alt="Poster" 
          class="poster-image" 
          @click="showPoster = true"
        />
        <div class="poster-info" v-if="hoveringPoster">
          <p>This is the CancerFree AACR 2025 Poster. Click to enlarge!</p>
        </div>
      </div>
    </div>

    <!-- Fullscreen Poster Modal -->
    <!-- Fullscreen Poster Modal with Info -->
    <div v-if="showPoster" class="modal-overlay" @click="showPoster = false">
      <div class="modal-content" @click.stop>
        <img 
          src="/posters/IT-CF-Justin_AACR2025_poster-250304-4-010_1.png" 
          alt="Poster" 
          class="fullscreen-poster"
        />
        <div class="poster-details">
          <h3>Poster Information</h3>
          <p>This poster showcases the latest research and findings for CancerFree AACR 2025. Click anywhere outside the poster to close.</p>
        </div>
      </div>
    </div>


    <!-- Contact Us Button with Arrow and Text -->
        <div class="contact-us-container">
      <button @click="showContactForm = true" class="contact-button">Contact Us</button>
      <div class="contact-us-arrow">
        <span class="bounce-text">Contact us to get more information</span>
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
          <textarea id="message" v-model="contactInfo.message" required></textarea>

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
      showPoster: false,
      hoveringPoster: false,
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
    async submitContactInfo() {
      try {
        const response = await fetch(this.webhookUrl,{
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
            'x-webhook-secret': this.webhookSecret, // 傳入 webhook secret 供 n8n 驗證
            'source-url': window.location.href // 傳入來源網址
          },
          body: JSON.stringify(this.contactInfo)
        });

        // 解析回傳的 JSON
        const result = await response.json();
        
        if (result.message === "Workflow was started") {
          alert('Contact information submitted successfully!');
          this.showContactForm = false;
        } else {
          // 如果不是預期訊息，可以做額外處理
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

