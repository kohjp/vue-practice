<template>
  <div class="cart">
    <div class="container">
      <h2>장바구니</h2>
      <ul>
        <li v-for="(i, idx) in state.items" :key="idx">
          <img :src="i.imgPath" />
          <div class="info">
            <span class="name">
              {{ i.name }}
            </span>
            <span class="price">
              {{
                (i.price - (i.price * i.discountPer) / 100)
                  .toString()
                  .replace(/\B(?=(\d{3})+(?!\d))/g, ",")
              }}원
            </span>
          </div>
          <i class="fa fa-trash" @click="remove(i.id)"></i>
        </li>
      </ul>
      <router-link to="/order" class="btn btn-primary">구입하기</router-link>
    </div>
  </div>
</template>

<script>
import { reactive } from "vue";
import axios from "axios";

export default {
  name: "Cart",
  setup() {
    const state = reactive({
      items: [],
    });

    const load = () => {
      axios.get("/api/cart/items").then((res) => {
        state.items = res.data;
      });
    };

    const remove = (itemId) => {
      axios.delete(`/api/cart/items/${itemId}`).then(() => {
        load();
      });
    };

    load();

    return { state, remove };
  },
};
</script>

<style scoped>
.cart ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

.cart ul li {
  border: 1px solid #eee;
  margin-top: 25px;
  margin-bottom: 25px;
  display: flex;
}

.cart ul li img {
  width: 150px;
  height: 150px;
}

.cart ul li .name {
  margin-left: 25px;
}

.cart ul li .price {
  margin-left: 25px;
}

.cart ul li .info {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex: 1;
  margin-right: 15px;
}

.cart ul li i {
  float: right;
  font-size: 20px;
  margin-top: 65px;
  margin-right: 50px;
  cursor: pointer;
}

.cart .btn {
  width: 300px;
  display: block;
  margin: 0 auto;
  padding: 30px 50px;
  font-size: 20px;
}

.cart h2 {
  margin-top: 30px;
  padding-bottom: 30px;
  border-bottom: solid 1px rgb(101, 101, 101);
}
</style>
