<template>
  <nav id="nav-top" class="navbar navbar-expand-lg sticky-top bg-body py-4">
    <div class="container px-sm-0">
      <RouterLink
        class="navbar-brand fs-2 me-0 pt-0"
        style="white-space: normal"
        to="/"
      >
        <i class="bi bi-bullseye"></i>
        沁記茶行
      </RouterLink>

      <button
        class="navbar-toggler bg-primary"
        type="button"
        @click="toggleNav"
        aria-controls="navbarNav"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>

      <div class="collapse navbar-collapse" ref="myNavBarRef">
        <ul class="navbar-nav ms-auto fs-5 text-nowrap">
          <li class="nav-item mx-2">
            <RouterLink
              class="nav-link underline-effect"
              aria-current="page"
              to="/user/about"
              >關於我們</RouterLink
            >
          </li>
          <li class="nav-item mx-2">
            <RouterLink
              class="nav-link underline-effect"
              aria-current="page"
              to="/user/knowledge"
              >茶葉知識</RouterLink
            >
          </li>
          <li class="nav-item mx-2">
            <RouterLink class="nav-link underline-effect" to="/user/products"
              >所有商品</RouterLink
            >
          </li>
          <li class="nav-item mx-2">
            <RouterLink
              class="nav-link underline-effect position-relative"
              to="/user/cart"
              ><i class="bi bi-cart2"></i>
              <span
                v-if="cartData.carts.length !== 0"
                class="position-absolute start-lg-100 top-lg-0 translate-middle badge rounded-pill bg-danger"
              >
                {{ cartData.carts.length }}
                <span class="visually-hidden">unread messages</span>
              </span>
            </RouterLink>
          </li>
        </ul>
      </div>
    </div>
  </nav>
</template>

<script>
import emitter from "@/libraries/emitter";
import Collapse from "bootstrap/js/dist/collapse";
export default {
  data() {
    return {
      cartData: {
        carts: [],
      },
      navBarModal: {},
    };
  },
  methods: {
    getCartList() {
      const url = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart`;
      this.$http
        .get(url)
        .then((res) => {
          this.cartData = res.data.data;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    toggleNav() {
      this.navBarModal.toggle();
    },
    closeNav() {
      this.navBarModal.hide();
    },
  },
  mounted() {
    this.navBarModal = new Collapse(this.$refs.myNavBarRef, {
      toggle: false,
    });
    this.getCartList();
    // 監聽點擊加入購物車，更新購物車的數字
    emitter.on("get-cart", () => {
      this.getCartList();
    });
    // 監聽關閉漢堡選單
    emitter.on("toggle-menu", () => {
      this.closeNav();
    });
  },
};
</script>
