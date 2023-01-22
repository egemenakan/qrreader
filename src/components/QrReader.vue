<script setup>
import { ref } from "vue"
import { StreamBarcodeReader } from "vue-barcode-reader"

const decodedText = ref("")
const scanner = ref(null)

const onLoaded = () => {
  console.log("loaded")
  setTimeout(() => {
    scanner.value.codeReader.stream.getTracks().forEach(function (track) {
      track.stop()
    })
  }, 2000)
  setTimeout(() => {
    scanner.value.codeReader.stream.getTracks().forEach(function (track) {
      track.play()
    })
  }, 5000)
  // document.querySelector("video").setAttribute("width", "100%")
}

const onDecode = (text) => {
  decodedText.value = text
  console.log(text)
}
</script>

<template>
  <StreamBarcodeReader ref="scanner" class="w-full" @decode="onDecode" @loaded="onLoaded"></StreamBarcodeReader>
  <h2>The decoded value in QR/barcode is</h2>
  <h2>{{ decodedText }}</h2>
  adssd
</template>

<style scoped></style>
