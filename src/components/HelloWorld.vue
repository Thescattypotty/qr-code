<script setup>
import { ref, onMounted } from 'vue'
import QRCode from 'qrcode'

defineProps({
    msg: String,
})

const qrCodeUrl = ref('https://vitejs.dev')
const qrCodeDataUrl = ref('')
const errorMessage = ref('')

const generateQRCode = async (url) => {
    try {
        qrCodeDataUrl.value = await QRCode.toDataURL(url)
        errorMessage.value = ''
    } catch (err) {
        console.error(err)
        errorMessage.value = 'Failed to generate QR Code. Please try again.'
    }
}

onMounted(() => {
    generateQRCode(qrCodeUrl.value)
})
</script>

<template>
    <div class="qr-code-generator">
        <h2>QR Code Generator</h2>
        <input 
            v-model="qrCodeUrl" 
            @input="generateQRCode(qrCodeUrl)" 
            placeholder="Enter URL" 
            class="url-input"
        />
        <div v-if="errorMessage" class="error-message">{{ errorMessage }}</div>
        <div v-if="qrCodeDataUrl" class="qr-code">
            <img :src="qrCodeDataUrl" alt="QR Code" />
        </div>
    </div>
</template>

<style scoped>
.qr-code-generator {
    text-align: center;
    margin: 20px;
}

.url-input {
    width: 80%;
    padding: 10px;
    margin: 10px 0;
    font-size: 16px;
}

.error-message {
    color: red;
    margin: 10px 0;
}

.qr-code img {
    margin-top: 20px;
    max-width: 100%;
    height: auto;
}
</style>