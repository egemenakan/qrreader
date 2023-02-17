<script setup>
import { ref } from "vue"
import { StreamBarcodeReader } from "vue-barcode-reader"

const decodedText = ref("")
const scanner = ref(null)

const onLoaded = () => {
  console.log("loaded")
  navigator.mediaDevices
    .getUserMedia({ video: true })
    .then(function (stream) {
      /* Kamera erişimine izin verilmişse, stream değişkeni kullanarak
    kamerayı kontrol edebilirsiniz */
    })
    .catch(function (error) {
      /* Kamera erişimine izin verilmemişse, hata mesajını burada 
    yakalayabilirsiniz */
    })
}

// create a method to start the scanner
const startScanner = () => {
  scanner.value.start()
  alert("Scanner started")
}

// create a method to stop the scanner
const stopScanner = () => {
  scanner.value.codeReader.stream.getTracks().forEach((track) => track.stop())
}

const onDecode = (text) => {
  decodedText.value = text
  console.log(text)
}
</script>

<template>
  <StreamBarcodeReader
    ref="scanner"
    class="h-96 overflow-hidden grid place-items-center"
    @decode="onDecode"
    @loaded="onLoaded"
  ></StreamBarcodeReader>
  <h2>The decoded value in QR/barcode is</h2>
  <h2>{{ decodedText }}</h2>
  <button @click="startScanner">Start</button>
  <br />
  <button @click="stopScanner">Stop</button>
</template>

<style scope>
/* .scanner-container > div {
  width: 90%;
  display: grid;
  place-items: center;
}
.overlay-element {
  width: 100%;
  height: 100% !important;
  max-width: 640px;
}

.laser {
  margin-left: 0% !important;
  max-width: 384px;
} */
</style>
