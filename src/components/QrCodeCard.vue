<template>
  <div>
    <v-card @click.stop="showQrCodeView">
      <v-card-title class="text-h6">
        {{ content.name }}
      </v-card-title>

      <v-card-subtitle class="text-capitalize">
        {{ content.definitionType }}
      </v-card-subtitle>

      <v-card-actions>
      </v-card-actions>
    </v-card>
  </div>
</template>

<script>
import QrcodeVue from 'qrcode.vue'

export default {
  name: "QrCodeCard",
  components: {
    QrcodeVue,
  },
  props: {
    content: Object
  },
  methods: {
    showQrCodeView() {
      console.log("show qrcode view " + this.content);
      this.$emit('showQrCodeView', this.content);
    }
  }
}
</script>

<style scoped>

</style>
