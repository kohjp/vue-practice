<template>
  <div class="card shadow-sm">
    <span class="img" :style="{ backgroundImage: `url(${item.imgPath})` }" />
    <div class="card-body">
      <p class="card-text">
        {{ item.name }} &nbsp;
        <small class="discount badge bg-danger">{{ item.discountPer }}%</small>
      </p>
      <div class="d-flex justify-content-between align-items-center">
        <button class="btn btn-primary" @click="addToCart(item.id)">
          <i class="fa fa-shopping-cart" aria-hidden="true"></i>
        </button>
        <small class="price text-body-secondary"
          >{{
            item.price.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")
          }}원</small
        >
        <small class="real text-danger"
          >{{
            (item.price - (item.price * item.discountPer) / 100)
              .toString()
              .replace(/\B(?=(\d{3})+(?!\d))/g, ",")
          }}원</small
        >
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Card",
  props: {
    item: Object,
  },
  setup() {
    const addToCart = (itemId) => {
      axios.post(`/api/cart/items/${itemId}`).then(() => {
        console.log("success");
      });
    };
    return { addToCart };
  },
};
</script>

<style scoped>
.card .img {
  width: 100%;
  display: inline-block;
  height: 250px;
  background-size: contain;
  background-repeat: no-repeat;
  background-position: center;
}

.card .card-body .price {
  text-decoration: line-through;
}
</style>
