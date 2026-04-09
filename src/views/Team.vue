<template>
  <section class="team">
    <div class="container">
      <div class="section-header">
        <span class="subtitle">Our Team</span>
        <h1>Meet The Developers</h1>
        <p>The talented people behind ANCS who made it all possible</p>
      </div>

      <div class="team-grid">
        <!-- Mohamed Bendary -->
        <div class="team-card">
          <div class="card-image">
            <div class="avatar">MB</div>
            <div class="social-links">
              <a href="#" target="_blank"><i class="fab fa-github"></i></a>
              <a href="#" target="_blank"><i class="fab fa-linkedin"></i></a>
              <a href="#" target="_blank"><i class="fas fa-envelope"></i></a>
            </div>
          </div>
          <div class="card-info">
            <h3>Mohamed Bendary</h3>
            <span class="role">Backend Developer</span>
            <p>Responsible for server-side logic, database management, and API development</p>
            <div class="skills">
              <span>Django</span>
              <span>Python</span>
              <span>Database</span>
            </div>
          </div>
        </div>

        <!-- Mohamed Ehab -->
        <div class="team-card">
          <div class="card-image">
            <div class="avatar" style="background: linear-gradient(135deg, #e91e63, #c2185b);">ME</div>
            <div class="social-links">
              <a href="#" target="_blank"><i class="fab fa-github"></i></a>
              <a href="#" target="_blank"><i class="fab fa-linkedin"></i></a>
              <a href="#" target="_blank"><i class="fas fa-envelope"></i></a>
            </div>
          </div>
          <div class="card-info">
            <h3>Mohamed Ehab</h3>
            <span class="role">Frontend Developer</span>
            <p>Designed and developed the user interface and user experience</p>
            <div class="skills">
              <span>Vue.js</span>
              <span>CSS3</span>
              <span>UI/UX</span>
            </div>
          </div>
        </div>

        <!-- Zyad -->
        <div class="team-card">
          <div class="card-image">
            <div class="avatar" style="background: linear-gradient(135deg, #4caf50, #388e3c);">Z</div>
            <div class="social-links">
              <a href="#" target="_blank"><i class="fab fa-github"></i></a>
              <a href="#" target="_blank"><i class="fab fa-linkedin"></i></a>
              <a href="#" target="_blank"><i class="fas fa-envelope"></i></a>
            </div>
          </div>
          <div class="card-info">
            <h3>Zyad</h3>
            <span class="role">Network Engineer</span>
            <p>Network infrastructure expert and GNS3 integration specialist</p>
            <div class="skills">
              <span>Cisco</span>
              <span>GNS3</span>
              <span>Networking</span>
            </div>
          </div>
        </div>

        <!-- Youssef (Logo Designer) -->
        <div class="team-card">
          <div class="card-image">
            <div class="avatar" style="background: linear-gradient(135deg, #ff9800, #f57c00);">Y</div>
            <div class="social-links">
              <a href="#" target="_blank"><i class="fab fa-github"></i></a>
              <a href="#" target="_blank"><i class="fab fa-linkedin"></i></a>
              <a href="#" target="_blank"><i class="fas fa-envelope"></i></a>
            </div>
          </div>
          <div class="card-info">
            <h3>Youssef</h3>
            <span class="role">UI Designer & Logo Creator</span>
            <p>Created the ANCS brand identity and visual design system</p>
            <div class="skills">
              <span>Logo Design</span>
              <span>Branding</span>
              <span>UI Design</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.team {
  min-height: 100vh;
  padding: 120px 20px 80px;
  background: linear-gradient(135deg, #0b1c2c 0%, #16293d 100%);
}

.container {
  max-width: 1200px;
  margin: 0 auto;
}

.section-header {
  text-align: center;
  margin-bottom: 80px;
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
  color: #fff;
}

.section-header p {
  font-size: 20px;
  color: rgba(255, 255, 255, 0.7);
  max-width: 600px;
  margin: 0 auto;
}

.team-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 30px;
}

.team-card {
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: 24px;
  overflow: hidden;
  transition: all 0.4s ease;
}

.team-card:hover {
  transform: translateY(-10px);
  background: rgba(255, 255, 255, 0.06);
  border-color: rgba(66, 165, 245, 0.2);
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.3);
}

.card-image {
  position: relative;
  height: 200px;
  background: linear-gradient(135deg, rgba(66, 165, 245, 0.1), rgba(0, 119, 182, 0.1));
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.avatar {
  width: 100px;
  height: 100px;
  background: linear-gradient(135deg, #42a5f5, #0077b6);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 36px;
  font-weight: 700;
  color: white;
  border: 4px solid rgba(255, 255, 255, 0.1);
  transition: all 0.4s ease;
}

.team-card:hover .avatar {
  transform: scale(1.1);
  border-color: rgba(66, 165, 245, 0.5);
}

.social-links {
  position: absolute;
  bottom: -50px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 15px;
  transition: all 0.4s ease;
}

.team-card:hover .social-links {
  bottom: 20px;
}

.social-links a {
  width: 40px;
  height: 40px;
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  text-decoration: none;
  transition: all 0.3s ease;
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.social-links a:hover {
  background: #42a5f5;
  transform: translateY(-3px);
}

.card-info {
  padding: 30px;
  text-align: center;
}

.card-info h3 {
  font-size: 22px;
  font-weight: 700;
  margin-bottom: 8px;
  color: white;
}

.role {
  display: inline-block;
  padding: 6px 16px;
  background: rgba(66, 165, 245, 0.1);
  border: 1px solid rgba(66, 165, 245, 0.3);
  border-radius: 50px;
  color: #42a5f5;
  font-size: 13px;
  font-weight: 600;
  margin-bottom: 15px;
}

.card-info > p {
  font-size: 14px;
  color: rgba(255, 255, 255, 0.6);
  line-height: 1.6;
  margin-bottom: 20px;
}

.skills {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  justify-content: center;
}

.skills span {
  padding: 5px 12px;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 20px;
  font-size: 12px;
  color: rgba(255, 255, 255, 0.8);
}

/* Responsive */
@media (max-width: 1200px) {
  .team-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 768px) {
  .section-header h1 {
    font-size: 36px;
  }

  .team-grid {
    grid-template-columns: 1fr;
    max-width: 400px;
    margin: 0 auto;
  }
}

@media (max-width: 600px) {
  .team {
    padding: 90px 20px 60px;
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

  .team-grid {
    max-width: 100%;
  }

  .team-card {
    padding: 20px;
  }

  .team-card h3 {
    font-size: 16px;
  }

  .team-card p {
    font-size: 13px;
  }

  .member-role {
    font-size: 12px;
  }
}
</style>
