<script setup>
import { ref, computed } from 'vue'
import MusicPlayer from './components/MusicPlayer.vue'
import { ostData } from './data/ostData.js'

const searchQuery = ref('')
const musicPlayer = ref(null)

const filteredData = computed(() => {
  const q = searchQuery.value.trim().toLowerCase()
  if (!q) return ostData
  return ostData.filter(d => {
    if (d.song.toLowerCase().includes(q)) return true
    return d.characters.some(c =>
      (c.cn && c.cn.toLowerCase().includes(q)) ||
      (c.en && c.en.toLowerCase().includes(q))
    )
  })
})
</script>

<template>
  <div class="app">
    <header class="header">
      <h1 class="title">Blue Archive · 角色记忆大厅 OST 对照</h1>
      <div class="search-box">
        <input
          v-model="searchQuery"
          type="text"
          class="search-input"
          placeholder="搜索歌曲或角色名称...（支持中/英文）"
        />
      </div>
      <div class="search-info">
        共 {{ ostData.length }} 首 OST{{ searchQuery ? `，搜索 "${searchQuery}" 显示 ${filteredData.length} 首` : '' }}
      </div>
      <MusicPlayer ref="musicPlayer" />
    </header>

    <main class="content">
      <div v-if="filteredData.length === 0" class="no-result">
        没有找到匹配的结果
      </div>
      <div
        v-for="(item, index) in filteredData"
        :key="index"
        class="song-card"
      >
        <div class="song-card-header">
          <span class="song-name">{{ item.song }}</span>
          <span class="play-btn" @click="musicPlayer?.playSong(item.song)">&#9654; 播放</span>
        </div>
        <div class="song-card-body">
          <div class="char-grid">
            <div
              v-for="(char, ci) in item.characters"
              :key="ci"
              class="char-item"
            >
              <span class="cn">{{ char.cn }}</span>
              <span v-if="char.en" class="en">{{ char.en }}</span>
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "Segoe UI", "Noto Sans", Roboto, -apple-system, BlinkMacSystemFont, "Helvetica Neue", "Noto Sans SC", "Microsoft YaHei", sans-serif;
  background: #1a1a2e;
  color: #e0e0e0;
  min-height: 100vh;
}

.app {
  min-height: 100vh;
}

.header {
  background: linear-gradient(135deg, #16213e 0%, #0f3460 50%, #1a1a2e 100%);
  padding: 20px;
  text-align: center;
  position: sticky;
  top: 0;
  z-index: 100;
  border-bottom: 2px solid rgba(49, 176, 235, 0.3);
}

.title {
  font-size: 24px;
  color: #fff;
  margin-bottom: 12px;
  text-shadow: 0 0 20px rgba(49, 176, 235, 0.5);
}

.search-box {
  max-width: 600px;
  margin: 0 auto 8px;
}

.search-input {
  width: 100%;
  padding: 10px 16px;
  border: 2px solid rgba(49, 176, 235, 0.3);
  border-radius: 8px;
  background: rgba(255, 255, 255, 0.08);
  color: #fff;
  font-size: 15px;
  outline: none;
  transition: border-color 0.3s;
}

.search-input:focus {
  border-color: #31b0eb;
}

.search-input::placeholder {
  color: rgba(255, 255, 255, 0.4);
}

.search-info {
  font-size: 13px;
  color: rgba(255, 255, 255, 0.5);
  margin-bottom: 8px;
}

.content {
  max-width: 1200px;
  margin: 20px auto;
  padding: 0 16px;
}

.song-card {
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 12px;
  margin-bottom: 12px;
  overflow: hidden;
  transition: border-color 0.3s;
}

.song-card:hover {
  border-color: rgba(49, 176, 235, 0.4);
}

.song-card-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 14px 18px;
  background: rgba(49, 176, 235, 0.1);
}

.song-card-header:hover {
  background: rgba(49, 176, 235, 0.15);
}

.song-name {
  font-size: 17px;
  font-weight: 600;
  color: #7ec8e3;
}

.play-btn {
  background: none;
  border: 1px solid rgba(49, 176, 235, 0.5);
  color: #31b0eb;
  padding: 6px 14px;
  border-radius: 6px;
  cursor: pointer;
  font-size: 13px;
  transition: all 0.2s;
  flex-shrink: 0;
}

.play-btn:hover {
  background: #31b0eb;
  color: #fff;
}

.song-card-body {
  padding: 12px 18px 16px;
}

.char-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
  gap: 8px;
}

.char-item {
  padding: 6px 10px;
  border-radius: 6px;
  background: rgba(255, 255, 255, 0.04);
  font-size: 14px;
  display: flex;
  align-items: center;
  gap: 6px;
}

.char-item .cn {
  color: #e0e0e0;
}

.char-item .en {
  color: rgba(255, 255, 255, 0.4);
  font-size: 12px;
}

.no-result {
  text-align: center;
  padding: 60px 20px;
  color: rgba(255, 255, 255, 0.3);
  font-size: 16px;
}

::-webkit-scrollbar {
  width: 6px;
}

::-webkit-scrollbar-track {
  background: transparent;
}

::-webkit-scrollbar-thumb {
  background: rgba(49, 176, 235, 0.3);
  border-radius: 3px;
}

::-webkit-scrollbar-thumb:hover {
  background: rgba(49, 176, 235, 0.5);
}

@media (max-width: 768px) {
  .title {
    font-size: 18px;
  }

  .char-grid {
    grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
  }
}
</style>
