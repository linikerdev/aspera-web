<template>
  <div id="app">
    <h1>ola mundo</h1>
    <button @click="abreaspera">Abre aspera</button>
  </div>
</template>

<script>
import { initAspera, asperaWeb } from "./plugin/aspera";
export default {
  name: "app",
  methods: {
    abreaspera() {
      initAspera({
        sdkLocation: "//d3gcli72yxqn2z.cloudfront.net/connect/v4"
      });
      asperaWeb.showSelectFileDialog({ success: this.ifSuccess });
    },

    ifSuccess({ dataTransfer }) {
      const transferSpec = {
        paths: dataTransfer.files,
        remote_host: "demo.asperasoft.com",
        remote_user: "asperaweb",
        remote_password: "demoaspera",
        direction: "send",
        target_rate_kbps: 500,
        resume: "sparse_checksum",
        destination_root: "Upload"
      };

      var files = dataTransfer.files;

      for (var i = 0, length = files.length; i < length; i += 1) {
        transferSpec.paths.push({ source: files[i].name });
      }

      if (transferSpec.paths.length === 0) {
        return;
      }
      let start = asperaWeb.startTransfer(transferSpec, {
        allow_dialogs: true
      });

      asperaWeb.addEventListener("transfer", (event, obj) => {
        console.log(obj);
      });
      console.log("start", start);
      console.log("transferSpec", transferSpec);
    }
  }
};
</script>

<style>
</style>
