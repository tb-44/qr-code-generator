<template>
    <section id="qr-code-settings" class="container container-front">
      <header>
        <h3>Generate your QR code</h3>
        <h4>Adjust the settings below to make your QR code look as you want</h4>
      </header>
  
      <div>
        <div class="setting">
          <label for="data">Text / URL</label>
          <span class="sub-label"
            >Include text or a URL you want the QR code to lead to</span
          >
          <input
            type="text"
            name="data"
            id="data"
            min="0"
            max="900"
            v-model="textURLData"
            placeholder="Add text or URL here"
            @keyup.enter="updateTextURLData($event)"
            required
          />
        </div>
  
        <div class="colors">
          <div class="setting">
            <label for="color">Main Color</label>
            <input
              id="color"
              type="color"
              v-model="mainColor"
              @change="updateMainColor($event)"
            />
            <span id="color-value">{{ mainColor }}</span>
          </div>
  
          <div class="setting">
            <label for="bg-color">Background Color</label>
            <input
              id="bg-color"
              type="color"
              v-model="bgColor"
              @change="updateBgColor($event)"
            />
            <span id="bg-color-value">{{ bgColor }}</span>
          </div>
        </div>
  
        <div class="setting">
          <label for="size">Size</label>
          <span id="size-value" class="sub-label">{{ size }} x {{ size }}</span>
          <div class="slider">
            <input
              name="size"
              id="size"
              type="range"
              min="100"
              max="1000"
              step="100"
              v-model="size"
              @change="updateSize($event)"
            />
          </div>
        </div>
  
        <div class="setting">
          <label for="margin">Margin</label>
          <span id="margin-value" class="sub-label">{{ margin }} px</span>
          <div class="slider">
            <input
              type="range"
              name="margin"
              id="margin"
              min="0"
              max="50"
              v-model="margin"
              @change="updateMargin($event)"
            />
          </div>
        </div>
  
        <div class="setting">
          <fieldset>
            <legend>Image Format</legend>
            <div>
              <input
                type="radio"
                id="png"
                name="format"
                v-model="imgFormat"
                checked
              />
              <label for="png">PNG</label>
            </div>
            <div>
              <input
                type="radio"
                id="gif"
                name="format"
                v-model="imgFormat"
              />
              <label for="gif">GIF</label>
            </div>
            <div>
              <input
                type="radio"
                id="jpeg"
                name="format"
                v-model="imgFormat"
              />
              <label for="jpeg">JPEG</label>
            </div>
            <div>
              <input
                type="radio"
                id="jpg"
                name="format"
                v-model="imgFormat"
              />
              <label for="jpg">JPG</label>
            </div>
            <div>
              <input
                type="radio"
                id="svg"
                name="format"
                v-model="imgFormat"
              />
              <label for="svg">SVG</label>
            </div>
          </fieldset>
        </div>
      </div>
      <div class="cta">
        <button id="cta" type="submit" @click="getQRCode">
          Generate QR code
        </button>
      </div>
    </section>
    <div>
      <section id="qr-code-result" class="container container-back">
        <header>
          <h2>Your QR code is ready!</h2>
          <h3>Scan this image to see it in action</h3>
        </header>
  
        <div class="setting">
          <img :src="url" alt="qr-code" id="qr-code-image" name="qr-code" />
        </div>
  
        <div class="info">
          <p>
            Right click on the image and select <b>"Save Image As"</b> to download
            the file
          </p>
        </div>
      </section>
    </div>
  </template>
  
<script setup lang="ts">
import { ref } from 'vue';
import axios from 'axios';

const props = defineProps({
  msg: String,
});

const textURLData = ref('');
const size = ref(100); 
const margin = ref(1);
const imgFormat = ref('png');
const bgColor = ref('#c7c7c7');
const mainColor = ref('#000000');
const url = ref('');

function getQRCode() {
  const parameters = prepareParameters({
    data: textURLData.value,
    size: size.value,
    color: mainColor.value,
    bgcolor: bgColor.value,
    qzone: margin.value,
    format: imgFormat.value,
  });

  const baseUrl = 'https://api.qrserver.com/v1/create-qr-code/';
  const urlParams = new URLSearchParams(parameters).toString();

  url.value = `${baseUrl}?${urlParams}`;

  axios.get(url.value).then((response: { status: number; }) => {
    if (response.status === 200) {
      console.log(response);
    }
  });
}

function updateSize(event: Event) {
  const target = event.target as HTMLInputElement;
  size.value = parseInt(target.value, 10);
}

function updateMargin(event: Event) {
  const target = event.target as HTMLInputElement;
  margin.value = parseInt(target.value, 10);
}

function updateTextURLData(event: Event) {
  const target = event.target as HTMLInputElement;
  textURLData.value = target.value;
}

function updateMainColor(event: Event) {
  const target = event.target as HTMLInputElement;
  mainColor.value = target.value;
}

function updateBgColor(event: Event) {
  const target = event.target as HTMLInputElement;
  bgColor.value = target.value;
}

function prepareParameters(params: {
  data: string;
  size: number;
  color: string;
  bgcolor: string;
  qzone: number;
  format: string;
}) {
  return {
    data: params.data,
    size: `${params.size}x${params.size}`,
    color: params.color.replace('#', ''),
    bgcolor: params.bgcolor.replace('#', ''),
    qzone: params.qzone.toString(),
    format: params.format,
  };
}
</script>
  
  <style>
  h3 {
    margin: 0;
    padding: 10px;
  }
  
  h4 {
    margin: 0;
    padding: 5px;
  }
  
  button {
    background-color: #fdc886;
    padding: 0.5rem 1.2rem;
    font-size: 1.1rem;
    font-family: 'Work Sans', sans-serif;
    font-weight: 500;
    border-radius: 0.3rem;
    margin: 0.5rem auto 1.5rem;
    border: 2px solid #fdc886;
    border-radius: 0.5rem;
    line-height: 1.1rem;
    cursor: pointer;
    transition: all 0.15s ease-in-out;
  }
  
  button:hover {
    transition: all 0.15s ease-in-out;
    transform: translateY(-0.2rem);
  }
  
  button:active {
    background-color: #dea764;
    transition: all 0.15s ease-in-out;
    border: 2px solid #dea764;
  }
  
  button:focus {
    outline: none;
    border: 2px solid #dea764;
  }
  
  button:disabled {
    outline: none;
    transition: none;
    transform: none;
    cursor: not-allowed;
  }
  
  .setting {
    margin: 1rem;
    display: flex;
    flex-direction: column;
  }
  
  input[type='color'] {
    appearance: none;
    background: none;
    border: 1px solid #000000;
    cursor: pointer;
    height: 50px;
    padding: 0.2rem;
    width: 50px;
  }
  
  input[type='color']:focus {
    outline: none;
    border: 2px solid #dea764;
    border-radius: 2px;
  }
  
  .colors {
    display: flex;
    justify-content: space-around;
  }
  
  .colors .setting {
    align-items: center;
    text-align: center;
  }
  
  .colors .setting span {
    font-weight: 200;
    font-size: 0.9rem;
  }
  
  .sub-label {
    font-weight: 200;
    font-size: 0.9rem;
  }
  
  input[type='text'] {
    border: 1px solid #000000;
    padding: 0.35rem 0.85rem 0.35rem 0.4rem;
    line-height: 1.2rem;
    font-size: 1rem;
    font-weight: 200;
    border-radius: 0.3rem;
    font-family: 'Work Sans', sans-serif;
  }
  
  input[type='text']:focus {
    outline: none;
    border: 1px solid #dea764;
  }
  
  input[type='range'] {
    -webkit-appearance: none;
    width: 100%;
    height: 5px;
    border-radius: 3px;
    background: #d9d9d9;
    outline: none;
    padding: 0;
    margin: 0;
  }
  input[type='range']::-webkit-slider-thumb {
    -webkit-appearance: none;
    appearance: none;
    width: 20px;
    height: 20px;
    border-radius: 50%;
    background: #fdc886;
    cursor: pointer;
    -webkit-transition: background 0.15s ease-in-out;
    transition: background 0.15s ease-in-out;
  }
  input[type='range']::-webkit-slider-thumb:hover {
    background: #dea764;
  }
  input[type='range']:active::-webkit-slider-thumb {
    background: #dea764;
  }
  input[type='range']::-moz-range-thumb {
    width: 20px;
    height: 20px;
    border: 0;
    border-radius: 50%;
    background: #fdc886;
    cursor: pointer;
    -moz-transition: background 0.15s ease-in-out;
    transition: background 0.15s ease-in-out;
  }
  input[type='range']::-moz-range-thumb:hover {
    background: #dea764;
  }
  input[type='range']:active::-moz-range-thumb {
    background: #dea764;
  }
  input[type='range']:focus::-webkit-slider-thumb {
    box-shadow: 0 0 0 3px #fff, 0 0 0 5px #dea764;
  }
  
  input[type='radio'] {
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    display: inline-block;
    width: 15px;
    height: 15px;
    padding: 2px;
    background-clip: content-box;
    border: 1px solid #fdc886;
    background-color: #ffffff;
    border-radius: 50%;
  }
  
  input[type='radio']:checked {
    background-color: #fdc886;
  }
  
  input[type='radio']:focus {
    outline: none;
    border: 2px solid #dea764;
    background-color: #dea764;
  }
  fieldset {
    display: flex;
    flex-wrap: wrap;
    border: none;
    padding: 0;
    margin: 0;
    justify-content: space-between;
  }
  
  fieldset legend {
    margin-bottom: 0.5rem;
  }
  
  fieldset div {
    border: 1px solid #000000;
    border-radius: 0.3rem;
    display: flex;
    align-items: center;
    padding: 0.25rem 0.5rem;
    margin-right: 0.25rem;
    margin-bottom: 0.5rem;
  }
  
  fieldset div:last-child {
    margin-right: 0;
  }
  
  fieldset div input {
    margin-right: 0.5rem;
    cursor: pointer;
  }
  
  fieldset div label {
    cursor: pointer;
  }
  
  .cta {
    display: flex;
  }
  
  .container-back.flipped {
    transform: rotateY(0);
  }
  
  .setting img {
    width: 90%;
    margin: 0 auto;
  }
  
  .info {
    text-align: center;
    margin-top: 0.2rem;
    font-weight: 200;
    font-size: 1rem;
    width: 80%;
    margin: 0 auto;
  }
  </style>