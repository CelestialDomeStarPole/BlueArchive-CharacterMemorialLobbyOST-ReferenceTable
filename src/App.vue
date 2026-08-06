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

function highlight(text) {
  const q = searchQuery.value.trim()
  if (!q || !text) return text
  const escaped = q.replace(/[.*+?^${}()|[\]\\]/g, '\\$&')
  const regex = new RegExp(`(${escaped})`, 'gi')
  return text.replace(regex, '<mark class="hl">$1</mark>')
}
</script>

<template>
  <div class="app">
    <header class="header">
      <a
        class="github-link"
        href="https://github.com/CelestialDomeStarPole/BlueArchive-CharacterMemorialLobbyOST-ReferenceTable"
        target="_blank"
        rel="noopener"
        title="查看 GitHub 仓库"
      >
        <i class="fab fa-github"></i>
      </a>
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
          <span class="song-name" v-html="highlight(item.song)"></span>
          <span class="play-btn" @click="musicPlayer?.playSong(item.song)">&#9654; 播放</span>
        </div>
        <div class="song-card-body">
          <div class="char-grid">
            <div
              v-for="(char, ci) in item.characters"
              :key="ci"
              class="char-item"
            >
              <span class="cn" v-html="highlight(char.cn)"></span>
              <span v-if="char.en" class="en" v-html="highlight(char.en)"></span>
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
  color: #37474f;
  min-height: 100vh;
}

html {
  min-height: 100%;
  background: linear-gradient(180deg, #e3f2fd 0%, #f8fbff 50%, #fff3e0 100%);
  background-attachment: fixed;
}

.app {
  min-height: 100vh;
}

.header {
  background: linear-gradient(135deg, #8ec5fc 0%, #e0c3fc 60%, #f6d365 100%);
  padding: 20px;
  text-align: center;
  position: sticky;
  top: 0;
  z-index: 100;
  border-bottom: 2px solid rgba(255, 255, 255, 0.5);
}

.github-link {
  position: fixed;
  top: 12px;
  right: 16px;
  z-index: 200;
  width: 38px;
  height: 38px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(255, 255, 255, 0.85);
  border-radius: 50%;
  color: #37474f;
  font-size: 20px;
  text-decoration: none;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.18);
  transition: transform 0.2s, background 0.2s;
}

.github-link:hover {
  transform: scale(1.1) rotate(10deg);
  background: #ffffff;
  color: #5d21d2;
}

.title {
  font-size: 24px;
  color: #fff;
  margin-bottom: 12px;
  text-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
}

.search-box {
  max-width: 600px;
  margin: 0 auto 8px;
}

.search-input {
  width: 100%;
  padding: 10px 16px;
  border: 2px solid rgba(49, 176, 235, 0.35);
  border-radius: 8px;
  background: #ffffff;
  color: #37474f;
  font-size: 15px;
  outline: none;
  transition: border-color 0.3s, box-shadow 0.3s;
}

.search-input:focus {
  border-color: #31b0eb;
  box-shadow: 0 0 0 3px rgba(49, 176, 235, 0.15);
}

.search-input::placeholder {
  color: #90a4ae;
}

.search-info {
  font-size: 13px;
  color: #607d8b;
  margin-bottom: 8px;
}

.content {
  max-width: 1200px;
  margin: 20px auto;
  padding: 0 16px;
}

.song-card {
  background: #ffffff;
  border: 1px solid rgba(49, 176, 235, 0.15);
  border-radius: 12px;
  margin-bottom: 12px;
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(49, 176, 235, 0.12);
  transition: box-shadow 0.3s, border-color 0.3s;
}

.song-card:hover {
  border-color: rgba(49, 176, 235, 0.5);
  box-shadow: 0 4px 16px rgba(49, 176, 235, 0.2);
}

.song-card-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 14px 18px;
  background: #eaf6fe;
}

.song-card-header:hover {
  background: #dceefb;
}

.song-name {
  font-size: 17px;
  font-weight: 600;
  color: #1565c0;
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
  background: #f5f8fb;
  font-size: 14px;
  display: flex;
  align-items: center;
  gap: 6px;
}

.char-item .cn {
  color: #37474f;
}

.char-item .en {
  color: #90a4ae;
  font-size: 12px;
}

.no-result {
  text-align: center;
  padding: 60px 20px;
  color: #90a4ae;
  font-size: 16px;
}

.hl {
  background: #ffe57f;
  color: inherit;
  border-radius: 3px;
  padding: 0 1px;
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
