<template>
  <div id="app">
    <!-- Header Section -->
    <header class="app-header">
      <a href="https://www.cancerfree.io" target="_blank">
        <img src="/src/assets/cancerfree.svg" alt="Logo" class="logo" />
      </a>
      <h1>CancerFree AACR 2025 Poster</h1>
    </header>

    <!-- Poster Image with Hover Info -->
    <div class="poster-container">
      <div 
        class="poster-wrapper" 
        @mouseover="hoveringPoster = true" 
        @mouseleave="hoveringPoster = false"
      >
        <img 
          src="/public/posters/IT-CF-Justin_AACR2025_poster-250304-4-010_1.png" 
          alt="Poster" 
          class="poster-image" 
          @click="showPoster = true"
        />
        <div class="poster-info" v-if="hoveringPoster">
          <p>This is the CancerFree AACR 2025 Poster. Click to enlarge!</p>
        </div>
      </div>
    </div>

    <!-- Fullscreen Poster Modal with Info -->
    <div v-if="showPoster" class="modal-overlay" @click="showPoster = false">
      <div class="modal-content" @click.stop>
        <img 
          src="/public/posters/IT-CF-Justin_AACR2025_poster-250304-4-010_1.png" 
          alt="Poster" 
          class="fullscreen-poster"
        />
        <div class="poster-details">
          <h3>Poster Information</h3>
          <p><strong>Title:</strong>An Automated Approach for Real-time Monitoring of CTC Tumoroid Development</p>
        </div>
      </div>
    </div>

    <!-- Contact Us Button with Arrow and Text -->
    <div class="contact-us-container">
      <button @click="showContactForm = true" class="contact-button">Contact Us</button>
      <div class="bounce-text">Contact us to get more information</div>
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
      webhookUrl: process.env.WEBHOOK_URL,
      webhookSecret: process.env.WEBHOOK_SECRET
    };
  },
  methods: {
    async submitContactInfo() {
      try {
        const response = await fetch(this.webhookUrl, {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
            'x-webhook-secret': this.webhookSecret
          },
          body: JSON.stringify(this.contactInfo)
        });

        if (!response.ok) {
          throw new Error('Failed to submit contact information');
        }

        alert('Contact information submitted successfully!');
        this.showContactForm = false;
      } catch (error) {
        console.error('Error submitting contact info:', error);
        alert('Failed to submit contact information.');
      }
    }
  }
};
</script>

<style>
/* Removed all styles from App.vue as they are now in style.css */
</style>
