<template>
  <Header />
  <Home />
  <Footer />
</template>

<script>
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";
import Home from "./pages/Home.vue";
export default {
  name: "App",
  //react의 state라고 생각.
  data() {
    return {};
  },
  methods: {},
  components: { Header, Footer, Home },
};
</script>

<style></style>
