<template>
  <!-- every page created wrap in div with this class ion-page -->
  <div class="ion-page">
    <ion-header>
      <ion-toolbar>
        <ion-title>ZipInfo</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content class="ion-padding">
      <ZipSearch @get-zip="getZipInfo" />
      <ZipInfo :info="info" />
      <ClearInfo :info="info" @clear-info="clearInfo" />
    </ion-content>
  </div>
</template>

<script>
// @ is an alias to /src
import ZipSearch from "../components/ZipSearch";
import ZipInfo from "../components/ZipInfo";
import ClearInfo from "../components/ClearInfo";
export default {
  name: "home",
  components: { ZipSearch, ZipInfo, ClearInfo },
  data: function() {
    return {
      info: null
    };
  },
  methods: {
    getZipInfo: async function(zip) {
      const res = await fetch(`https://api.zippopotam.us/us/${zip}`);
      if (res.status == 404) {
        this.showAlert();
      }
      this.info = await res.json();
    },
    showAlert: function() {
      return this.$ionic.alertController
        .create({
          header: "Enter Zipcode",
          message: "Please enter a valid US zipcode",
          buttons: ["OK"]
        })
        .then(a => a.present());
    },
    clearInfo: function() {
      this.info = null;
    }
  }
};
</script>
