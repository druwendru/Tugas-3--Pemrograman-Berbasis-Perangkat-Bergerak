<template>
  <ion-app>
    <ion-router-outlet />
  </ion-app>
</template>

<script setup lang="ts">
import { IonApp, IonRouterOutlet } from '@ionic/vue'
</script>

<style>
:root {
  --ion-background-color: #0F0F1A;
  --ion-background-color-rgb: 15, 15, 26;
  --ion-text-color: #ffffff;
  --ion-color-primary: #5C4AFF;
}

ion-app {
  background: #0F0F1A;
}
</style>