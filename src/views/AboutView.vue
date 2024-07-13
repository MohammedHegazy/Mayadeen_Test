<template>
  <div class="about-page">
    <div class="container">
      <div class="image-container">
        <img :src="img" alt="Map" />
      </div>
      <div
        v-for="(input, index) in inputs"
        :key="index"
        :style="{ top: input.y + 'px', left: input.x + 'px' }"
        class="input-wrapper"
      >
        <button type="button" class="mybtn" @click="showImage(input.file)">
          {{ index }}
        </button>
      </div>
    </div>
  </div>
  <div class="popup" id="popup" style="opacity: 0; visibility: hidden">
    <a-scene v-if="clicked">
      <a-sky :src="imageSrc"></a-sky>
    </a-scene>
  </div>
</template>

<script>
import "aframe";

export default {
  name: "about-page",
  data() {
    return {
      img: require("@/assets/Images/File2D.jpg"),
      inputs: [
        {
          x: 1148,
          y: 428.7249984741211,
          file: require("@/assets/Images/360photos/p1.jpg"),
        },
        {
          x: 656,
          y: 426.7249984741211,
          file: require("@/assets/Images/360photos/p2.jpg"),
        },
      ],
      clicked: false,
      imageSrc: null,
    };
  },
  methods: {
    showImage(file) {
      const element = document.getElementById("popup");
      element.style.display = "block";
      element.style.opacity = 1;
      element.style.visibility = "visible";
      this.imageSrc = file;
      this.clicked = true;
    },
  },
};
</script>

<style>
a-scene {
  width: 100%;
  height: 100vh;
}

.popup {
  position: absolute;
  top: 0;
  left: 0;
}

.mybtn {
  color: white;
  background-color: royalblue;
  font-size: 24px;
  width: 50px;
  height: 50px;
  padding: 10px;
  border: none;
  border-radius: 10px;
  transition: 0.5s ease-in-out;
  cursor: pointer;
}

.mybtn:hover {
  background-color: rgb(49, 79, 167);
}
</style>
