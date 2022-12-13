<template>
<!--  TODO: implement edit dialog -->
  <v-dialog
      v-model="show"
      width="500">
    <v-card>
      <v-card-title class="headline" primary-title>
        Delete Element
      </v-card-title>
      <v-card-text class="pa-5">
        <p>Are you sure want to delete this element?</p>
      </v-card-text>
      <v-card-actions class="pa-5">
        <v-btn class="ml-auto" @click.stop="this.delete" variant="elevated" color="error">Delete</v-btn>
        <v-btn @click.stop="cancel" variant="outlined">Cancel</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  name: "DeleteQrCodeDialog",
  props: {
    modelValue: Boolean,
    content: Object
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
    delete: function () {
      this.show = false
      this.$emit('deleteQrCodeEvent')
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
