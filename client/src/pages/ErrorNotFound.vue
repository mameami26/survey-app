<template>
  <div class="fullscreen bg-blue text-white text-center q-pa-md flex flex-center">
    <div>
      <div class="text-h1" style="font-size: 30vh; font-weight: bold;">
        404
      </div>

      <div class="text-h4" style="opacity: 0.7;">
        Oops. Nothing here...
      </div>

      <q-btn
        class="q-mt-xl"
        color="white"
        text-color="blue"
        unelevated
        to="/"
        label="Go Home"
        no-caps
        style="text-transform: uppercase;"
      />
    </div>
  </div>
</template>

<script setup>
defineOptions({
  name: 'ErrorNotFound'
});
</script>

<style scoped>
.fullscreen {
  height: 100vh;
}

.bg-blue {
  background-color: #007bff; /* Blue background color */
}
</style>
