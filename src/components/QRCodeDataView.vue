<template>
  <v-dialog
    v-model="show"
    width="500">
    <v-card>
      <v-card-title class="text-h6">
        {{ content.name }}
      </v-card-title>

      <v-card-subtitle class="text-capitalize">
        {{ content.type }}
      </v-card-subtitle>

      <v-card-text class="text-center">
        <qrcode-vue class="align-center" :value="JSON.stringify(content)" :size="300" level="H"/>
      </v-card-text>

      <v-card-actions>
        <v-btn class="ml-auto" @click.stop="submit" variant="elevated" color="primary">Edit</v-btn>
        <v-btn @click.stop="deleteData" variant="elevated" color="error">Delete</v-btn>
        <v-btn @click.stop="cancel" variant="outlined">Cancel</v-btn>
        <!-- TODO: implement edit, delete -->
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
import QrcodeVue from 'qrcode.vue'

export default {
  name: "QRCodeDataView",
  components: {
    QrcodeVue,
  },
  props: {
    modelValue: Boolean,
    content: Object
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
      get() {
        return this.modelValue
      },
      set(modelValue) {
        this.$emit('update:modelValue', modelValue)
      }
    }
  },
  methods: {
    submit: function () {
      console.log("submit triggered!")
    },
    showView: function () {
      console.log("show View inner")
    },
    deleteData: function () {
      console.log("delete triggered")
    },
    cancel: function () {
      this.show = false
    }
  }
}

</script>

<style scoped>

</style>
