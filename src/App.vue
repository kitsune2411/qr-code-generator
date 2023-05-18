<script setup lang="ts">
import { reactive, ref } from "vue";
import html2canvas from 'html2canvas'
import QrcodeVue from 'qrcode.vue'

const data = reactive({
  QrValue: '',
  size: 300,
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
  <header>
    <div class="">
      <p>QR Value</p>
      <!-- <input type="text" class="input-qr-value" id="qr-content" v-model="data.QrValue"> -->
      <textarea name="" id="" class="input-qr-value " cols="50" rows="5" v-model="data.QrValue" placeholder="input "></textarea>
    </div>
  </header>

  <main>
    <div class="" ref="printcontent" style="width: fit-content;">
      <qrcode-vue :value="data.QrValue" :size="data.size" level="H" />
    </div>
    <br>
    {{ data.QrValue }}
    <br>
    <button :onclick="printThis" style="background-color: green;">download</button>
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

.input-qr-value {
  margin-bottom: 1rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
