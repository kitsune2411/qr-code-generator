<script setup lang="ts">
import { reactive, ref } from "vue";
import html2canvas from 'html2canvas'
import QrcodeVue from 'qrcode.vue'

const data = reactive({
  QrValue: '',
  size: 325,
})

const printcontent = ref<HTMLDivElement>()

async function printThis() {
  console.log("printing..");
  const el = printcontent.value;

  console.log('el', el);
  if (!el) return;

  const options = {
    type: "dataURL"
  };

  const printCanvas = await html2canvas(el);

  const link = document.createElement("a");
  link.setAttribute("download", "myQR.png");
  link.setAttribute(
    "href",
    printCanvas
      .toDataURL("image/png")
      .replace("image/png", "image/octet-stream")
  );
  link.click();

  console.log("done");
}

</script>

<template>
  <div class="input-container">
    <p>QR Value</p>
    <!-- <input type="text" class="input-qr-value" id="qr-content" v-model="data.QrValue"> -->
    <textarea name="" id="" class="input-qr-value " rows="5" maxlength="4296" v-model="data.QrValue"
      placeholder="input "></textarea>
  </div>


  <div class="qr-container">

    <div class="Qr-code">
      <div class="" ref="printcontent" style="width: fit-content;">
        <qrcode-vue id="vueQrCode" :value="data.QrValue" :size="data.size" level="H" />
      </div>
    </div>
    <br>
    <div class="ar-value-text" v-if="data.QrValue !== ''">
      <p style="max-width: 325px;word-wrap: break-word;">{{ data.QrValue }}</p>
      <br>
    </div>
    <button class="download-button" :onclick="printThis">
      <svg class="download-icon" fill="white" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20">
        <path d="M13 8V2H7v6H2l8 8 8-8h-5zM0 18h20v2H0v-2z" />
      </svg>
      download
    </button>
  </div>
</template>

<style scoped>
header {
  line-height: 1.5;
}


/* dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500 */

.input-qr-value {
  margin-bottom: 1rem;
  width: 100%;
  background-color: #4a5568;
  border-color: #718096;
  border-radius: 0.25rem;
  color: white;
}
.input-qr-value::placeholder {
  color: #cbd5e0;

}
.qr-container {
  width: 325px;
}

.Qr-code {
  width: 100;
}

#vueQrCode {
  padding: 0;
  margin: auto;
  display: block;
}

.download-button {
  width: 100%;
  border-radius: 15px;
  padding: 0.5rem 1rem;
  background-color: #38a169;
  color: white;
}

.download-button:hover {
  background-color: #2f855a;
}

.download-icon {
  width: 1rem;
  height: 1rem;
  color: white;
}

@media (min-width: 1024px) {
  .input-container {
    padding-right: calc(var(--section-gap) / 2);
    width: 40vw;
  }

}
</style>
