<template>
  <Header />
  <RouterView />
  <Footer />
</template>

<script>
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";
import store from "./scripts/store";
import axios from "axios";
import { useRoute } from "vue-router";
import { watch } from "vue";
export default {
  name: "App",
  //react의 state라고 생각.
  data() {
    return {};
  },
  setup() {
    const check = () => {
      axios.get("/api/account/check").then((res) => {
        console.log(res.data);

        store.commit("setAccount", res.data || 0);
      });
    };

    const route = useRoute();

    watch(route, () => {
      check();
    });
  },
  methods: {},
  components: { Header, Footer },
};
</script>

<style></style>
