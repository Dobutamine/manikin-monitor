<template>
  <q-layout view="hHh lpR fFf">
    <q-header bordered class="bg-primary text-white">
      <div class="q-ma-sm">Radboudumc NeoSIM patientmonitor emulator</div>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>

    <q-footer bordered class="bg-grey-8 text-white">
      <div class="q-ma-es">Monitor connected to manikin on ip</div>
    </q-footer>
  </q-layout>
</template>

<script></script>
