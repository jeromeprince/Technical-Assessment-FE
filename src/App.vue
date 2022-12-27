<script setup lang="ts">
import { RouterLink, RouterView } from "vue-router";
import Menu from "@/components/Menu.vue"
</script>

<template>
  <header>
    <div class="wrapper">
      <Menu />
      <RouterView />
    </div>
  </header>
</template>
