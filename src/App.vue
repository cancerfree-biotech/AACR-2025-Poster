<template>
  <div id="app">
    <!-- Header Section -->
    <header class="app-header">
      <a href="https://cancerfree.io/en/" target="_blank" rel="noopener">
        <img src="./assets/cancerfree.svg" alt="Logo" class="logo" />
      </a>
      <!-- 手機版：Logo 下顯示社群媒體 icon -->

      <div class="social-media-icons mobile-social">
        <a href="https://www.facebook.com/CancerFreeBiotech/" target="_blank" rel="noopener">
          <img src="./assets/icons8-facebook.svg" alt="Facebook" />

        </a>
        <a href="https://x.com/CancerFreeBio" target="_blank" rel="noopener">
          <img src="./assets/icons8-x.svg" alt="X" />
        </a>
        <a href="https://www.linkedin.com/company/cancerfree-biotech/" target="_blank" rel="noopener">
          <img src="./assets/icons8-linkedin.svg" alt="LinkedIn" />
        </a>
        <a href="https://youtube.com/channel/UC4F1TrhBWThR8IPivO01llw?si=Ea50mPMsy4-QZmAy" target="_blank" rel="noopener">
          <img src="./assets/icons8-youtube.svg" alt="Youtube" />
        </a>
      </div>
      <h1 class="desktop-title">CancerFree AACR 2025</h1>
      <h1 class="mobile-title">CancerFree AACR 2025</h1>
      <!-- 桌機版：Title 下顯示社群媒體 icon -->
      <div class="social-media-icons desktop-social">
        <a href="https://www.facebook.com/CancerFreeBiotech/" target="_blank" rel="noopener">
          <img src="./assets/icons8-facebook.svg" alt="Facebook" />

        </a>
        <a href="https://x.com/CancerFreeBio" target="_blank" rel="noopener">
          <img src="./assets/icons8-x.svg" alt="X" />
        </a>
        <a href="https://www.linkedin.com/company/cancerfree-biotech/" target="_blank" rel="noopener">
          <img src="./assets/icons8-linkedin.svg" alt="LinkedIn" />
        </a>
        <a href="https://youtube.com/channel/UC4F1TrhBWThR8IPivO01llw?si=Ea50mPMsy4-QZmAy" target="_blank" rel="noopener">
          <img src="./assets/icons8-youtube.svg" alt="Youtube" />
        </a>
      </div>
    </header>

    <!-- Poster Images with Caption Above -->
    <p class="attention">Click poster image or contact us to get more detail.</p>    <div class="poster-container">
      <div class="poster-wrapper" v-for="(poster, index) in posters" :key="index">
        <!-- 新增：永遠顯示的資訊區塊，位於圖片前面 -->
        <div class="poster-caption">
          <p>{{ poster.title }}</p>
        </div>
        <img :src="poster.src" alt="Poster" class="poster-image" @click="openPoster(poster)" />
        <!-- 如果你還想保留鼠標懸浮資訊，這區域可以保留或移除 -->
        <div class="poster-info" v-if="hoveringPoster === index">
          <p>{{ poster.title }}</p>
        </div>
      </div>
    </div>

    <!-- Fullscreen Poster Modal -->
    <div v-if="activePoster" class="modal-overlay" @click="activePoster = null">
      <div class="modal-content" @click.stop>
        <img :src="activePoster.src" alt="Poster" class="fullscreen-poster" />
        <div class="poster-details">

          <h4>{{ activePoster.title }}</h4>
          <p style="margin: 0;text-align: left;"><strong>ID:</strong> {{ activePoster.id }}</p>
          <p style="margin: 0;text-align: left;"><strong>Location:</strong> {{ activePoster.location }}</p>
          <p style="margin: 0;text-align: left;"><strong>Board:</strong> {{ activePoster.board }}</p>
          
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
          to get full posters.
        </span>
      </div>
    </div>

    <!-- Contact Us Modal -->
    <div v-if="showContactForm" class="modal-overlay">
      <div class="modal scrollable">
        <form @submit.prevent="submitContactInfo">
          <!-- 表單內容 -->
          <label for="name">Name:</label>
          <input type="text" id="name" v-model="contactInfo.name" required />

          <label for="email">Email:</label>
          <input type="email" id="email" v-model="contactInfo.email" required />

          <label for="company">Company:</label>
          <input type="text" id="company" v-model="contactInfo.company" />

          <label for="title">Title:</label>
          <input type="text" id="title" v-model="contactInfo.title" />

          <label>What brings you here today?<br />
            Select all that apply and we’ll get back to you with relevant information:
          </label>
          <div class="checkbox-group">
            <div v-for="(option, index) in reasonsOptions" :key="index">
              <label>
                <input type="checkbox" :value="option" v-model="contactInfo.reasons" />
                {{ option }}
              </label>
            </div>
          </div>

          <label for="message">Please let us know if you have any questions.</label>
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
          details: "An Automated Approach for Real-time Monitoring of CTC Tumoroid Development.",
          title: "An Automated Approach for Real-time Monitoring of CTC Tumoroid Development",
          id: "#9830 LB109",
          location: "Poster Section 51",
          board: "Board 10"

        },
        {
          src: "/posters/LAB-poster-AACR2025-250408-3-010_1.png",
          info: "Translational platform overview. Click to view details.",
          details: "A translational circulating tumor cell platform integrating drug sensitivity screening and genomic profiling: insights from Antrodia cinnamomea in high-TMB brain tumors.",
          title: "A translational circulating tumor cell platform integrating drug sensitivity screening and genomic profiling: insights from Antrodia cinnamomea in high-TMB brain tumors",
          id: "1993",
          location: "Poster Section 29",
          board: "Board 28"
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
        message: '',
        reasons: []
      },
      reasonsOptions: [
        "I'm interested in learning more about your technology",
        "I'm exploring potential collaboration opportunities",
        "I'm a customer/user with a question (please include the question below)",
        "I'm interested in partnership or investment",
        "Just curious / browsing"
      ],
      webhookUrl: import.meta.env.VITE_WEBHOOK_URL,
      webhookSecret: import.meta.env.VITE_WEBHOOK_SECRET
    };
  },
  watch: {
    showContactForm(newVal) {
      document.body.style.overflow = newVal ? 'hidden' : 'auto';
    }
  },
  methods: {
    openPoster(poster) {
      this.activePoster = poster;
    },
    async submitContactInfo() {
      try {
        const payload = {
          ...this.contactInfo,
          reasons: Array.isArray(this.contactInfo.reasons)
            ? this.contactInfo.reasons.join(', ')
            : this.contactInfo.reasons
        };
        const response = await fetch(this.webhookUrl, {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
            'x-webhook-secret': this.webhookSecret, // 傳入 webhook secret 供 n8n 驗證
            'source-url': window.location.href // 傳入來源網址
          },
          body: JSON.stringify(payload)
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