<script setup>
import { ref } from "vue"
import { QrcodeStream } from "vue3-qrcode-reader"

const result = ref("")
const error = ref("")

function onDecode(result) {
  result.value = result
}

async function onInit(promise) {
  try {
    await promise
  } catch (error) {
    if (error.value.name === "NotAllowedError") {
      error.value = "ERROR: you need to grant camera access permission"
    } else if (error.value.name === "NotFoundError") {
      error.value = "ERROR: no camera on this device"
    } else if (error.value.name === "NotSupportedError") {
      error.value = "ERROR: secure context required (HTTPS, localhost)"
    } else if (error.value.name === "NotReadableError") {
      error.value = "ERROR: is the camera already in use?"
    } else if (error.value.name === "OverconstrainedError") {
      error.value = "ERROR: installed cameras are not suitable"
    } else if (error.value.name === "StreamApiNotSupportedError") {
      error.value = "ERROR: Stream API is not supported in this browser"
    } else if (error.value.name === "InsecureContextError") {
      error.value = "ERROR: Camera access is only permitted in secure context. Use HTTPS or localhost rather than HTTP."
    } else {
      error.value = `ERROR: Camera error (${error.value.name})`
    }
  }
}
</script>

<template>
  <div>
    <p class="error">{{ error }}</p>

    <p class="decode-result p-0">
      Last result: <b>{{ result }}</b>
    </p>

    <qrcode-stream @decode="onDecode" @init="onInit" />
  </div>
</template>
<style scoped>
.error {
  font-weight: bold;
  color: red;
}
</style>
