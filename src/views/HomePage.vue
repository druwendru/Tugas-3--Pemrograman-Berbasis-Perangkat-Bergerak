<template>
  <ion-page>

    <!-- ── Header ── -->
    <ion-header class="ion-no-border">
      <ion-toolbar>
        <div class="header-content">
          <div>
            <h1 class="header-title">Crypto Market</h1>
            <p class="header-sub">Live prices · Coinlore API</p>
          </div>
          <div class="live-badge">
            <span class="live-dot"></span>
            <span class="live-txt">LIVE</span>
          </div>
        </div>

        <div class="stats-row">
          <div class="stat-card">
            <span class="stat-lbl">Total Koin</span>
            <span class="stat-val">{{ totalCoins || '—' }}</span>
          </div>
          <div class="stat-card">
            <span class="stat-lbl">#1 Harga</span>
            <span class="stat-val">{{ coins.length ? formatPrice(coins[0].price_usd) : '—' }}</span>
          </div>
        </div>

        <div class="search-wrap">
          <ion-searchbar
            v-model="searchQuery"
            placeholder="Cari koin..."
            class="custom-search"
            @ion-input="filterCoins"
          />
        </div>
      </ion-toolbar>
    </ion-header>

    <!-- ── Content ── -->
    <ion-content class="ion-padding-horizontal">

      <div v-if="isLoading" class="loading-box">
        <ion-spinner name="crescent" color="primary"></ion-spinner>
        <p class="loading-txt">Mengambil data...</p>
      </div>

      <div v-else-if="error" class="error-box">
        <p class="error-txt">{{ error }}</p>
        <ion-button fill="outline" color="primary" @click="fetchCoins">Coba Lagi</ion-button>
      </div>

      <div v-else>
        <div
          v-for="(coin, index) in filteredCoins"
          :key="coin.id"
          class="coin-card"
        >
          <div class="coin-avatar" :style="avatarStyle(index)">
            {{ coin.symbol.slice(0, 2) }}
          </div>
          <div class="coin-info">
            <div class="coin-name-row">
              <span class="coin-sym">{{ coin.symbol }}</span>
              <span class="rank-pill">#{{ coin.rank }}</span>
            </div>
            <span class="coin-name">{{ coin.name }}</span>
          </div>
          <div class="coin-price-col">
            <span class="coin-price">{{ formatPrice(coin.price_usd) }}</span>
            <span
              class="coin-change"
              :class="parseFloat(coin.percent_change_24h) >= 0 ? 'pos' : 'neg'"
            >
              {{ parseFloat(coin.percent_change_24h) >= 0 ? '+' : '' }}{{ parseFloat(coin.percent_change_24h).toFixed(2) }}%
            </span>
          </div>
        </div>
      </div>

    </ion-content>

    <!-- ── Footer ── -->
    <ion-footer class="ion-no-border">
      <div class="footer-wrap">
        <ion-button expand="block" class="refresh-btn" @click="fetchCoins" :disabled="isLoading">
          <ion-icon slot="start" :icon="refreshOutline"></ion-icon>
          Refresh
        </ion-button>
        <div class="identity">
          <p class="id-name">Andre Bintang Pramastyo</p>
          <p class="id-nim">NIM: 050600902</p>
          <p class="id-prodi">Sistem Informasi · UPBJJ UT Surabaya</p>
        </div>
      </div>
    </ion-footer>

  </ion-page>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import {
  IonPage, IonHeader, IonToolbar, IonContent, IonFooter,
  IonSearchbar, IonSpinner, IonButton, IonIcon
} from '@ionic/vue'
import { refreshOutline } from 'ionicons/icons'

const coins         = ref<any[]>([])
const filteredCoins = ref<any[]>([])
const isLoading     = ref(false)
const error         = ref('')
const searchQuery   = ref('')
const totalCoins    = ref(0)

const avatarColors = [
  { bg: '#1A2A4A', fg: '#5B9BD5' },
  { bg: '#2A1A3A', fg: '#9B7FE8' },
  { bg: '#1A3A2A', fg: '#5BC98A' },
  { bg: '#3A2A1A', fg: '#E8A55B' },
  { bg: '#3A1A1A', fg: '#E87A7A' },
  { bg: '#1A3A3A', fg: '#5BD5D5' },
  { bg: '#2A3A1A', fg: '#9BD55B' },
  { bg: '#3A1A3A', fg: '#D55B9B' },
]

function avatarStyle(index: number) {
  const c = avatarColors[index % avatarColors.length]
  return { background: c.bg, color: c.fg }
}

function formatPrice(raw: string): string {
  const v = parseFloat(raw)
  if (isNaN(v)) return '—'
  if (v >= 1000) return '$' + v.toLocaleString('en-US', { maximumFractionDigits: 0 })
  if (v >= 1)    return '$' + v.toFixed(2)
  return '$' + v.toFixed(5)
}

async function fetchCoins() {
  isLoading.value = true
  error.value     = ''
  try {
    const res  = await fetch('https://api.coinlore.net/api/tickers/')
    const data = await res.json()
    coins.value         = data.data
    filteredCoins.value = data.data
    totalCoins.value    = data.info?.coins_num ?? data.data.length
  } catch (e) {
    error.value = 'Gagal memuat data. Periksa koneksi internet.'
  } finally {
    isLoading.value = false
  }
}

function filterCoins() {
  const q = searchQuery.value.toLowerCase()
  filteredCoins.value = coins.value.filter(
    c => c.name.toLowerCase().includes(q) || c.symbol.toLowerCase().includes(q)
  )
}

onMounted(() => fetchCoins())
</script>

<style scoped>
/* ── ion-page background ── */
ion-page {
  --background: #0F0F1A;
  background: #0F0F1A;
}

/* ── Header ── */
ion-toolbar {
  --background: #131320;
  --padding-start: 0;
  --padding-end: 0;
  --border-width: 0;
}
.header-content {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  padding: 14px 16px 10px;
}
.header-title {
  color: #ffffff;
  font-size: 18px;
  font-weight: 600;
  margin: 0;
}
.header-sub {
  color: #7C7C9A;
  font-size: 11px;
  margin: 2px 0 0;
}
.live-badge {
  display: flex;
  align-items: center;
  gap: 5px;
  background: #1A3A1A;
  border: 0.5px solid #2D6A2D;
  border-radius: 20px;
  padding: 4px 10px;
}
.live-dot {
  width: 6px; height: 6px;
  background: #4CAF50;
  border-radius: 50%;
  animation: pulse 1.5s infinite;
}
@keyframes pulse { 0%,100%{opacity:1} 50%{opacity:0.4} }
.live-txt {
  color: #4CAF50;
  font-size: 10px;
  font-weight: 600;
}

/* ── Stats ── */
.stats-row {
  display: flex;
  gap: 8px;
  padding: 0 16px 10px;
}
.stat-card {
  flex: 1;
  background: #1A1A2E;
  border: 0.5px solid #2A2A45;
  border-radius: 10px;
  padding: 8px 12px;
  display: flex;
  flex-direction: column;
}
.stat-lbl { color: #5A5A7A; font-size: 10px; }
.stat-val { color: #E0E0FF; font-size: 13px; font-weight: 500; margin-top: 2px; }

/* ── Search ── */
.search-wrap { padding: 0 10px 10px; }
ion-searchbar.custom-search {
  --background: #1A1A2E;
  --color: #ccc;
  --placeholder-color: #5A5A7A;
  --icon-color: #5A5A7A;
  --border-radius: 12px;
  --box-shadow: none;
  border: 0.5px solid #2A2A45;
  border-radius: 12px;
  padding: 0;
}

/* ── Content ── */
ion-content {
  --background: #0F0F1A;
}

/* ── Loading / Error ── */
.loading-box, .error-box {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 60px 20px;
  gap: 12px;
}
.loading-txt { color: #5A5A7A; font-size: 13px; }
.error-txt   { color: #f44336; font-size: 13px; text-align: center; }

/* ── Coin card ── */
.coin-card {
  display: flex;
  align-items: center;
  gap: 10px;
  background: #131320;
  border: 0.5px solid #1E1E35;
  border-radius: 14px;
  padding: 10px 12px;
  margin-bottom: 6px;
}
.coin-avatar {
  width: 38px; height: 38px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 12px;
  font-weight: 600;
  flex-shrink: 0;
}
.coin-info { flex: 1; min-width: 0; }
.coin-name-row { display: flex; align-items: center; gap: 6px; }
.coin-sym  { color: #E0E0FF; font-size: 13px; font-weight: 500; }
.rank-pill {
  background: #1E1E35;
  border-radius: 20px;
  padding: 1px 7px;
  font-size: 9px;
  color: #7C7C9A;
}
.coin-name {
  color: #5A5A7A;
  font-size: 10px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  display: block;
}
.coin-price-col { text-align: right; flex-shrink: 0; }
.coin-price { color: #E0E0FF; font-size: 12px; font-weight: 500; display: block; }
.coin-change { font-size: 10px; font-weight: 500; }
.pos { color: #4CAF50; }
.neg { color: #F44336; }

/* ── Footer ── */
ion-footer {
  --background: #0A0A14;
  background: #0A0A14;
}
.footer-wrap {
  background: #0A0A14;
  padding: 10px 16px 20px;
  border-top: 0.5px solid #1A1A2E;
}
.refresh-btn {
  --background: #5C4AFF;
  --background-activated: #4a39ee;
  --border-radius: 14px;
  --color: #ffffff;
  margin-bottom: 12px;
}
.identity { text-align: center; }
.id-name  { color: #7C7C9A; font-size: 12px; margin: 0; }
.id-nim   { color: #5A5A7A; font-size: 11px; margin: 2px 0; }
.id-prodi { color: #5C4AFF; font-size: 11px; margin: 0; }
</style>