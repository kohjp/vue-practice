<template>
  <Header />
  <RouterView />
  <Footer />
</template>

<script>
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";
export default {
  name: "App",
  //react의 state라고 생각.
  data() {
    return {};
  },
  methods: {},
  components: { Header, Footer },
};
</script>

<style></style>
