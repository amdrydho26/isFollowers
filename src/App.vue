<script setup>
import { ref } from 'vue'
import LandingPage from './components/LandingPage.vue'
import DataAnalysis from './components/DataAnalysis.vue'

// Reactive data
const currentView = ref('landing') // 'landing' or 'analysis'
const followers = ref([])
const following = ref([])

// Handle data ready from landing page
const handleDataReady = (data) => {
  followers.value = data.followersData
  following.value = data.followingData
  currentView.value = 'analysis'
}

// Handle back to upload
const handleBackToUpload = () => {
  currentView.value = 'landing'
  followers.value = []
  following.value = []
}

// Scroll to upload help section
const scrollToHelp = () => {
  const uploadHelpSection = document.getElementById('upload-help');
  if (uploadHelpSection) {
    uploadHelpSection.scrollIntoView({ behavior: 'smooth' });
  }
};
</script>

<template>
  <div class="app">
    <!-- Top Bar -->
    <div class="top-bar">
      <div class="top-bar-content">
        <div class="logo"><span style="font-weight: 400;">is</span><span style="font-weight: 700;">Followers</span></div>
        <div class="top-bar-icons">
          <a href="https://github.com/amdrydho26/isFollowers.git" target="_blank" class="icon github-icon">
            <svg width="24" height="24" viewBox="0 0 24 24" fill="currentColor">
              <path d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"/>
            </svg>
          </a>
          <button @click="scrollToHelp" class="icon tutorial-icon" title="Tutorial">
            <svg width="24" height="24" viewBox="0 0 24 24" fill="currentColor">
              <path d="M9 21c0 .55.45 1 1 1h4c.55 0 1-.45 1-1v-1H9v1zm3-19C8.14 2 5 5.14 5 9c0 2.38 1.19 4.47 3 5.74V17c0 .55.45 1 1 1h6c.55 0 1-.45 1-1v-2.26c1.81-1.27 3-3.36 3-5.74 0-3.86-3.14-7-7-7zm2.85 11.1l-.85.6V16h-4v-2.3l-.85-.6C7.8 12.16 7 10.63 7 9c0-2.76 2.24-5 5-5s5 2.24 5 5c0 1.63-.8 3.16-2.15 4.1z"/>
            </svg>
          </button>
        </div>
      </div>
    </div>

    <!-- Main Content -->
    <main class="main-content">
      <LandingPage 
        v-if="currentView === 'landing'" 
        @data-ready="handleDataReady" 
      />
      <DataAnalysis 
        v-else-if="currentView === 'analysis'"
        :followers="followers"
        :following="following"
        @back-to-upload="handleBackToUpload"
      />
    </main>
  </div>
</template>

<style scoped>
.app {
  background-color: var(--bg-primary);
  min-height: 100vh;
  color: var(--text-primary);
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
}

/* Top Bar */
.top-bar {
  background-color: var(--bg-secondary);
  border-radius: 16px;
  padding: 16px 0;
  margin: 20px auto 32px auto;
  box-shadow: 0 8px 32px #3457d512;
  backdrop-filter: blur(10px);
  max-width: 1160px;
  width: 100%;
  position: relative;
  z-index: 2;
}

.top-bar-content {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 25px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.logo {
  font-size: 24px;
  font-weight: 600;
  color: var(--text-primary);
}

.top-bar-icons {
  display: flex;
  gap: 12px;
}

.icon {
  width: 48px;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.3s ease;
  text-decoration: none;
  color: var(--accent-blue);
  background: none;
  border: none;
  padding: 0;
}

.icon:hover {
  transform: translateY(-2px);
}

.icon svg {
  transition: transform 0.2s ease;
}

.icon:hover svg {
  transform: scale(1.1);
}

.github-icon {
  color: var(--accent-blue);
}

.github-icon:hover {
  color: #2a4bc4;
}

.tutorial-icon {
  color: var(--accent-blue);
  background: transparent !important;
  border: none !important;
  outline: none !important;
}

.tutorial-icon:hover {
  color: #2a4bc4;
  background: transparent !important;
}

.tutorial-icon:focus {
  background: transparent !important;
  outline: none !important;
}

/* Container */
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

/* Main Content */
.main-content {
  flex: 1;
}

/* Loading and Error */
.loading, .error {
  text-align: center;
  padding: 40px;
  font-size: 18px;
  color: var(--text-secondary);
}

.error {
  color: #ff6b6b;
}

/* Section Titles */
.section-title {
  font-size: 32px;
  font-weight: 700;
  color: var(--text-primary);
  margin-bottom: 8px;
  text-align: left;
}

.count {
  color: var(--accent-blue);
  font-weight: 600;
  font-size: 24px;
}

.section-description {
  color: var(--text-secondary);
  text-align: left;
  margin-bottom: 24px;
  font-size: 14px;
}

/* Statistics Section */
.stats-section {
  margin-bottom: 48px;
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 20px;
  margin-top: 24px;
}

.stat-card {
  background: var(--bg-secondary);
  padding: 24px 20px;
  border-radius: 12px;
  text-align: left;
  transition: transform 0.2s;
}

.stat-card:hover {
  transform: translateY(-2px);
}

.stat-card h3 {
  margin: 0;
  color: var(--text-secondary);
  font-size: 14px;
  font-weight: 500;
  text-transform: none;
}

.stat-number {
  font-size: 33px !important;
  font-weight: 700;
  color: var(--text-primary);
  margin: 0;
}

/* Sections */
.section {
  margin-bottom: 48px;
}

.section h2 {
  color: var(--text-primary);
  margin-bottom: 8px;
}

.section p {
  color: var(--text-secondary);
  margin-bottom: 24px;
}

/* User Lists */
.user-list {
  background: var(--bg-secondary);
  border-radius: 12px;
  max-height: 400px;
  overflow-y: auto;
  border: 1px solid var(--border-color);
}

.user-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px 20px;
  border-bottom: 1px solid var(--border-color);
  transition: background-color 0.2s;
}

.user-item:last-child {
  border-bottom: none;
}

.user-item:hover {
  background-color: #353535;
}

.username {
  color: var(--accent-blue);
  text-decoration: none;
  font-weight: 500;
  font-size: 16px;
}

.username:hover {
  text-decoration: underline;
}

.timestamp {
  color: var(--text-secondary);
  font-size: 12px;
}

/* Scrollbar styling */
.user-list::-webkit-scrollbar {
  width: 1px;
}

.user-list::-webkit-scrollbar-track {
  background: var(--bg-secondary);
  border-radius: 4px;
}

.user-list::-webkit-scrollbar-thumb {
  background: var(--accent-blue);
  border-radius: 4px;
}

.user-list::-webkit-scrollbar-thumb:hover {
  background: #606060;
}

/* Responsive */
@media (max-width: 768px) {
  .top-bar {
    margin: 16px auto 24px auto;
    padding: 12px 0;
    max-width: calc(100% - 32px);
  }
  
  .top-bar-content {
    padding: 0 16px;
  }
  
  .logo {
    font-size: 18px;
  }
  
  .icon {
    width: 36px;
    height: 36px;
  }
  
  .stats-grid {
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    gap: 16px;
  }
  
  .stat-card {
    padding: 20px 16px;
  }
  
  .stat-number {
    font-size: 24px;
  }
  
  .section-title {
    font-size: 24px;
  }
}

@media (max-width: 480px) {
  .top-bar {
    margin: 12px auto 20px auto;
    padding: 10px 0;
    max-width: calc(100% - 24px);
  }
  
  .top-bar-content {
    padding: 0 12px;
  }
  
  .logo {
    font-size: 16px;
  }
  
  .icon {
    width: 32px;
    height: 32px;
  }
  
  .icon svg {
    width: 16px;
    height: 16px;
  }
}
</style>
