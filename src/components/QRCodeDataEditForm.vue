<template>
<!--  TODO: implement edit dialog -->
  <v-dialog
      v-model="show"
      width="500">
    <v-card>
      <v-card-title class="headline" primary-title>
        Create Element
      </v-card-title>
      <v-card-text class="pa-5">
        <v-form ref="sendForm" v-model="valid" lazy-validation>
          <v-select outlined :items="typeItems" v-model="type" label="Type"></v-select>
          <v-text-field outlined v-model="name" label="Name"></v-text-field>
          <v-textarea outlined v-model="input" label="Input"></v-textarea>
        </v-form>
      </v-card-text>
      <v-card-actions class="pa-5">
        <v-btn class="ml-auto" @click.stop="submit" variant="outlined" color="primary">Create</v-btn>
        <v-btn @click.stop="cancel" variant="outlined">Cancel</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
import QRCode from "qrcode";

export default {
  name: "QRCodeDataEditForm",
  props: {
    modelValue: Boolean
  },
  data() {
    return {
      name: "",
      type: "",
      typeItems: ['plugin', 'fusion', 'export', 'resource', 'parameter', 'flow-source'],
      input: "",
      valid: true,
      rules: {
        required: modelValue => !!modelValue || "This field is required",
        email: v => /.+@.+\..+/.test(v) || "Must be a valid email"
      }
    }
  },
  computed: {
    show: {
      get () {
        return this.modelValue
      },
      set (modelValue) {
        this.$emit('update:modelValue', modelValue)
      }
    }
  },
  methods: {
     submit: function () {
       let canvas = document.getElementById('canvas')
       console.log(canvas)
       console.log(this.input)
       let qrCodeUrl = null

       QRCode.toDataURL(this.input,{}, function (error, url) {
         if (error) console.error(error)
         qrCodeUrl = url;
       })

       if (qrCodeUrl) {
         const qrCodeData = {
           type: this.type,
           name: this.name,
           qrCodeUrl
         }

         this.$emit('addQrCodeEvent', qrCodeData)
         this.type = ""
         this.name = ""
         this.input = ""
         this.show = false
       }
     },
    cancel: function () {
        this.type = ""
        this.name = ""
        this.input = ""
        this.show = false
    }
  }
}

</script>

<style scoped>

</style>
