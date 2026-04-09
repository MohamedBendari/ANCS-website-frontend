<template>
  <section class="download">
    <div class="container">
      <div class="section-header" data-aos="fade-up">
        <span class="subtitle">Get Started</span>
        <h1>Download ANCS</h1>
        <p>Choose your platform and start automating your network configuration today</p>
      </div>

      <div class="download-content">
        <!-- Main Download Card -->
        <div class="main-download" data-aos="fade-up" data-aos-delay="100">
          <div class="download-card featured">
            <div class="card-badge">Latest Release</div>
            <div class="platform-icon">
              <i class="fas fa-laptop-code"></i>
            </div>
            <h2>ANCS v1.0.0</h2>
            <p class="version-info">Stable Release • Released March 2026</p>

            <div class="system-requirements">
              <h4>System Requirements:</h4>
              <ul>
                <li><i class="fas fa-check"></i> Windows 10/11 or Linux Ubuntu 20.04+</li>
                <li><i class="fas fa-check"></i> 4GB RAM minimum (8GB recommended)</li>
                <li><i class="fas fa-check"></i> Python 3.8+ & Node.js 18+</li>
                <li><i class="fas fa-check"></i> GNS3 (optional but recommended)</li>
              </ul>
            </div>

            <div class="download-buttons">
              <!-- ✅ زرار التحميل بعد التعديل -->
              <a
                href="https://raw.githubusercontent.com/MOHAmED3467/ANCS-website/main/public/ANCS.exe"
                class="btn primary"
                download
              >
                <i class="fab fa-windows"></i>
                Download for Windows
              </a>

              <a href="#" class="btn secondary">
                <i class="fab fa-linux"></i>
                Download for Linux
              </a>
            </div>
          </div>
        </div>

        <!-- Alternative Downloads -->
        <div class="alt-downloads" data-aos="fade-up" data-aos-delay="200">
          <h3>Other Download Options</h3>

          <div class="alt-grid">
            <a href="https://github.com/MOHAmED3467/ANCS-website" target="_blank" class="alt-card">
              <div class="alt-icon">
                <i class="fab fa-github"></i>
              </div>
              <div class="alt-info">
                <h4>Source Code</h4>
                <p>Clone from GitHub repository</p>
              </div>
              <i class="fas fa-arrow-right"></i>
            </a>

            <div class="alt-card">
              <div class="alt-icon docker">
                <i class="fab fa-docker"></i>
              </div>
              <div class="alt-info">
                <h4>Docker Image</h4>
                <p>Coming Soon</p>
              </div>
              <span class="badge">Soon</span>
            </div>

            <div class="alt-card">
              <div class="alt-icon docs">
                <i class="fas fa-book"></i>
              </div>
              <div class="alt-info">
                <h4>Documentation</h4>
                <p>Setup guide & API reference</p>
              </div>
              <i class="fas fa-arrow-right"></i>
            </div>
          </div>
        </div>

        <!-- Installation Steps -->
        <div class="installation-guide" data-aos="fade-up" data-aos-delay="300">
          <h3>Quick Installation Guide</h3>

          <div class="steps">
            <div class="step">
              <div class="step-number">1</div>
              <div class="step-content">
                <h4>Download</h4>
                <p>Download the installer for your operating system</p>
              </div>
            </div>

            <div class="step">
              <div class="step-number">2</div>
              <div class="step-content">
                <h4>Install Dependencies</h4>
                <p>Run <code>pip install -r requirements.txt</code> and <code>npm install</code></p>
              </div>
            </div>

            <div class="step">
              <div class="step-number">3</div>
              <div class="step-content">
                <h4>Configure</h4>
                <p>Edit config.json with your network settings</p>
              </div>
            </div>

            <div class="step">
              <div class="step-number">4</div>
              <div class="step-content">
                <h4>Launch</h4>
                <p>Run <code>npm run dev</code> to start the application</p>
              </div>
            </div>
          </div>
        </div>

        <!-- Support -->
        <div class="support-section" data-aos="fade-up" data-aos-delay="400">
          <div class="support-card">
            <i class="fas fa-headset"></i>
            <h3>Need Help?</h3>
            <p>If you encounter any issues during installation or usage, our team is here to help.</p>
            <router-link to="/contact" class="btn outline">
              Contact Support
            </router-link>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.download {
  min-height: 100vh;
  padding: 120px 20px 80px;
  background: linear-gradient(135deg, #0b1c2c 0%, #16293d 100%);
}

.container {
  max-width: 1000px;
  margin: 0 auto;
}

.section-header {
  text-align: center;
  margin-bottom: 60px;
}

.subtitle {
  display: inline-block;
  padding: 8px 20px;
  background: rgba(66, 165, 245, 0.1);
  border: 1px solid rgba(66, 165, 245, 0.3);
  border-radius: 50px;
  color: #42a5f5;
  font-size: 14px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 1px;
  margin-bottom: 20px;
}

.section-header h1 {
  font-size: 56px;
  font-weight: 800;
  margin-bottom: 20px;
  background: linear-gradient(135deg, #ffffff, #42a5f5);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  color: transparent;
}

.section-header p {
  font-size: 20px;
  color: rgba(255, 255, 255, 0.7);
}

.download-content {
  display: flex;
  flex-direction: column;
  gap: 50px;
}

/* Main Download Card */
.main-download {
  display: flex;
  justify-content: center;
}

.download-card {
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: 24px;
  padding: 50px;
  text-align: center;
  max-width: 600px;
  width: 100%;
  position: relative;
  transition: all 0.4s ease;
}

.download-card:hover {
  background: rgba(255, 255, 255, 0.05);
  border-color: rgba(66, 165, 245, 0.2);
  transform: translateY(-5px);
}

.card-badge {
  position: absolute;
  top: 20px;
  right: 20px;
  padding: 6px 16px;
  background: linear-gradient(135deg, #4caf50, #388e3c);
  border-radius: 50px;
  font-size: 12px;
  font-weight: 600;
  color: white;
}

.platform-icon {
  width: 100px;
  height: 100px;
  background: linear-gradient(135deg, #42a5f5, #0077b6);
  border-radius: 24px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto 30px;
  font-size: 40px;
  color: white;
  box-shadow: 0 15px 40px rgba(66, 165, 245, 0.3);
}

.download-card h2 {
  font-size: 32px;
  font-weight: 700;
  margin-bottom: 10px;
  color: white;
}

.version-info {
  color: rgba(255, 255, 255, 0.6);
  font-size: 15px;
  margin-bottom: 30px;
}

.system-requirements {
  background: rgba(0, 0, 0, 0.2);
  border-radius: 16px;
  padding: 25px;
  margin-bottom: 30px;
  text-align: left;
}

.system-requirements h4 {
  font-size: 16px;
  margin-bottom: 15px;
  color: white;
}

.system-requirements ul {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.system-requirements li {
  display: flex;
  align-items: center;
  gap: 10px;
  font-size: 14px;
  color: rgba(255, 255, 255, 0.8);
}

.system-requirements li i {
  color: #4caf50;
  font-size: 12px;
}

.download-buttons {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.btn {
  padding: 16px 32px;
  border-radius: 12px;
  text-decoration: none;
  font-size: 16px;
  font-weight: 600;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  transition: all 0.3s ease;
}

.btn.primary {
  background: linear-gradient(135deg, #0077b6, #0096c7);
  color: white;
  box-shadow: 0 4px 15px rgba(0, 119, 182, 0.4);
}

.btn.primary:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 25px rgba(0, 119, 182, 0.5);
}

.btn.secondary {
  background: rgba(255, 255, 255, 0.1);
  color: white;
  border: 2px solid rgba(255, 255, 255, 0.2);
}

.btn.secondary:hover {
  background: rgba(255, 255, 255, 0.2);
  border-color: rgba(255, 255, 255, 0.4);
}

.btn.outline {
  background: transparent;
  color: #42a5f5;
  border: 2px solid #42a5f5;
}

.btn.outline:hover {
  background: rgba(66, 165, 245, 0.1);
}

/* Alternative Downloads */
.alt-downloads h3 {
  font-size: 24px;
  margin-bottom: 25px;
  color: white;
  text-align: center;
}

.alt-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
}

.alt-card {
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: 16px;
  padding: 25px;
  display: flex;
  align-items: center;
  gap: 15px;
  text-decoration: none;
  color: white;
  transition: all 0.3s ease;
}

.alt-card:hover {
  background: rgba(255, 255, 255, 0.06);
  border-color: rgba(66, 165, 245, 0.2);
  transform: translateY(-3px);
}

.alt-icon {
  width: 50px;
  height: 50px;
  background: rgba(66, 165, 245, 0.1);
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 20px;
  color: #42a5f5;
  flex-shrink: 0;
}

.alt-icon.docker {
  color: #2496ed;
  background: rgba(36, 150, 237, 0.1);
}

.alt-icon.docs {
  color: #ff9800;
  background: rgba(255, 152, 0, 0.1);
}

.alt-info {
  flex: 1;
}

.alt-info h4 {
  font-size: 16px;
  margin-bottom: 5px;
}

.alt-info p {
  font-size: 13px;
  color: rgba(255, 255, 255, 0.6);
}

.alt-card > i {
  color: #42a5f5;
  font-size: 14px;
}

.badge {
  padding: 4px 12px;
  background: rgba(255, 152, 0, 0.2);
  border: 1px solid rgba(255, 152, 0, 0.3);
  border-radius: 50px;
  font-size: 12px;
  color: #ff9800;
}

/* Installation Guide */
.installation-guide h3 {
  font-size: 24px;
  margin-bottom: 30px;
  color: white;
  text-align: center;
}

.steps {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
}

.step {
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: 16px;
  padding: 30px 20px;
  text-align: center;
  transition: all 0.3s ease;
}

.step:hover {
  background: rgba(255, 255, 255, 0.06);
  transform: translateY(-5px);
}

.step-number {
  width: 50px;
  height: 50px;
  background: linear-gradient(135deg, #42a5f5, #0077b6);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 20px;
  font-weight: 700;
  color: white;
  margin: 0 auto 20px;
}

.step-content h4 {
  font-size: 16px;
  margin-bottom: 10px;
  color: white;
}

.step-content p {
  font-size: 13px;
  color: rgba(255, 255, 255, 0.7);
  line-height: 1.5;
}

.step-content code {
  background: rgba(0, 0, 0, 0.3);
  padding: 2px 6px;
  border-radius: 4px;
  font-family: 'Courier New', monospace;
  font-size: 12px;
  color: #4caf50;
}

/* Support Section */
.support-section {
  display: flex;
  justify-content: center;
}

.support-card {
  background: linear-gradient(135deg, rgba(66, 165, 245, 0.1), rgba(0, 119, 182, 0.1));
  border: 1px solid rgba(66, 165, 245, 0.2);
  border-radius: 20px;
  padding: 40px;
  text-align: center;
  max-width: 500px;
}

.support-card > i {
  font-size: 48px;
  color: #42a5f5;
  margin-bottom: 20px;
}

.support-card h3 {
  font-size: 24px;
  margin-bottom: 10px;
  color: white;
}

.support-card p {
  font-size: 15px;
  color: rgba(255, 255, 255, 0.7);
  margin-bottom: 25px;
}

/* Responsive */
@media (max-width: 968px) {
  .steps {
    grid-template-columns: repeat(2, 1fr);
  }

  .alt-grid {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 768px) {
  .download {
    padding: 90px 20px 60px;
  }

  .section-header h1 {
    font-size: 36px;
  }

  .download-card {
    padding: 30px 20px;
  }

  .steps {
    grid-template-columns: 1fr;
  }

  .download-buttons .btn {
    font-size: 14px;
  }
}

@media (max-width: 600px) {
  .download {
    padding: 80px 16px 50px;
  }

  .section-header {
    margin-bottom: 40px;
  }

  .section-header h1 {
    font-size: 28px;
  }

  .section-header p {
    font-size: 16px;
  }

  .download-card {
    padding: 25px 16px;
  }

  .alt-grid {
    grid-template-columns: 1fr;
    gap: 15px;
  }

  .alt-card {
    flex-direction: column;
    text-align: center;
    gap: 12px;
  }

  .download-buttons {
    gap: 10px;
  }

  .download-buttons .btn {
    font-size: 13px;
    padding: 14px 24px;
  }

  .steps {
    gap: 15px;
  }

  .step {
    padding: 20px 16px;
  }

  .support-card {
    padding: 30px 20px;
  }
}
</style>
