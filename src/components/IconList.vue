<script setup>
import Icon from "./Icon.vue";

const props = defineProps(["files"]);

</script>

<template>
  <div class="flex gap-20">
    <Icon
      v-for="file in props.files"
      :key="file?.id"
      :file="file"
    ></Icon>
  </div>  
</template>/