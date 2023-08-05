

<template>
  <div class="scan-qr-layout">

    <div class="top">
      <h1>{{ judul }}</h1>
    </div>
    <div class="middle">
      <qrcode-stream v-if="showScanner" @decode="onDecode" @init="onInit" />
      <div v-else class="scanner-inactive">
        Klik tombol di bawah untuk memulai pemindaian QR.
      </div>
    </div>
    <div class="bottom">
      <button @click="toggleScanner" class="scan-button">
        {{ showScanner ? 'Berhenti Scan' : 'Mulai Scan' }}
      </button>
    </div>
  </div>
</template>

<script>
import { QrcodeStream } from 'vue-qrcode-reader';

export default {
  components: {
    QrcodeStream,
  },
  data() {
    return {
      judul: 'Aplikasi Scan QR',
      showScanner: false,
    };
  },
  methods: {
    onDecode(decodedString) {
      // Logika untuk menangani hasil dari pemindaian QR akan ditempatkan di sini
      alert('Hasil pemindaian QR:'.concat(decodedString));
    },
    onInit(stream) {
      this.showScanner = true;
      if (stream) {
        const tracks = stream.getVideoTracks();
        if (tracks.length > 0) {
          tracks[0].applyConstraints({
            facingMode: 'environment', // Atur untuk kamera belakang (opsional)
          });
        }
      }
    },
    toggleScanner() {
      this.showScanner = !this.showScanner;
    },
  },
};
</script>






<style scoped>
.scan-qr-layout {
  display: flex;
  flex-direction: column;
  height: 100vh;
}

.top {
  text-align: center;
  padding: 20px;
  background-color: #E60000;
  color: white;
  border-bottom-left-radius: 15px;
  border-bottom-right-radius: 15px;
}

.middle {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  flex-grow: 1;
  background-color: #F47B00;
  border-radius: 15px;
}

.bottom {
  text-align: center;
  padding: 20px;
  background-color: #FFC400;
  border-top-left-radius: 15px;
  border-top-right-radius: 15px;
}

.scan-button {
  background-color: #004D7A;
  color: white;
  border: none;
  border-radius: 10px;
  padding: 10px 20px;
  font-size: 18px;
  cursor: pointer;
}
</style>
