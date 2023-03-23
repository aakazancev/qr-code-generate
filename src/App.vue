<template>
  <div id="app">
    <h1>Генератор QR-кода SVG</h1>
    <div>
      <label for="inputText">Введите текст для QR-кода:</label>
      <input id="inputText" class="input" v-model="text" />
    </div>
    <div v-if="text" v-html="svgQRCode" />
    <div class="btn-group">
      <button v-if="text" @click="downloadSVG">Скачать SVG</button>
      <button v-if="text" @click="downloadPNG">Скачать PNG</button>
    </div>

  </div>
</template>

<script>
import QRCode from 'qrcode-svg';

export default {
  data() {
    return {
      text: '',
    };
  },
  computed: {
    svgQRCode() {
      const qrcode = new QRCode({
        content: this.text,
        padding: 4,
        width: 256,
        height: 256,
        color: '#000000',
        background: '#ffffff',
        ecl: 'H',
      });

      return qrcode.svg();
    },
  },
  methods: {
    async downloadSVG() {
      const a = document.createElement('a');
      a.href = `data:image/svg+xml;charset=utf-8,${encodeURIComponent(this.svgQRCode)}`;
      a.download = 'qr-code.svg';
      a.style.display = 'none';
      document.body.appendChild(a);
      a.click();
      document.body.removeChild(a);
    },
    async downloadPNG() {
      const img = new Image();
      img.src = 'data:image/svg+xml;charset=utf-8,' + encodeURIComponent(this.svgQRCode);

      img.onload = () => {
        const canvas = document.createElement('canvas');
        canvas.width = img.width;
        canvas.height = img.height;
        const ctx = canvas.getContext('2d');
        ctx.drawImage(img, 0, 0);

        const a = document.createElement('a');
        a.href = canvas.toDataURL('image/png');
        a.download = 'qr-code.png';
        a.style.display = 'none';
        document.body.appendChild(a);
        a.click();
        document.body.removeChild(a);
      };
    },
  },
};
</script>

<style>
#app {
  text-align: center;
}
*{
  outline: none;
}
.input{
  display: block;
  margin-inline: auto;
  margin-top: 10px;
  height: 56px;
  border-radius: 12px;
  border: 1px solid #f1f1f1;
  max-width: 516px;
  width: 100%;
  padding: 10px;
  font-size: 18px;
  transition: .2s;
}
.input:hover{
  border-color: #646cff;
}
.input:focus{
  border-color: #646cff;
}
.btn-group{
  display: flex;
  gap: 16px;
  justify-content: center;
}
</style>
