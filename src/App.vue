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
          <v-window-item v-for="type in Array.from(Object.keys(this.typeMap).map(k=>this.typeMap[k]))" :value="type">
            <v-row class="ma-auto">
              <v-col
                v-for="(element, i) in this.qrCodeData[type]"
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
import QRCodeDataView from "@/components/QRCodeDataView";
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
    typeMap: {
      "plugin": "plugins",
      "fusion": "fusions",
      "export": "exports",
      "resource": "resources"
    }
  }),
  methods: {
    addQrCodeComponent(qrCodeData) {
      const mappedType = this.typeMap[qrCodeData.type]

      console.log("qrKey: " + JSON.stringify(this.qrCodeData[mappedType]))

      this.qrCodeData[mappedType].push({
        "type": qrCodeData.type,
        "name": qrCodeData.name
      });

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
