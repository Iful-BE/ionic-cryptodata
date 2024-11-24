<template>
    <ion-page>
      <ion-header :translucent="true">
        <ion-toolbar>
          <ion-title>Cryptokuy</ion-title>
        </ion-toolbar>
      </ion-header>
      <ion-content :fullscreen="true">
        <div id="container">
          <div class="ion-text-center">
            <ion-button expand="block" @click="fetchData">Get Data</ion-button>
          </div>
          <ion-grid>
            <ion-row>
              <ion-col size="4"><strong>Name</strong></ion-col>
              <ion-col size="4"><strong>Symbol</strong></ion-col>
              <ion-col size="4"><strong>Harga USD</strong></ion-col>
            </ion-row>
            <ion-row v-for="crypto in cryptos" :key="crypto.id">
              <ion-col size="4">{{ crypto.name }}</ion-col>
              <ion-col size="4">{{ crypto.symbol }}</ion-col>
              <ion-col size="4">{{ crypto.price_usd }}</ion-col>
            </ion-row>
          </ion-grid>
        </div>
      </ion-content>
    </ion-page>
  </template>
  
  
  <script setup lang="ts">
import { IonContent, IonHeader, IonPage, IonTitle, IonToolbar } from '@ionic/vue';import { defineComponent, ref } from "vue";
import axios from "axios";

interface Crypto {
  id: number;
  name: string;
  symbol: string;
  price_usd: string;
}
const cryptos = ref<Crypto[]>([]);
const fetchData = async () => {
  try {
    const response = await axios.get("https://api.coinlore.net/api/tickers/");
    cryptos.value = response.data.data.map((crypto: any) => ({
      id: crypto.id,
      name: crypto.name,
      symbol: crypto.symbol,
      price_usd: parseFloat(crypto.price_usd).toFixed(2),
    }));
  } catch (error) {
    console.error("Error fetching data:", error);
  }
};

  </script>

<style>
#container {
  margin: 16px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

ion-grid {
  width: 100%;
  max-width: 600px;
  border: 1px solid #ccc;
  border-collapse: collapse;
}

ion-row {
  border-bottom: 1px solid #ccc;
}

ion-col {
  text-align: center;
  padding: 8px;
}
</style>

  
 