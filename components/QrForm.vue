<template>
  <div class="form-wrap">
    <ValidationObserver v-if="!qrUrl" ref="form" class="form-generate" v-slot="{ handleSubmit }">
      <form @submit.prevent="handleSubmit(createQr)" class="form-generate-textarea-wrap">
        <label class="form-generate-textarea-label">
          <span>Enter QR name</span>

          <ValidationProvider  rules="required" v-slot="{ errors }">
            <textarea v-model="qrText"  class="form-generate-textarea" name="name"/>
            <span class="error">{{ errors[0] }}</span>
          </ValidationProvider>
        </label>

        <button  type="submit" class="btn">Generate QR</button>
      </form>
    </ValidationObserver>

    <div v-else class="qr-wrap">
      <div class="qr-bg">
        <img class="qr-img" :src="qrUrl" alt="">
      </div>
    </div>
  </div>
</template>

<script>
import QRCode from 'qrcode'
import { ValidationObserver, ValidationProvider } from 'vee-validate';

export default {
  name: "QrForm",
  components: {
    ValidationObserver, ValidationProvider
  },
  data() {
    return {
      qrUrl: '',
      qrText: ''
    }
  },
  methods: {
    createQr() {
        QRCode.toDataURL(this.qrText)
          .then(url => {
            this.qrUrl = url
          })
    }
  }
}
</script>

<style scoped>

</style>
