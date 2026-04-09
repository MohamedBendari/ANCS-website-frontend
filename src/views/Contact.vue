
<template>
  <section class="contact">
    <div class="container">
      <div class="contact-header">
        <h1>Get In Touch</h1>
        <p>Have questions? We'd love to hear from you. Send us a message and we'll respond as soon as possible.</p>
      </div>

      <div class="contact-content">
        <div class="contact-info">
          <div class="info-card">
            <i class="fas fa-envelope"></i>
            <h3>Email Us</h3>
            <a href="mailto:mohamedsharkawy078@gmail.com" target="_blank">
              mohamedsharkawy078@gmail.com
            </a>
          </div>

          <div class="info-card">
            <i class="fas fa-map-marker-alt"></i>
            <h3>Location</h3>
            <a href="https://maps.google.com" target="_blank">
              Cairo, Egypt
            </a>
          </div>

          <div class="info-card">
            <i class="fab fa-github"></i>
            <h3>GitHub</h3>
            <a href="https://github.com/MohamedBendari" target="_blank">
              github.com/MohamedBendari
            </a>
          </div>
        </div>

        <form class="contact-form" @submit.prevent="submitForm">
          <div class="form-group">
            <label for="name">Your Name</label>
            <input
              type="text"
              id="name"
              v-model="form.name"
              placeholder="John Doe"
              required
              :disabled="isSubmitting"
            >
          </div>

          <div class="form-group">
            <label for="email">Email Address</label>
            <input
              type="email"
              id="email"
              v-model="form.email"
              placeholder="john@example.com"
              required
              :disabled="isSubmitting"
            >
          </div>

          <div class="form-group">
            <label for="purpose">Inquiry Purpose</label>
            <select id="purpose" v-model="form.purpose" required :disabled="isSubmitting">
             <option disabled value="">Select purpose</option>
             <option value="buy">Buy ANCS</option>
             <option value="support">Technical Support</option>
             <option value="question">General Question</option>
             <option value="partnership">Partnership</option>
            </select>
          </div>

          <div class="form-group">
            <label for="message">Message</label>
            <textarea
              id="message"
              v-model="form.message"
              placeholder="Tell us more about your inquiry..."
              rows="5"
              required
              :disabled="isSubmitting"
            ></textarea>
          </div>

          <button type="submit" class="submit-btn" :disabled="isSubmitting">
            <span v-if="!isSubmitting">
              <i class="fas fa-paper-plane"></i>
              Send Message
            </span>
            <span v-else class="loading">
              <i class="fas fa-spinner fa-spin"></i>
              Sending...
            </span>
          </button>

          <div v-if="submitStatus" class="status-message" :class="submitStatus.type">
            <i :class="submitStatus.icon"></i>
            {{ submitStatus.message }}
          </div>
        </form>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref } from 'vue'

const form = ref({
  name: '',
  email: '',
  purpose: '',
  message: ''
})

const isSubmitting = ref(false)
const submitStatus = ref(null)

const submitForm = async () => {
  isSubmitting.value = true
  submitStatus.value = null

  try {
    const response = await fetch('http://127.0.0.1:8000/api/messages/', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify(form.value)
})

    const data = await response.json()

    if (response.ok) {
      submitStatus.value = {
        type: 'success',
        icon: 'fas fa-check-circle',
        message: 'Message sent successfully! We\'ll get back to you soon.'
      }
      // Reset form fields
      form.value = { name: '', email: '', purpose: '', message: '' }
    } else {
      throw new Error(data.message || 'Something went wrong')
    }
  } catch (error) {
    submitStatus.value = {
      type: 'error',
      icon: 'fas fa-exclamation-circle',
      message: error.message || 'Failed to send message. Please try again.'
    }
  } finally {
    isSubmitting.value = false
  }
}
</script>

<style scoped>
.contact {
  min-height: 100vh;
  padding: 120px 20px 60px;
  background: linear-gradient(135deg, #0b1c2c 0%, #16293d 100%);
}

.container {
  max-width: 1200px;
  margin: 0 auto;
}

.contact-header {
  text-align: center;
  margin-bottom: 60px;
}

.contact-header h1 {
  font-size: 48px;
  font-weight: 700;
  margin-bottom: 20px;
  background: linear-gradient(135deg, #ffffff, #42a5f5);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  color: #fff;
}

.contact-header p {
  font-size: 18px;
  color: rgba(255, 255, 255, 0.7);
  max-width: 600px;
  margin: 0 auto;
}

.contact-content {
  display: grid;
  grid-template-columns: 1fr 2fr;
  gap: 60px;
  align-items: start;
}

.contact-info {
  display: flex;
  flex-direction: column;
  gap: 25px;
}

.info-card {
  background: rgba(255, 255, 255, 0.05);
  padding: 30px;
  border-radius: 16px;
  border: 1px solid rgba(255, 255, 255, 0.1);
  transition: all 0.3s ease;
}

.info-card:hover {
  transform: translateY(-5px);
  background: rgba(255, 255, 255, 0.08);
  border-color: rgba(66, 165, 245, 0.3);
}

.info-card i {
  font-size: 32px;
  color: #42a5f5;
  margin-bottom: 15px;
}

.info-card h3 {
  font-size: 18px;
  font-weight: 600;
  margin-bottom: 8px;
  color: white;
}

.info-card a {
  font-size: 15px;
  color: rgba(255, 255, 255, 0.7);
  text-decoration: none;
  transition: color 0.3s ease;
}

.info-card a:hover {
  color: #42a5f5;
}

.contact-form {
  background: rgba(255, 255, 255, 0.05);
  padding: 40px;
  border-radius: 20px;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.form-group {
  margin-bottom: 25px;
}

.form-group label {
  display: block;
  margin-bottom: 8px;
  font-size: 14px;
  font-weight: 500;
  color: rgba(255, 255, 255, 0.9);
}

.form-group input,
.form-group select,
.form-group textarea {
  width: 100%;
  padding: 14px 18px;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 10px;
  color: white;
  font-size: 15px;
  transition: all 0.3s ease;
  font-family: inherit;
}

.form-group select option {
  background: #16293d;
  color: white;
}

.form-group input:focus,
.form-group select:focus,
.form-group textarea:focus {
  outline: none;
  border-color: #42a5f5;
  background: rgba(255, 255, 255, 0.08);
}

.submit-btn {
  width: 100%;
  padding: 16px;
  background: linear-gradient(135deg, #0077b6, #0096c7);
  color: white;
  border: none;
  border-radius: 10px;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
}

.submit-btn:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 8px 25px rgba(0, 119, 182, 0.4);
}

.submit-btn:disabled {
  opacity: 0.7;
  cursor: not-allowed;
}

.status-message {
  margin-top: 20px;
  padding: 16px;
  border-radius: 10px;
  display: flex;
  align-items: center;
  gap: 10px;
  font-size: 14px;
}

.status-message.success {
  background: rgba(34, 197, 94, 0.1);
  border: 1px solid rgba(34, 197, 94, 0.3);
  color: #22c55e;
}

.status-message.error {
  background: rgba(239, 68, 68, 0.1);
  border: 1px solid rgba(239, 68, 68, 0.3);
  color: #ef4444;
}

@media (max-width: 968px) {
  .contact-content { grid-template-columns: 1fr; }
  .contact-info { order: 2; flex-direction: row; flex-wrap: wrap; }
  .info-card { flex: 1; min-width: 250px; }
  .contact-form { order: 1; }
}

@media (max-width: 768px) {
  .contact-header h1 { font-size: 36px; }
  .contact-info { flex-direction: column; }
  .info-card { min-width: 100%; }
}

@media (max-width: 600px) {
  .contact {
    padding: 90px 20px 60px;
  }

  .contact-header {
    margin-bottom: 40px;
  }

  .contact-header h1 {
    font-size: 28px;
  }

  .contact-header p {
    font-size: 16px;
  }

  .contact-form {
    padding: 20px;
  }

  .form-group label {
    font-size: 13px;
  }

  .form-group input,
  .form-group textarea {
    font-size: 14px;
    padding: 10px;
  }

  .form-submit {
    padding: 12px 24px;
    font-size: 14px;
  }

  .info-card {
    padding: 16px;
  }

  .info-card h4 {
    font-size: 14px;
  }

  .info-card p {
    font-size: 12px;
  }
}
</style>
