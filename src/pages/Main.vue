<script setup>
import { ref, provide } from "vue";
import Dropfield from "../components/Dropfield.vue";
import IconList from "../components/IconList.vue";
import useFileList from "../composables/file-list";
import { toPng } from "html-to-image";
import download from "downloadjs";
import * as uuid from "uuid";

const { files, addFiles } = useFileList();

const imageSize = ref(150),
  textSize = ref(30),
  borderWidth = ref(4);

provide("sizeProps", {
  imageSize,
  textSize,
  borderWidth
});

const onDrop = (newFiles) => {
  addFiles(newFiles);
}

const saveImages = () => {
  const icons = document.querySelectorAll(".icon");
  const iconImages = document.querySelectorAll(".icon__image");
  processItems(icons);
  processItems(iconImages);
};

const processItems = (items) => {
  [...items].map((item) => {
    toPng(item)
    .then(function (dataUrl) {
      download(dataUrl, `${uuid.v4()}.png`);
    })
    .catch(function (error) {
      console.error('oops, something went wrong!', error);
    });
  });
};
</script>

<template>
  <div class="flex flex-col gap-8 w-full max-w-[900px] px-6 mx-auto mt-28">
    <div class="w-full h-32">
      <Dropfield @files-dropped="onDrop" :files="files"/>
    </div>

    <div class="flex flex-col gap-8 mt-8">
      <div class="flex gap-6">
        <input
          class="flex-1 px-6 py-4 text-xl border border-gray-300 outline-none rounded-lg"
          type="number"
          :value="textSize"
          @input="(e) => textSize = e.target.value"
          placeholder="Размер текста"
        >
        <input
          class="flex-1 px-6 py-4 text-xl border border-gray-300 outline-none rounded-lg"
          type="number"
          :value="imageSize"
          @input="(e) => imageSize = e.target.value"
          placeholder="Размер картинки"
        >
        <input
          class="flex-1 px-6 py-4 text-xl border border-gray-300 outline-none rounded-lg"
          type="number"
          :value="borderWidth"
          @input="(e) => borderWidth = e.target.value"
          placeholder="Размер бордера"
        >
      </div>
      <button
        @click="saveImages()"
        class="w-full p-8 rounded-lg bg-[#933fb5] hover:bg-[#a452c5] text-2xl text-white"
      >Сохранить</button>
    </div>

    <div class="mt-32">
      <IconList
        :files="files"
      ></IconList>
    </div>
  </div>
</template>