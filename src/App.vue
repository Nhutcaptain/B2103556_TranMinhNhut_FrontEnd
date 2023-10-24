<script>
import AppHeader from "@/components/AppHeader.vue";
export default {
  components: {
    AppHeader,
  },
};
</script>
<template>
  <div id="app">
    <AppHeader />
    <div class="view">
      <router-view/>
    </div>
  </div>
</template>
<style>
.page {
  max-width: 750px;
  margin: auto;
}

</style>