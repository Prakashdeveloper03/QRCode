<template>
  <div class="min-h-screen bg-blue-600 flex justify-center items-center">
    <div class="bg-white rounded-lg p-8 flex flex-col items-center space-y-6">
      <h1 class="text-2xl font-bold">QR Code Generator</h1>
      <p class="text-gray-700">Paste a URL or enter text to generate QR code</p>
      <div class="flex flex-col items-center space-y-4 w-full">
        <input
          type="text"
          class="w-full px-4 py-3 border border-gray-300 rounded-lg outline-none transition duration-150 ease-in-out focus:ring-2 focus:ring-blue-500 focus:border-transparent"
          :value="inputValue"
          @input="handleInputChange"
          placeholder="Enter text or URL"
        />
        <button
          class="bg-blue-500 text-white px-4 py-3 rounded-lg font-bold transition duration-150 ease-in-out hover:bg-blue-600"
          @click="handleGenerateButtonClick"
        >
          Generate QR Code
        </button>
      </div>
      <div v-if="qrCodeValue" class="flex justify-center items-center mt-8">
        <div class="w-1/2 bg-white shadow-lg rounded-lg overflow-hidden">
          <img :src="qrCodeValue" alt="QR Code" class="w-full h-auto" />
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'

export default defineComponent({
  name: 'QRCodeGenerator',
  setup() {
    const inputValue = ref('')
    const qrCodeValue = ref('')

    const handleInputChange = (event: InputEvent) => {
      const target = event.target as HTMLInputElement
      inputValue.value = target.value
      qrCodeValue.value = ''
    }

    const handleGenerateButtonClick = async () => {
      if (!inputValue.value.trim()) return

      try {
        const response = await fetch(
          `https://api.qrserver.com/v1/create-qr-code/?size=300x300&data=${inputValue.value}`
        )
        if (response.ok) {
          const blob = await response.blob()
          qrCodeValue.value = URL.createObjectURL(blob)
        }
      } catch (error) {
        console.error(error)
      }
    }

    return {
      inputValue,
      qrCodeValue,
      handleInputChange,
      handleGenerateButtonClick
    }
  }
})
</script>

<style type="postcss">
@import './style.css';
body {
  font-family: 'Poppins', sans-serif;
  margin: 0;
}
</style>
