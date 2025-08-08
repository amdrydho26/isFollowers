<script setup>
import { ref, onMounted, computed } from 'vue'
import followersData from './assets/followers_1.json'
import followingData from './assets/following.json'

// Reactive data
const followers = ref([])
const following = ref([])
const isLoading = ref(true)
const error = ref('')

// Parse data function
const parseUserData = (data, isFollowing = false) => {
  if (isFollowing) {
    // Following data has different structure
    return data.relationships_following?.map(item => ({
      username: item.string_list_data[0]?.value || '',
      href: item.string_list_data[0]?.href || '',
      timestamp: item.string_list_data[0]?.timestamp || 0,
      date: new Date((item.string_list_data[0]?.timestamp || 0) * 1000)
    })) || []
  } else {
    // Followers data structure
    return data.map(item => ({
      username: item.string_list_data[0]?.value || '',
      href: item.string_list_data[0]?.href || '',
      timestamp: item.string_list_data[0]?.timestamp || 0,
      date: new Date((item.string_list_data[0]?.timestamp || 0) * 1000)
    }))
  }
}

// Computed properties for analysis
const followersSet = computed(() => new Set(followers.value.map(f => f.username)))
const followingSet = computed(() => new Set(following.value.map(f => f.username)))

const mutualFollowers = computed(() => {
  return followers.value.filter(follower => followingSet.value.has(follower.username))
})

const notFollowingBack = computed(() => {
  return followers.value.filter(follower => !followingSet.value.has(follower.username))
})

const youDontFollowBack = computed(() => {
  return following.value.filter(followed => !followersSet.value.has(followed.username))
})

const statistics = computed(() => ({
  totalFollowers: followers.value.length,
  totalFollowing: following.value.length,
  mutualFollowersCount: mutualFollowers.value.length,
  notFollowingBackCount: notFollowingBack.value.length,
  youDontFollowBackCount: youDontFollowBack.value.length,
  followBackRate: followers.value.length > 0 ? ((mutualFollowers.value.length / followers.value.length) * 100).toFixed(2) : 0
}))

// Load data on component mount
onMounted(async () => {
  try {
    isLoading.value = true
    
    // Parse the imported JSON data
    followers.value = parseUserData(followersData)
    following.value = parseUserData(followingData, true)
    
    console.log('Followers loaded:', followers.value.length)
    console.log('Following loaded:', following.value.length)
    
  } catch (err) {
    error.value = `Error memuat data: ${err.message}`
    console.error('Error:', err)
  } finally {
    isLoading.value = false
  }
})

// Helper function to format date
const formatDate = (date) => {
  return date.toLocaleDateString('id-ID', {
    year: 'numeric',
    month: 'long',
    day: 'numeric',
    hour: '2-digit',
    minute: '2-digit'
  })
}
</script>

<template>
  <div class="app">
    <!-- Top Bar -->
    <div class="top-bar">
      <div class="top-bar-content">
        <div class="logo">isFollowers</div>
        <div class="top-bar-icons">
          <a href="https://github.com" target="_blank" class="icon github-icon">
            <svg width="24" height="24" viewBox="0 0 24 24" fill="currentColor">
              <path d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"/>
            </svg>
          </a>
          <a href="#" class="icon tutorial-icon" title="Tutorial">
            <svg width="24" height="24" viewBox="0 0 24 24" fill="currentColor">
              <path d="M9 21c0 .55.45 1 1 1h4c.55 0 1-.45 1-1v-1H9v1zm3-19C8.14 2 5 5.14 5 9c0 2.38 1.19 4.47 3 5.74V17c0 .55.45 1 1 1h6c.55 0 1-.45 1-1v-2.26c1.81-1.27 3-3.36 3-5.74 0-3.86-3.14-7-7-7zm2.85 11.1l-.85.6V16h-4v-2.3l-.85-.6C7.8 12.16 7 10.63 7 9c0-2.76 2.24-5 5-5s5 2.24 5 5c0 1.63-.8 3.16-2.15 4.1z"/>
            </svg>
          </a>
        </div>
      </div>
    </div>

    <div class="container">
      <div v-if="isLoading">
        <p class="loading">Memuat data...</p>
      </div>
      
      <div v-else-if="error">
        <p class="error">{{ error }}</p>
      </div>
      
      <div v-else>
        <!-- Statistics Section -->
        <div class="stats-section">
          <h2 class="section-title">Statistik</h2>
          <div class="stats-grid">
            <div class="stat-card">
              <h3>Total pengikut</h3>
              <p class="stat-number">{{ statistics.totalFollowers }}</p>
            </div>
            <div class="stat-card">
              <h3>Total Mengikuti</h3>
              <p class="stat-number">{{ statistics.totalFollowing }}</p>
            </div>
            <div class="stat-card">
              <h3>Saling Mengikuti</h3>
              <p class="stat-number">{{ statistics.mutualFollowersCount }}</p>
            </div>
            <div class="stat-card">
              <h3>Kamu Tidak Follbackk</h3>
              <p class="stat-number">{{ statistics.notFollowingBackCount }}</p>
            </div>
            <div class="stat-card">
              <h3>Tidak Follback Kamu</h3>
              <p class="stat-number">{{ statistics.youDontFollowBackCount }}</p>
            </div>
            <div class="stat-card">
              <h3>Tingkat Follow Back</h3>
              <p class="stat-number">{{ statistics.followBackRate }}%</p>
            </div>
          </div>
        </div>

        <!-- Mutual Followers Section -->
        <div class="section">
          <h2 class="section-title">Saling Mengikuti • <span class="count">{{ mutualFollowers.length }}</span></h2>
          <p class="section-description">Orang yang mengikuti kamu dan kamu ikuti balik</p>
          <div class="user-list">
            <div v-for="user in mutualFollowers" :key="user.username" class="user-item">
              <a :href="user.href" target="_blank" class="username">@{{ user.username }}</a>
              <span class="timestamp">{{ formatDate(user.date) }}</span>
            </div>
          </div>
        </div>

        <!-- Not Following Back Section -->
        <div class="section">
          <h2 class="section-title">Tidak Follow Kamu Balik • <span class="count">{{ youDontFollowBack.length }}</span></h2>
          <p class="section-description">Orang yang kamu ikuti tapi mereka tidak mengikuti kamu balik</p>
          <div class="user-list">
            <div v-for="user in youDontFollowBack" :key="user.username" class="user-item">
              <a :href="user.href" target="_blank" class="username">@{{ user.username }}</a>
              <span class="timestamp">{{ formatDate(user.date) }}</span>
            </div>
          </div>
        </div>

        <!-- You Don't Follow Back Section -->
        <div class="section">
          <h2 class="section-title">Kamu Tidak Follow Balik • <span class="count">{{ notFollowingBack.length }}</span></h2>
          <p class="section-description">Orang yang mengikuti kamu tapi kamu tidak mengikuti mereka balik</p>
          <div class="user-list">
            <div v-for="user in notFollowingBack" :key="user.username" class="user-item">
              <a :href="user.href" target="_blank" class="username">@{{ user.username }}</a>
              <span class="timestamp">{{ formatDate(user.date) }}</span>
            </div>
          </div>
        </div>

        <!-- All Followers Section -->
        <div class="section">
          <h2 class="section-title">Semua Pengikut • <span class="count">{{ followers.length }}</span></h2>
          <div class="user-list">
            <div v-for="user in followers" :key="user.username" class="user-item">
              <a :href="user.href" target="_blank" class="username">@{{ user.username }}</a>
              <span class="timestamp">{{ formatDate(user.date) }}</span>
            </div>
          </div>
        </div>

        <!-- All Following Section -->
        <div class="section">
          <h2 class="section-title">Semua yang Diikuti • <span class="count">{{ following.length }}</span></h2>
          <div class="user-list">
            <div v-for="user in following" :key="user.username" class="user-item">
              <a :href="user.href" target="_blank" class="username">@{{ user.username }}</a>
              <span class="timestamp">{{ formatDate(user.date) }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.app {
  background-color: var(--bg-primary);
  min-height: 100vh;
  color: var(--text-primary);
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
}

/* Top Bar */
.top-bar {
  background-color: var(--bg-secondary);
  border-radius: 16px;
  padding: 16px 0;
  margin: 20px 20px 32px 20px;
  box-shadow: 0 8px 32px #3457d512;
  backdrop-filter: blur(10px);
}

.top-bar-content {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
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

.github-icon:hover {
  color: #2a4bc4;
}

.tutorial-icon:hover {
  color: #2a4bc4;
}

/* Container */
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
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
    margin: 16px 16px 24px 16px;
    padding: 12px 0;
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
    margin: 12px 12px 20px 12px;
    padding: 10px 0;
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
