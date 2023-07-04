<template>
  <div class="order">
    <div class="container">
      <main>
        <div class="py-5 text-center">
          <h2>주문하기</h2>
          <p class="lead">
            Below is an example form built entirely with Bootstrap’s form
            controls. Each required form group has a validation state that can
            be triggered by attempting to submit the form without completing it.
          </p>
        </div>
        <div class="row g-5">
          <div class="col-md-5 col-lg-4 order-md-last">
            <h4 class="d-flex justify-content-between align-items-center mb-3">
              <span class="text-primary">구입 목록</span
              ><span class="badge bg-primary rounded-pill">{{
                state.items.length
              }}</span>
            </h4>
            <ul class="list-group mb-3">
              <li
                class="list-group-item d-flex justify-content-between lh-sm"
                v-for="(i, idx) in state.items"
                :key="idx"
              >
                <div>
                  <h6 class="my-0">{{ i.name }}</h6>
                  <small class="text-body-secondary">Brief description</small>
                </div>
                <span class="text-body-secondary">
                  {{
                    (i.price - (i.price * i.discountPer) / 100)
                      .toString()
                      .replace(/\B(?=(\d{3})+(?!\d))/g, ",")
                  }}원</span
                >
              </li>
            </ul>
            <h3 class="text-center total-price">{{ computedPrice }}원</h3>
          </div>
          <div class="col-md-7 col-lg-8">
            <h4 class="mb-3">주문자 정보</h4>
            <div class="needs-validation" novalidate="">
              <div class="row g-3">
                <div class="col-12">
                  <label for="username" class="form-label">이름</label>
                  <div class="input-group has-validation">
                    <input
                      type="text"
                      class="form-control"
                      id="username"
                      placeholder="Username"
                      v-model="state.form.name"
                    />
                    <div class="invalid-feedback">
                      Your username is required.
                    </div>
                  </div>
                </div>
                <div class="col-12">
                  <label for="address" class="form-label">주소</label
                  ><input
                    type="text"
                    class="form-control"
                    id="address"
                    v-model="state.form.address"
                  />
                  <div class="invalid-feedback">
                    Please enter your shipping address.
                  </div>
                </div>
              </div>
              <hr class="my-4" />
              <h4 class="mb-3">결제수단</h4>
              <div class="my-3">
                <div class="form-check">
                  <input
                    id="card"
                    name="paymentMethod"
                    type="radio"
                    class="form-check-input"
                    checked=""
                    v-model="state.form.payment"
                    value="card"
                  /><label class="form-check-label" for="card">신용카드</label>
                </div>
                <div class="form-check">
                  <input
                    id="npay"
                    name="paymentMethod"
                    type="radio"
                    class="form-check-input"
                    v-model="state.form.payment"
                    value="npay"
                  /><label class="form-check-label" for="npay"
                    >네이버페이</label
                  >
                </div>
                <div class="form-check">
                  <input
                    id="bank"
                    name="paymentMethod"
                    type="radio"
                    class="form-check-input"
                    v-model="state.form.payment"
                    value="bank"
                  /><label class="form-check-label" for="bank">계좌이체</label>
                </div>
              </div>
              <div class="row gy-3">
                <div class="col-md-6">
                  <label for="cc-name" class="form-label">카드번호</label
                  ><input
                    type="text"
                    class="form-control"
                    id="cc-name"
                    placeholder=""
                    v-model="state.form.cardNumber"
                  />
                  <div class="invalid-feedback">Name on card is required</div>
                </div>
              </div>
              <hr class="my-4" />
              <button
                class="w-100 btn btn-primary btn-lg"
                type="click"
                @click="submit"
              >
                결제하기
              </button>
            </div>
          </div>
        </div>
      </main>
    </div>
  </div>
</template>

<script>
import { computed, reactive } from "vue";
import axios from "axios";

export default {
  name: "Order",
  setup() {
    const state = reactive({
      items: [],
      form: {
        name: "",
        address: "",
        payment: "",
        cardNumber: "",
        items: "",
      },
    });

    const load = () => {
      axios.get("/api/cart/items").then((res) => {
        state.items = res.data;
      });
    };

    const submit = () => {
      const args = JSON.parse(JSON.stringify(state.form));
      args.items = JSON.stringify(state.items);
      axios.post("/api/orders", args).then(() => {
        console.log("success");
      });
    };

    load();

    const computedPrice = computed(() => {
      let result = 0;
      for (let i of state.items) {
        result += i.price - (i.price * i.discountPer) / 100;
      }
      return result.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    });

    return { state, computedPrice, submit };
  },
};
</script>

<style scoped></style>
