<template>
  <v-dialog
    v-model="show"
    width="500">
    <v-card>
      <v-card-title class="headline" primary-title>
        Edit Element
      </v-card-title>
      <v-card-text class="pa-5">
        <v-form ref="sendForm" v-model="valid" lazy-validation>
          <v-select disabled outlined :items="typeItems" v-model="definitionType"
                    label="Type"></v-select>
          <v-text-field disabled outlined v-model="name"
                        label="Name"></v-text-field>
          <v-text-field v-if="definitionType !== 'resource'" outlined v-model="clazz" label="Class"></v-text-field>
          <v-text-field v-if="definitionType === 'resource'" outlined v-model="type"
                        label="Type"></v-text-field>
          <v-text-field v-if="definitionType === 'resource'" outlined v-model="location"
                        label="Location"></v-text-field>
        </v-form>
      </v-card-text>
      <v-card-actions class="pa-5">
        <v-btn class="ml-auto" autofocus @click.stop="submit" variant="elevated"
               color="primary">Save
        </v-btn>
        <v-btn @click.stop="cancel" variant="outlined">Cancel</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  name: "EditQrCodeDialog",
  props: {
    modelValue: Boolean,
    content: Object
  },
  data() {
    return {
      name: "",
      definitionType: "",
      clazz: "",
      type: "",
      location: "",
      typeItems: ['plugin', 'fusion', 'export', 'resource']
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
    },
  },
  methods: {
    submit: function () {
      const qrCodeData = {
        definitionType: this.definitionType,
        name: this.name,
        clazz: this.clazz,
        type: this.type,
        location: this.location
      }

      this.$emit('editQrCodeEvent', qrCodeData)
      this.definitionType = ""
      this.name = ""
      this.clazz = ""
      this.type = ""
      this.location = ""
      this.show = false
    },
    cancel: function () {
      this.show = false
      this.definitionType = ""
      this.name = ""
      this.clazz = ""
      this.type = ""
      this.location = ""
    },
    showView: function (qrCodeData) {
      this.definitionType = qrCodeData.definitionType
      this.name = qrCodeData.name
      this.clazz = qrCodeData.clazz
      this.type = qrCodeData.type
      this.location = qrCodeData.location
    }
  }
}

</script>
