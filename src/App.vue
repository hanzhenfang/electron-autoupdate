<script setup lang="ts">
import { ref, onMounted } from "vue";

import HelloWorld from "./components/HelloWorld.vue";

const version = ref("");
const message = ref("没有检测到更新");

onMounted(async () => {
  version.value = await window.ipcRenderer.invoke("app-version");
});

window.ipcRenderer.on("update-available", () => {
  message.value = "更新！！";
});

function restart() {
  if (message.value !== "没有检测到更新") return;
  window.ipcRenderer.send("restart");
}
</script>

<template>
  <div class="flex-center">
    {{ version }}
  </div>
  <p>{{ message }}</p>
  <button @click="restart">重启</button>
</template>

<style>
.flex-center {
  display: flex;
  align-items: center;
  justify-content: center;
}

.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}

.logo.electron:hover {
  filter: drop-shadow(0 0 2em #9feaf9);
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
