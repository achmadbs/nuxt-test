<template>
  <div class="container">
    <div class="content__box">
      <div
        :class="{ image__empty: !imageState, border__image: isApplyBorder }"
        class="image__wrapper"
        @click="handleSelectImage"
      >
        <img src="~/assets/images/icon.png" alt="images" v-if="!imageState" />
        <img :src="imageState" v-else alt="images" id="images" ref="img" />
      </div>
      <input
        type="file"
        ref="inputFile"
        accept="image/*"
        hidden
        crossorigin="anonymous"
        @change="handlePreviewFile"
      />
      <button v-if="imageState && !isApplyBorder" @click="isApplyBorder = true">
        Add Frame on Image
      </button>
      <button v-if="imageState && isApplyBorder" @click="handleSaveImage">
        Download Image
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      imageState: false,
      isApplyBorder: false,
    };
  },
  methods: {
    handleSelectImage() {
      this.$refs.inputFile.click();
    },
    handlePreviewFile(event) {
      let reader = new FileReader();
      reader.onload = (event) => {
        this.imageState = event.target.result;
      };
      reader.readAsDataURL(event.target.files[0]);
    },
    handleSaveImage() {
      const canvas = document.createElement("canvas");
      canvas.width = document.getElementById("images").clientWidth;
      canvas.height = document.getElementById("images").clientHeight;
      const ctx = canvas.getContext("2d");
      ctx.drawImage(this.$refs.img, 0, 0, canvas.width, canvas.height);
      ctx.strokeStyle = "#1BAAA0";
      ctx.lineWidth = 10;
      ctx.strokeRect(0, 0, canvas.width, canvas.height);
      const link = document.createElement("a");
      link.href = canvas.toDataURL();
      link.download = "images.png";
      link.click();
    },
  },
};
</script>

<style scoped>
.container {
  display: grid;
  place-items: center;
  height: 100vh;
}
.content__box {
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 1.5rem;
}
.content__box button {
  background-color: var(--main-color);
  border-radius: 10px;
  padding: 15px;
  border: none;
  color: #ffffff;
  font-weight: 700;
}
.image__wrapper {
  border-radius: 10px;
  height: 350px;
  width: 350px;
  cursor: pointer;
}
.image__wrapper > * {
  width: 100%;
  height: 100%;
  object-fit: scale-down;
  box-sizing: border-box;
}
.border__image > * {
  border: 10px solid var(--main-color);
}
.image__empty {
  background-color: #d9d9d9;
}
</style>
