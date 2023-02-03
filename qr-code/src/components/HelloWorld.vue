<template>
  <div class="qr-section">
    <button v-if="!isClicked" @click="startQRScanning()">QR okut</button>
    <div id="reader" class="qr-reader"></div>
    <div v-if="decodedText != ''" class="qr-section__result">
      <h3 class="qr-section__title">New QR Code Detected!</h3>
      <p><strong>Text:</strong> {{ decodedText }}</p>
    </div>
  </div>
</template>

<script>
import { Html5QrcodeScanner } from "html5-qrcode";

export default {
  name: "HelloWorld",
  data() {
    return {
      qrScanner: {},
      decodedText: "",
      isClicked: false,
    };
  },
  created() {},
  methods: {
    onScanSuccess(decodedText, decodedResult) {
      this.decodedText = decodedText;
      console.log(`Code matched = ${decodedText}`, decodedResult);
      this.qrScanner.pause(true);
    },
    onScanFailure(error) {
      console.warn(`Code scan error = ${error}`);
    },
    startQRScanning() {
      this.isClicked = true;
      this.qrScanner = new Html5QrcodeScanner(
        "reader",
        { fps: 10, qrbox: { width: 150, height: 150 } },
        false
      );
      this.qrScanner.render(this.onScanSuccess, this.onScanFailure);
    },
  },
};
</script>

<style scoped>
.qr-section__result {
  border: 1px solid black;
}
.qr-section__title {
  border-bottom: 1px solid black;
  background-color: 1px solid #cbd5e1;
}
.qr-reader {
  width: 300px;
}
</style>
