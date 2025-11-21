<template>
  <ion-page>
    <!-- HEADER -->
    <ion-header :translucent="true">
      <ion-toolbar color="primary">
        <ion-title>
          Crypto Market
        </ion-title>
      </ion-toolbar>

      <ion-toolbar class="sub-toolbar">
        <div class="sub-header">
          <div class="subtitle">
            Live prices from CoinLore API
          </div>
          <div class="meta">
            <span v-if="coins.length">Total: {{ coins.length }} coins</span>
            <span v-if="lastUpdated">• Updated: {{ lastUpdated }}</span>
          </div>
        </div>
      </ion-toolbar>
    </ion-header>

    <!-- CONTENT -->
    <ion-content :fullscreen="true">
      <ion-refresher slot="fixed" @ionRefresh="doRefresh($event)">
        <ion-refresher-content></ion-refresher-content>
      </ion-refresher>

      <!-- Header yang ikut turun ketika scroll -->
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Crypto Market</ion-title>
        </ion-toolbar>
      </ion-header>

      <div class="page-wrapper">
        <!-- Panel atas: search + tombol refresh -->
        <div class="control-card">
          <ion-searchbar
            v-model="search"
            placeholder="Cari koin berdasarkan nama atau simbol..."
            :debounce="300"
          ></ion-searchbar>

          <div class="control-row">
            <div class="left-info">
              <ion-text color="medium" v-if="filteredCoins.length">
                Menampilkan {{ filteredCoins.length }} dari {{ coins.length }} koin
              </ion-text>
              <ion-text color="danger" v-if="errorMessage">
                {{ errorMessage }}
              </ion-text>
            </div>

            <div class="right-actions">
              <ion-button
                size="small"
                fill="outline"
                :disabled="loading"
                @click="loadData"
              >
                <ion-spinner v-if="loading" name="dots"></ion-spinner>
                <span v-else>Refresh</span>
              </ion-button>
            </div>
          </div>
        </div>

        <!-- LIST DATA -->
        <ion-list v-if="!loading || coins.length" class="coin-list">
          <coin-list-item
            v-for="coin in filteredCoins"
            :key="coin.id"
            :coin="coin"
          />
        </ion-list>

        <!-- STATE: LOADING AWAL -->
        <div v-if="loading && !coins.length" class="state-container">
          <ion-spinner name="crescent"></ion-spinner>
          <p>Memuat data harga crypto...</p>
        </div>

        <!-- STATE: TIDAK ADA DATA -->
        <div v-if="!loading && !coins.length && !errorMessage" class="state-container">
          <p>Tidak ada data yang dapat ditampilkan.</p>
        </div>

        <!-- STATE: ERROR -->
        <div v-if="errorMessage && !coins.length" class="state-container">
          <p>{{ errorMessage }}</p>
        </div>
      </div>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import {
  IonButton,
  IonContent,
  IonHeader,
  IonList,
  IonPage,
  IonRefresher,
  IonRefresherContent,
  IonTitle,
  IonToolbar,
  IonSearchbar,
  IonSpinner,
  IonText
} from '@ionic/vue';
import { defineComponent } from 'vue';
import CoinListItem from '@/components/CoinListItem.vue';

interface Coin {
  id: string;
  symbol: string;
  name: string;
  rank: string;
  price_usd: string;
  percent_change_24h?: string;
}

export default defineComponent({
  name: 'Home',
  components: {
    IonButton,
    IonContent,
    IonHeader,
    IonList,
    IonPage,
    IonRefresher,
    IonRefresherContent,
    IonTitle,
    IonToolbar,
    IonSearchbar,
    IonSpinner,
    IonText,
    CoinListItem
  },
  data() {
    return {
      coins: [] as Coin[],
      loading: false,
      search: '',
      lastUpdated: '',
      errorMessage: ''
    };
  },
  mounted() {
    this.loadData();
  },
  computed: {
    filteredCoins(): Coin[] {
      if (!this.search) {
        return this.coinsSorted;
      }
      const q = this.search.toLowerCase();
      return this.coinsSorted.filter((c) => {
        return (
          c.name.toLowerCase().includes(q) ||
          c.symbol.toLowerCase().includes(q)
        );
      });
    },
    coinsSorted(): Coin[] {
      // Sort default by rank ascending
      return [...this.coins].sort((a, b) => Number(a.rank) - Number(b.rank));
    }
  },
  methods: {
    loadData() {
      this.loading = true;
      this.errorMessage = '';

      this.axios
        .get('https://api.coinlore.net/api/tickers/')
        .then((response: any) => {
          this.coins = response.data.data;
          this.lastUpdated = new Date().toLocaleString();
        })
        .catch((err: any) => {
          console.error(err);
          this.errorMessage = 'Gagal mengambil data API. Periksa koneksi internet Anda.';
        })
        .finally(() => {
          this.loading = false;
        });
    },
    doRefresh(ev: CustomEvent) {
      this.errorMessage = '';

      this.axios
        .get('https://api.coinlore.net/api/tickers/')
        .then((response: any) => {
          this.coins = response.data.data;
          this.lastUpdated = new Date().toLocaleString();
        })
        .catch((err: any) => {
          console.error(err);
          this.errorMessage = 'Gagal mengambil data API. Periksa koneksi internet Anda.';
        })
        .finally(() => {
          ev.detail.complete();
        });
    }
  }
});
</script>

<style scoped>
.page-wrapper {
  padding: 12px 0 24px;
  background: #f3f4f6;
  min-height: 100%;
}

.sub-toolbar {
  --background: #0f172a;
  --color: #e5e7eb;
}

.sub-header {
  padding-inline: 16px;
  padding-block: 6px 8px;
  display: flex;
  flex-direction: column;
  gap: 2px;
}

.subtitle {
  font-size: 13px;
  opacity: 0.9;
}

.meta {
  font-size: 11px;
  opacity: 0.7;
}

.control-card {
  margin: 12px 12px 4px;
  padding: 10px 12px 8px;
  border-radius: 16px;
  background: #ffffff;
  box-shadow: 0 4px 14px rgba(15, 23, 42, 0.06);
  border: 1px solid #e5e7eb;
}

.control-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 4px;
  gap: 8px;
}

.left-info {
  font-size: 12px;
}

.right-actions {
  display: flex;
  align-items: center;
}

.coin-list {
  background: transparent;
  margin-top: 4px;
}

.state-container {
  padding: 40px 16px;
  text-align: center;
  color: #6b7280;
  font-size: 14px;
}

.state-container ion-spinner {
  margin-bottom: 8px;
}
</style>
