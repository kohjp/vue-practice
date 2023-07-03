<template>
  <Header />
  <RouterView />
  <Footer />
</template>

<script>
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";
import store from "./scripts/store";
export default {
  name: "App",
  //react의 state라고 생각.
  data() {
    return {};
  },
  setup() {
    const id = sessionStorage.getItem("id");

    if (id) {
      store.commit("setAccount", id);
    }
  },
  methods: {},
  components: { Header, Footer },
};
</script>

<style></style>
