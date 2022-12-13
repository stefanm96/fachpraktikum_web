<template>
  <v-app>
    <v-app-bar
      color="primary"

    >
      <v-app-bar-title>GMAF QR-Code Generator</v-app-bar-title>

      <template v-slot:append>
        <v-btn variant="outlined" @click.stop="showQRCodeDataForm=true">
          <v-icon>mdi-plus</v-icon>
          create QR-Code
        </v-btn>
      </template>

    </v-app-bar>

    <CreateQrCodeDialog
      v-model="showQRCodeDataForm"
      :qr-code-name-list="qrCodeData.plugins.map(plugin => plugin.name)"
      @addQrCodeEvent="addQrCode"
    />

    <QrCodeDetailView
      ref="detailView"
      v-model="showQrCodeDetailView"
      :content="selectedElement"
      @openEditQrCodeEvent="openEditQrCodeDialog"
      @openDeleteQrCodeEvent="openDeleteQrCodeDialog"
    />

    <EditQrCodeDialog
      ref="editView"
      v-model="showEditQrCodeDialog"
      :content="selectedElement"
      @editQrCodeEvent="editQrCode"
    />

    <DeleteQrCodeDialog
      ref="deleteView"
      v-model="showDeleteQrCodeDialog"
      @deleteQrCodeEvent="deleteQrCode"
    />

    <v-snackbar
      v-model="showSnackbar"
      :timeout="2000"
    >
      {{ snackbarText }}
    </v-snackbar>

    <v-main class="bg-grey-lighten-3 grey lighten-2">
      <v-tabs
        v-model="typeTab"
        bg-color="primaryLight"
        slider-color="orange"
        fixed-tabs
        grow
      >
        <v-tab value="plugins">Plugins</v-tab>
        <v-tab value="fusions">Fusions</v-tab>
        <v-tab value="exports">Exports</v-tab>
        <v-tab value="resources">Resources</v-tab>
      </v-tabs>
      <v-container>
        <v-window v-model="typeTab">
          <v-window-item
            v-for="definitionType in Array.from(Object.keys(this.definitionTypeMap).map(k=>this.definitionTypeMap[k]))"
            :value="definitionType">
            <v-row class="ma-auto">
              <v-col
                v-for="(element, i) in this.qrCodeData[definitionType]"
                :key="`${name}${i}`"
                cols="6"
                md="3"
              >
                <QrCodeCard :id="`${name}${i}`" :content="element" @showQrCodeView="openQrCodeDetailView"/>
              </v-col>
            </v-row>
          </v-window-item>
        </v-window>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import CreateQrCodeDialog from "@/components/CreateQrCodeDialog";
import QrCodeDetailView from "@/components/QRCodeDetailView";
import qrCodeData from "./assets/qrCodeData.json";
import QrCodeCard from "@/components/QrCodeCard";
import EditQrCodeDialog from "@/components/EditQrCodeDialog";
import DeleteQrCodeDialog from "@/components/DeleteQrCodeDialog";

export default {
  components: {DeleteQrCodeDialog, QrCodeCard, CreateQrCodeDialog, QrCodeDetailView, EditQrCodeDialog},
  data: () => ({
    typeTab: null,
    drawer: null,
    showQRCodeDataForm: false,
    showQrCodeDetailView: false,
    showEditQrCodeDialog: false,
    showDeleteQrCodeDialog: false,
    selectedElement: null,
    qrCodeData: qrCodeData,
    snackbarText: "",
    showSnackbar: false,
    definitionTypeMap: {
      "plugin": "plugins",
      "fusion": "fusions",
      "export": "exports",
      "resource": "resources"
    }
  }),
  methods: {
    addQrCode(qrCodeData) {
      const mappedDefinitionType = this.definitionTypeMap[qrCodeData.definitionType]

      console.log("qrKey: " + JSON.stringify(this.qrCodeData[mappedDefinitionType]))

      const nonEmptyValues = Object.keys(qrCodeData).filter(key => qrCodeData[key]);
      const newObj = nonEmptyValues.reduce((acc, key) =>
        ({...acc, [key]: qrCodeData[key]}), {});

      this.qrCodeData[mappedDefinitionType].push(newObj);

      this.openSnackbar("Successfully created QR-Code!")

      console.log(this.qrCodeData);
    },
    editQrCode(qrCodeData) {
      const mappedDefinitionType = this.definitionTypeMap[qrCodeData.definitionType]

      console.log("qrKey: " + JSON.stringify(this.qrCodeData[mappedDefinitionType]))

      const nonEmptyValues = Object.keys(qrCodeData).filter(key => qrCodeData[key]);
      const newObj = nonEmptyValues.reduce((acc, key) =>
        ({...acc, [key]: qrCodeData[key]}), {});

      const index = this.qrCodeData[mappedDefinitionType].findIndex(definition => definition.name === qrCodeData.name)

      this.qrCodeData[mappedDefinitionType].splice(index, 1, newObj)

      this.openSnackbar("Successfully edited QR-Code!")

      console.log("QR CODE DATA: " + JSON.stringify(this.qrCodeData))
    },
    deleteQrCode() {
      const mappedDefinitionType = this.definitionTypeMap[this.selectedElement.definitionType]

      console.log("qrKey: " + JSON.stringify(this.qrCodeData[mappedDefinitionType]))

      const index = this.qrCodeData[mappedDefinitionType].findIndex(definition => definition.name === this.selectedElement.name)

      this.qrCodeData[mappedDefinitionType].splice(index, 1)

      this.openSnackbar("Successfully deleted QR-Code!")

      console.log("QR CODE DATA: " + JSON.stringify(this.qrCodeData))
    },
    openQrCodeDetailView(qrCodeData) {
      console.log("show qrcode view " + JSON.stringify(qrCodeData))
      this.selectedElement = qrCodeData
      this.showQrCodeDetailView = true

      const detailView = this.$refs.detailView;
      detailView.showView(qrCodeData)
    },
    openEditQrCodeDialog() {
      console.log("show edit qrcode dialog " + this.selectedElement)
      this.showEditQrCodeDialog = true

      const editView = this.$refs.editView;
      editView.showView(this.selectedElement)
    },
    openDeleteQrCodeDialog() {
      console.log("show edit qrcode dialog " + this.selectedElement)
      this.showDeleteQrCodeDialog = true
    },
    openSnackbar(text) {
      this.snackbarText = text
      this.showSnackbar = true
    }
  }
}
</script>
