<script setup>
import { ref, onMounted, onUnmounted, computed } from "vue";

const isActive = ref(false);
let inActiveTimeout = null;

const emit = defineEmits();

const props = defineProps(["files"]);

const labelText = computed(() => {
  const filesCount = props?.files.length;
  return filesCount
    ? `Всего файлов: ${filesCount}`
    : "Перенеси файлы сюда";
});

const setActive = () => {
  isActive.value = true;
  clearTimeout(inActiveTimeout);
}

const setInactive = () => {
  inActiveTimeout = setTimeout(() => {
    isActive.value = false;
  }, 50);
}

const onDrop = (e) => {
  setInactive();
  emit("files-dropped", [...e.dataTransfer.files]);
}

const preventDefaults = (e) => {
  e.preventDefault()
}

const events = ["dragenter", "dragover", "dragleave", "drop"];

onMounted(() => {
  events.forEach((eventName) => {
    document.body.addEventListener(eventName, preventDefaults);
  });
});

onUnmounted(() => {
  events.forEach((eventName) => {
    document.body.removeEventListener(eventName, preventDefaults);
  });
});
</script>

<template>
  <label
    class="flex items-center justify-center w-full h-full rounded-xl text-2xl cursor-pointer"
    :class="{
      'bg-[#f5f5f5]': isActive,
      'bg-[#f9f9f9]': !isActive
    }"
    @drop.prevent="onDrop"
    @dragenter.prevent="setActive"
    @dragover.prevent="setActive"
    @dragleave.prevent="setInactive"
    for="file"
  >
    <span>{{ labelText }}</span>
    <input class="hidden" id="file" type="file" multiple>
   </label>
</template>