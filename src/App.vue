<template>
  <v-app>
    <v-app-bar
      color="primary"
    >
      <v-app-bar-title>GMAF QR-Code Generator</v-app-bar-title>

      <template v-slot:append>
        <v-btn variant="outlined" @click.stop="showQRCodeDataForm=true">
          <v-icon>mdi-plus</v-icon>
          create element
        </v-btn>
      </template>

    </v-app-bar>

    <QRCodeDataForm v-model="showQRCodeDataForm" @addQrCodeEvent="addQrCodeComponent"></QRCodeDataForm>
    <QRCodeDataView ref="dataView" v-model="showQRCodeDataView" :content="selectedElement"></QRCodeDataView>

    <v-main class="bg-grey-lighten-3 grey lighten-2">
      <v-tabs
        v-model="typeTab"
        bg-color="primary"
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
          <v-window-item v-for="definitionType in Array.from(Object.keys(this.definitionTypeMap).map(k=>this.definitionTypeMap[k]))" :value="definitionType">
            <v-row class="ma-auto">
              <v-col
                v-for="(element, i) in this.qrCodeData[definitionType]"
                :key="`${name}${i}`"
                cols="6"
                md="3"
              >
                <QrCodeCard :id="`${name}${i}`" :content="element" @showQrCodeView="showQrCodeView"/>
              </v-col>
            </v-row>
          </v-window-item>
        </v-window>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import QRCodeDataForm from "@/components/QRCodeDataCreateForm";
import QRCodeDataView from "@/components/QRCodeDetailView";
import qrCodeData from "./assets/qrCodeData.json";
import QrCodeCard from "@/components/QrCodeCard";

export default {
  components: {QrCodeCard, QRCodeDataForm, QRCodeDataView},
  data: () => ({
    typeTab: null,
    drawer: null,
    showQRCodeDataForm: false,
    showQRCodeDataView: false,
    selectedElement: null,
    qrCodeData: qrCodeData,
    definitionTypeMap: {
      "plugin": "plugins",
      "fusion": "fusions",
      "export": "exports",
      "resource": "resources"
    }
  }),
  methods: {
    addQrCodeComponent(qrCodeData) {
      const mappedDefinitionType = this.definitionTypeMap[qrCodeData.definitionType]

      console.log("qrKey: " + JSON.stringify(this.qrCodeData[mappedDefinitionType]))

      const nonEmptyValues = Object.keys(qrCodeData).filter(key => qrCodeData[key]);
      const newObj = nonEmptyValues.reduce((acc, key) =>
        ({ ...acc, [key]: qrCodeData[key] }), {});

      this.qrCodeData[mappedDefinitionType].push(newObj);

      console.log(this.qrCodeData);
    },
    showQrCodeView(qrCodeData) {
      console.log("show qrcode view " + JSON.stringify(qrCodeData))
      this.selectedElement = qrCodeData
      this.showQRCodeDataView = true

      const dataView = this.$refs.dataView;
      dataView.showView(qrCodeData)
    }
  }
}
</script>
