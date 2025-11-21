<template>
  <ion-item v-if="coin" class="coin-item" lines="none">
    <ion-label class="ion-text-wrap">
      <!-- Baris atas: nama & simbol -->
      <div class="top-row">
        <div class="name-symbol">
          <div class="name">
            {{ coin.name }}
          </div>
          <div class="symbol">
            {{ coin.symbol }}
          </div>
        </div>

        <!-- Rank sebagai badge -->
        <div class="rank-badge">
          #{{ coin.rank }}
        </div>
      </div>

      <!-- Baris tengah: harga -->
      <div class="price-row">
        <div class="price-label">
          Price (USD)
        </div>
        <div class="price-value">
          {{ formattedPrice }}
        </div>
      </div>

      <!-- Baris bawah: optional change 24h kalau ada datanya -->
      <div v-if="coin.percent_change_24h" class="meta-row">
        <div class="change-label">
          24h Change
        </div>
        <div
          class="change-value"
          :class="{
            positive: Number(coin.percent_change_24h) > 0,
            negative: Number(coin.percent_change_24h) < 0
          }"
        >
          {{ Number(coin.percent_change_24h).toFixed(2) }}%
        </div>
      </div>
    </ion-label>
  </ion-item>
</template>

<script lang="ts">
import { defineComponent, computed } from 'vue';
import { IonItem, IonLabel } from '@ionic/vue';

interface Coin {
  id: string;
  symbol: string;
  name: string;
  rank: string;
  price_usd: string;
  percent_change_24h?: string;
}

export default defineComponent({
  name: 'CoinListItem',
  components: {
    IonItem,
    IonLabel
  },
  props: {
    coin: {
      type: Object as () => Coin,
      required: true
    }
  },
  setup(props) {
    const formattedPrice = computed(() => {
      const n = Number(props.coin.price_usd || 0);
      return n.toLocaleString('en-US', {
        minimumFractionDigits: 2,
        maximumFractionDigits: 2
      });
    });

    return {
      formattedPrice
    };
  }
});
</script>

<style scoped>
.coin-item {
  --background: #ffffff;
  --border-radius: 16px;
  --padding-start: 16px;
  --inner-padding-end: 16px;
  --min-height: 72px;
  margin: 8px 12px;
  border-radius: 16px;
  box-shadow: 0 4px 12px rgba(15, 23, 42, 0.06);
  border: 1px solid #e5e7eb;
}

.top-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 6px;
}

.name-symbol {
  display: flex;
  align-items: baseline;
  gap: 8px;
}

.name {
  font-weight: 600;
  font-size: 16px;
  color: #111827;
}

.symbol {
  font-size: 12px;
  text-transform: uppercase;
  padding: 2px 8px;
  border-radius: 999px;
  background: #f1f5f9;
  color: #4b5563;
  letter-spacing: 0.08em;
}

.rank-badge {
  font-size: 12px;
  font-weight: 600;
  padding: 4px 10px;
  border-radius: 999px;
  background: #eef2ff;
  color: #4338ca;
}

.price-row {
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  margin-top: 2px;
}

.price-label {
  font-size: 11px;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  color: #9ca3af;
}

.price-value {
  font-weight: 700;
  font-size: 16px;
  color: #047857;
}

.meta-row {
  display: flex;
  justify-content: space-between;
  margin-top: 6px;
}

.change-label {
  font-size: 12px;
  color: #9ca3af;
}

.change-value {
  font-size: 13px;
  font-weight: 600;
}

.change-value.positive {
  color: #16a34a;
}

.change-value.negative {
  color: #dc2626;
}
</style>
