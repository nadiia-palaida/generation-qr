<template>
  <div class="form-wrap">

    <ValidationObserver v-if="!qrCode" ref="form" class="form-generate" v-slot="{ handleSubmit }">
      <form @submit.prevent="handleSubmit(createQr)" class="form-generate-textarea-wrap">
        <label class="form-generate-textarea-label">
          <span>Enter QR name</span>

          <ValidationProvider  rules="required" v-slot="{ errors }">
            <textarea v-model="qrText"  class="form-generate-textarea" name="name"/>
            <span class="error">{{ errors[0] }}</span>
          </ValidationProvider>
        </label>

        <button type="submit" class="btn">Generate QR</button>
      </form>
    </ValidationObserver>

    <div v-show="qrCode" class="qr-wrap">
      <div class="qr-bg">
        <div ref="qrcode" id="qrcode" class="qrcode"></div>
      </div>

      <button @click="download" class="btn qr-btn">Download</button>

      <button @click="clear" class="btn qr-btn">Generate new QR</button>
    </div>
  </div>
</template>

<script>
import * as QRCode from 'easyqrcodejs'
import { ValidationObserver, ValidationProvider } from 'vee-validate'

export default {
  name: "QrForm",
  components: {
    ValidationObserver, ValidationProvider
  },
  data() {
    return {
      qrText: '',
      qrCode: null,
      qrOptions: {
        colorLight: 'rgba(255, 255, 255, 0)',
        colorDark: '#000',
        crossOrigin: 'anonymous'
      }
    }
  },
  methods: {
    createQr() {
      this.$set(this, 'qrCode', new QRCode(this.$refs.qrcode, {
        text: this.qrText,
        ...this.qrOptions
      }))
    },
    clear() {
      this.qrCode.clear()
      this.qrCode = null
      this.qrText = ''
    },
    download() {
      let canvas = this.$refs.qrcode.children[0]
      let anchor = document.createElement("a");
      anchor.href = canvas.toDataURL("image/png");
      anchor.download = "IMAGE.PNG";
      anchor.click();
    }
  }
}
</script>

<style scoped>

</style>
