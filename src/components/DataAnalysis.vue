<template>
  <div class="data-analysis">

    <div class="container">
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
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  followers: {
    type: Array,
    required: true
  },
  following: {
    type: Array,
    required: true
  }
})

const emit = defineEmits(['back-to-upload'])

// Computed properties for analysis
const followersSet = computed(() => new Set(props.followers.map(f => f.username)))
const followingSet = computed(() => new Set(props.following.map(f => f.username)))

const mutualFollowers = computed(() => {
  return props.followers.filter(follower => followingSet.value.has(follower.username))
})

const notFollowingBack = computed(() => {
  return props.followers.filter(follower => !followingSet.value.has(follower.username))
})

const youDontFollowBack = computed(() => {
  return props.following.filter(followed => !followersSet.value.has(followed.username))
})

const statistics = computed(() => ({
  totalFollowers: props.followers.length,
  totalFollowing: props.following.length,
  mutualFollowersCount: mutualFollowers.value.length,
  notFollowingBackCount: notFollowingBack.value.length,
  youDontFollowBackCount: youDontFollowBack.value.length,
  followBackRate: props.followers.length > 0 ? ((mutualFollowers.value.length / props.followers.length) * 100).toFixed(2) : 0
}))

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

<style scoped>
.data-analysis {
  background-color: var(--bg-primary);
  min-height: 100vh;
  color: var(--text-primary);
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
}

.header-section {
  background-color: var(--bg-secondary);
  border-radius: 16px;
  padding: 16px 0;
  margin: 20px 20px 32px 20px;
  box-shadow: 0 8px 32px #3457d512;
  backdrop-filter: blur(10px);
  max-width: 1160px;
  width: 100%;
  margin-left: auto;
  margin-right: auto;
}

.header-section {
  position: relative;
  text-align: center;
}

.back-btn {
  position: absolute;
  left: 25px;
  top: 50%;
  transform: translateY(-50%);
  background: none;
  border: none;
  color: var(--accent-blue);
  cursor: pointer;
  padding: 8px 16px;
  border-radius: 8px;
  display: flex;
  align-items: center;
  gap: 8px;
  font-family: 'Inter', sans-serif;
  font-weight: 500;
  transition: all 0.3s ease;
}

.back-btn:hover {
  background: rgba(52, 87, 213, 0.1);
  transform: translateY(-50%) translateX(-2px);
}

.analysis-title {
  font-size: 24px;
  font-weight: 600;
  color: var(--text-primary);
  margin: 0;
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
  .header-section {
    margin: 16px 16px 24px 16px;
    padding: 12px 0;
  }
  
  .back-btn {
    position: static;
    transform: none;
    margin-bottom: 16px;
    justify-content: center;
    width: 100%;
  }
  
  .analysis-title {
    font-size: 18px;
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
  .header-section {
    margin: 12px 12px 20px 12px;
    padding: 10px 0;
  }
  
  .analysis-title {
    font-size: 16px;
  }
  
  .back-btn {
    font-size: 14px;
    padding: 6px 12px;
  }
}
</style>
