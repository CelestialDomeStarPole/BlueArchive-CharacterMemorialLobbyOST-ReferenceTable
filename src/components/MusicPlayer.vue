<script setup>
import { ref, onMounted, onBeforeUnmount, watch, nextTick } from 'vue'
import APlayer from 'aplayer'
import { ostData, audioMap } from '../data/ostData.js'

const ap = ref(null)
const currentSong = ref('')

const audioList = ostData
  .map(d => ({
    name: d.song,
    artist: 'Blue Archive',
    url: audioMap[d.song] ? `/music/${audioMap[d.song]}` : '',
    cover: '',
    lrc: ''
  }))
  .filter(a => a.url)

const initPlayer = () => {
  ap.value = new APlayer({
    container: document.getElementById('aplayer'),
    autoplay: false,
    mini: false,
    order: 'list',
    lrcType: 0,
    listFolded: false,
    loop: 'all',
    audio: audioList
  })

  ap.value.on('listswitch', () => {
    nextTick(() => {
      const idx = ap.value.list.index
      if (idx !== -1) {
        currentSong.value = ap.value.list.audios[idx].name
      }
    })
  })
}

const playSong = (name) => {
  const idx = audioList.findIndex(a => a.name === name)
  if (idx !== -1) {
    if (ap.value) {
      ap.value.list.switch(idx)
      ap.value.play()
    }
    currentSong.value = name
  }
}

const isPlaying = (name) => currentSong.value === name

defineExpose({ playSong })

onMounted(() => {
  initPlayer()
})

onBeforeUnmount(() => {
  if (ap.value) ap.value.destroy()
})
</script>

<template>
  <div class="music-player-wrapper">
    <div id="aplayer"></div>
  </div>
</template>

<style scoped>
.music-player-wrapper {
  width: 100%;
  max-width: 800px;
  margin: 0 auto;
}

#aplayer {
  width: 100%;
  opacity: 0.9;
  z-index: 2;
}
</style>

<style>
.aplayer {
  background: rgba(240,240,240,0.95) !important;
  border-radius: 8px !important;
  overflow: hidden;
}
.aplayer .aplayer-body {
  background-size: contain;
}
.aplayer .aplayer-pic {
  height: 120px !important;
  width: 120px !important;
}
.aplayer .aplayer-pic .aplayer-play {
  width: 44px !important;
  height: 44px !important;
  border: 3px solid #fff !important;
  margin: 0 -22px -22px 0 !important;
}
.aplayer .aplayer-pic .aplayer-play svg {
  height: 28px !important;
  width: 28px !important;
  top: 8px !important;
  left: 10px !important;
}
.aplayer .aplayer-info {
  margin-left: 120px !important;
  height: 120px !important;
  padding: 12px 7px 0 14px !important;
  display: flex !important;
  flex-direction: column !important;
}
.aplayer .aplayer-info .aplayer-music {
  flex-shrink: 0;
}
.aplayer .aplayer-info .aplayer-music .aplayer-title {
  font-size: 18px !important;
  color: #222 !important;
}
.aplayer .aplayer-info .aplayer-music .aplayer-artist {
  color: #555 !important;
}
.aplayer .aplayer-info .aplayer-controller {
  margin-top: auto !important;
}
.aplayer .aplayer-info .aplayer-controller .aplayer-time {
  bottom: 0 !important;
  height: 28px !important;
  display: flex !important;
  align-items: center !important;
  gap: 2px !important;
  font-size: 12px !important;
}
.aplayer .aplayer-icon {
  width: 24px !important;
  height: 24px !important;
}
.aplayer .aplayer-icon-loop,
.aplayer .aplayer-icon-order {
  display: inline-block !important;
}
.aplayer.aplayer-arrow .aplayer-icon-loop,
.aplayer.aplayer-arrow .aplayer-icon-order {
  display: inline-block !important;
}
.aplayer .aplayer-info .aplayer-controller .aplayer-volume-wrap .aplayer-volume-bar-wrap {
  bottom: 26px !important;
  width: 30px !important;
}
.aplayer .aplayer-info .aplayer-controller .aplayer-volume-wrap:hover .aplayer-volume-bar-wrap {
  height: 80px !important;
}
.aplayer .aplayer-info .aplayer-controller .aplayer-volume-wrap .aplayer-volume-bar-wrap .aplayer-volume-bar {
  right: 10px !important;
  width: 8px !important;
  height: 60px !important;
  background: rgba(0,0,0,0.12) !important;
  border-radius: 5px !important;
}
.aplayer .aplayer-info .aplayer-controller .aplayer-volume-wrap .aplayer-volume-bar-wrap .aplayer-volume-bar .aplayer-volume {
  width: 10px !important;
  background: #31b0eb !important;
  border-radius: 5px !important;
}
.aplayer .aplayer-list {
  max-height: 45vh !important;
}
.aplayer .aplayer-list ol {
  scrollbar-width: thin;
  scrollbar-color: rgba(49,176,235,0.4) transparent;
}
.aplayer .aplayer-list ol::-webkit-scrollbar {
  width: 6px;
}
.aplayer .aplayer-list ol::-webkit-scrollbar-track {
  background: transparent;
}
.aplayer .aplayer-list ol::-webkit-scrollbar-thumb {
  background: rgba(49,176,235,0.4);
  border-radius: 3px;
}
.aplayer .aplayer-list ol li {
  border-top: 1px solid rgba(0,0,0,0.06) !important;
  display: flex !important;
  align-items: center !important;
}
.aplayer .aplayer-list ol li .aplayer-list-cur {
  flex-shrink: 0 !important;
  position: static !important;
  margin-right: 4px !important;
}
.aplayer .aplayer-list ol li .aplayer-list-index {
  width: 32px !important;
  text-align: right !important;
  padding-right: 6px !important;
  color: #888 !important;
  font-size: 13px !important;
  flex-shrink: 0 !important;
}
.aplayer .aplayer-list ol li .aplayer-list-title {
  color: #333 !important;
  font-weight: 500 !important;
  font-size: 14px !important;
  flex: 1 !important;
  min-width: 0 !important;
  overflow: hidden !important;
  text-overflow: ellipsis !important;
  white-space: nowrap !important;
}
.aplayer .aplayer-list ol li .aplayer-list-author {
  margin-left: auto !important;
  padding-left: 8px !important;
  color: #999 !important;
  font-size: 12px !important;
  flex-shrink: 0 !important;
}
</style>
