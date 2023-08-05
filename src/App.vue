<template>
  <div class="scan-qr-layout">
    <div class="top">
      <h1>{{ judul }}</h1>
    </div>
    <div class="middle">
      <video ref="video" autoplay></video>
    </div>
    <div class="bottom">
      <button @click="captureImage" class="scan-button">Ambil Gambar</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      judul: 'Aplikasi Kamera',
      mediaStream: null,
      videoWidth: 640,
      videoHeight: 480,
    };
  },
  mounted() {
    this.startCamera();
  },
  methods: {
    startCamera() {
      navigator.mediaDevices
          .getUserMedia({ video: true })
          .then(stream => {
            this.mediaStream = stream;
            this.$refs.video.srcObject = stream;
          })
          .catch(error => {
            console.error('Gagal mengakses kamera:', error);
          });
    },
    captureImage() {
      const canvas = document.createElement('canvas');
      canvas.width = this.videoWidth;
      canvas.height = this.videoHeight;
      const context = canvas.getContext('2d');
      context.drawImage(this.$refs.video, 0, 0, canvas.width, canvas.height);
      const imageData = canvas.toDataURL('image/jpeg'); // Gambar diubah menjadi base64 string
      console.log('Image Data:', imageData);

      // pindah untuk nampilin gambar
      const newTab = window.open('', '_blank');
      newTab.document.write(`<img src="${imageData}" alt="Gambar yang Diambil"/>`);
      newTab.document.close();
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
