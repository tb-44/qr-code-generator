<template>
  <div>
    <section id="qr-code-result" class="container container-back">
      <header>
        <h2>Your QR code is ready!</h2>
        <h3>Scan this image to see it in action</h3>
      </header>

      <div class="setting">
        <img :src="qrImageSrc" alt="QR code" id="qr-code-image" name="qr-code" />
      </div>

      <div class="info">
        <p>
          Right click on the image and select <b>"Save Image As"</b> to download the file
        </p>

        <p>Or need to make some changes?</p>

        <div class="cta">
          <button id="edit" type="submit" @click="goBack">Go back</button>
        </div>
      </div>
    </section>
  </div>
</template>

<script setup lang="ts">

// eslint-disable-next-line @typescript-eslint/no-unused-vars
const props = defineProps({
  qrImageSrc: String
});

const emit = defineEmits(['edit']);

function goBack() {
  emit('edit');
}
</script>
  
  <style scoped>
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