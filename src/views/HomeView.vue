<template>
  <div class="home">
    <form action="" id="myForm">
      <div class="BackGround"></div>
      <div class="container">
        <div class="grid-row">
          <div class="grid-item">
            <div class="d-block">
              <div class="title">Name</div>
              <input type="text" class="my_inp" name="" id="" />
            </div>
          </div>
          <div class="grid-item">
            <div class="d-block">
              <div class="title">Price</div>
              <input type="text" class="my_inp" name="" id="" />
            </div>
          </div>
          <div class="grid-item">
            <div class="d-block">
              <div class="title">Location</div>
              <input type="text" class="my_inp" name="" id="" />
            </div>
          </div>
          <div class="grid-item">
            <div class="d-block">
              <div class="title">Space</div>
              <input type="text" class="my_inp" name="" id="" />
            </div>
          </div>
          <div class="grid-item">
            <div class="d-block">
              <div class="title">Rooms</div>
              <input type="text" class="my_inp" name="" id="" />
            </div>
          </div>
        </div>
        <div class="inp_container">
          <h1>2D Map</h1>
          <input
            type="file"
            class="my_inp"
            id="Image2D"
            accept="image/*"
            @change="FileChange()"
            style="width: 70%"
            v-show="generated"
          />
          <div class="image-container" @click="addInput($event)">
            <img
              src=""
              alt="Image"
              v-show="file != null"
              id="file-preview"
              class="styled-image"
            />
            <div
              v-for="(input, index) in inputs"
              :key="index"
              :style="{ top: input.y + 'px', left: input.x + 'px' }"
              class="input-wrapper"
            >
              <input type="file" name="Files360" v-show="generated" />
            </div>
          </div>
        </div>
      </div>
    </form>
    <button type="button" @click="generatePDF()">Generate pdf</button>
  </div>
</template>

<script>
// @ is an alias to /src
import jsPDF from "jspdf";
import html2canvas from "html2canvas";

export default {
  name: "HomeView",
  data() {
    return {
      file: null,
      generated: true,
      inputs: [],
    };
  },
  methods: {
    FileChange() {
      const input = document.getElementById("Image2D");
      const file = input.files;
      if (file) {
        const fileReader = new FileReader();
        const preview = document.getElementById("file-preview");
        fileReader.onload = (event) => {
          preview.setAttribute("src", event.target.result);
        };
        fileReader.readAsDataURL(file[0]);
      }
      this.file = file[0];
    },
    async generatePDF() {
      this.generated = false;
      setTimeout(() => {
        this.generateHelper();
      }, 100);
    },
    async generateHelper() {
      const formContainer = document.querySelector("#myForm");
      const canvas = await html2canvas(formContainer);

      const imgData = canvas.toDataURL("image/png");
      const pdf = new jsPDF();

      const pdfWidth = pdf.internal.pageSize.getWidth();
      // const pdfHeight = pdf.internal.pageSize.getHeight();

      const imgWidth = pdfWidth * 0.75; // Adjust as needed
      const imgHeight = (canvas.height * imgWidth) / canvas.width;

      pdf.addImage(
        imgData,
        "PNG",
        (pdfWidth - imgWidth) / 2,
        10,
        imgWidth,
        imgHeight
      );
      pdf.save("custom_form_data.pdf");
    },
    addInput(event) {
      const rect = event.target.getBoundingClientRect();
      const x = event.clientX - rect.left;
      const y = event.clientY - rect.top;

      this.inputs.push({ x: x, y: y, file: null });
    },
    removeInput(index) {
      this.inputs.splice(index, 1);
    },
  },
};
</script>

<style>
.BackGround {
  position: fixed;
  top: 0;
  left: 0;
  opacity: 0.7;
  z-index: -1;
  background-color: black;
  height: 100dvh;
  width: 100dvw;
  background-image: url("../assets/Images/PdfImage.jpg");
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}

.container {
  position: relative;
  width: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  color: white;
}

.grid-row {
  display: grid;
  grid-template-columns: 400px 400px;
  justify-content: center;
  padding: 50px;
  gap: 20px 100px;
}

.grid-item {
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.d-block {
  display: block;
  width: 100%;
}

.title {
  font-size: 20px;
  text-align: start;
}

.my_inp {
  height: 50px;
  width: 100%;
  margin: 20px 0;
  border-radius: 10px;
  border: none;
  font-size: 24px;
  padding: 0 20px;
}

.my_inp:focus {
  outline: none !important;
}

.inp_container {
  width: 100%;
}

/*  */
.image-container {
  position: relative;
  display: inline-block;
}

.image-container img {
  width: 100%;
  height: auto;
}

.input-wrapper {
  position: absolute;
}

.styled-image {
  height: 500px;
  width: 300px;
}
</style>
