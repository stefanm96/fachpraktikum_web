<template>
  <v-dialog
    v-model="show"
    width="500">
    <v-card>
      <v-card-title class="text-h6">
        {{ content.name }}
      </v-card-title>

      <v-card-subtitle class="text-capitalize">
        {{ content.definitionType }}
      </v-card-subtitle>

      <v-card-text class="text-center">
        <qrcode-vue class="align-center" :value="serializedContent()" :size="300" level="H"/>
        <p class="text-center text-grey">{{ serializedContent() }}</p>
      </v-card-text>

      <v-card-actions>
        <v-btn class="ml-auto" @click.stop="showEdit" variant="elevated" color="primary">Edit</v-btn>
        <v-btn @click.stop="this.delete" variant="elevated" color="error">Delete</v-btn>
        <v-btn @click.stop="cancel" variant="outlined">Cancel</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
import QrcodeVue from 'qrcode.vue'

export default {
  name: "QrCodeDetailView",
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
        required: modelValue => !!modelValue || "This field is required"
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
    showView: function () {
    },
    delete: function () {
      this.$emit('openDeleteQrCodeEvent')
      this.show = false
    },
    serializedContent: function () {
      const definitionType = this.content.definitionType
      const name = this.content.name
      const clazzTemplate = (this.content.clazz && ` class="${this.content.clazz}" `) || ""
      const typeTemplate = (this.content.type && ` type="${this.content.type}" `) || ""
      const locationTemplate = (this.content.location && ` location="${this.content.location}" `) || ""

      return `<${definitionType}-definition name="${name}"${clazzTemplate}${typeTemplate}${locationTemplate}/>`
    },
    cancel: function () {
      this.show = false
    },
    showEdit: function () {
      this.$emit('openEditQrCodeEvent')
      this.show = false
    }
  }
}

</script>

<style scoped>

</style>
