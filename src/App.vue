<template>
  <Tinymce v-model="ctx" />

  <Button style="margin-top: 50px" @click="visible = true"
    >查看编辑器内容：</Button
  >

  <Dialog v-model:visible="visible" header="编辑器内容" >{{
    ctx
  }}</Dialog>
</template>
<script setup lang="ts">
import { ref } from "vue";
import Tinymce from "./components/Tinymce.vue";
import { Button, Dialog } from "tdesign-vue-next";

const ctx = ref("<p>tdesign</p>");
const visible = ref(false);
</script>
