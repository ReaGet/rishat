<script setup>
import { inject, ref } from "vue";

const emit = defineEmits(["change"]);

const { textSize } = inject("sizeProps");

const isEditing = ref(false),
  titleValue = ref("eye contact"),
  input = ref(null);

const handleClickOnText = () => {
  isEditing.value = true;
  setTimeout(() => {
    input.value?.focus();
  });
};

const onValueChange = () => {
  emit("change", titleValue.value);
  isEditing.value = false;
};

</script>

<template>
  <div class="flex justify-center w-full text-center">

    <span
      v-if="!isEditing"
      class="editable__text flex items-center justify-center w-[185px] min-h-[40px]"
      :style="{
          fontSize: `${textSize}px`,
          lineHeight: `${textSize}px`,
       }"
      @click.stopPropagation="handleClickOnText"
    >{{ titleValue }}</span>
    <input
      v-else
      class="w-[185px] h-16 bg-red-50 text-center border-none outline-none text-5xl"
      @keypress.enter="onValueChange"
      ref="input"
      :value="titleValue"
      @input="(e) => titleValue = e.target.value"
    >
  </div>
</template>

<style scoped>
.editable__text {
  font-family: 'Comic Neue';
  font-weight: 700;
}
</style>