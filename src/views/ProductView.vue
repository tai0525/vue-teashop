<template>
  <VueLoading :active="isLoading" />
  <div class="container px-4 py-5">
    <!-- <h3 class="fs-3">個別產品詳細內容</h3> -->
    <div class="row g-5 mb-5">
      <div class="col-md-6">
        <nav aria-label="breadcrumb">
          <ol class="breadcrumb">
            <li class="breadcrumb-item">
              <RouterLink to="/" class="text-decoration-none">首頁</RouterLink>
            </li>
            <li class="breadcrumb-item">
              <RouterLink to="/user/products" class="text-decoration-none"
                >全部商品</RouterLink
              >
            </li>
            <li class="breadcrumb-item">{{ product.category }}</li>
          </ol>
        </nav>
        <div
          :style="{ backgroundImage: `url(${product.imageUrl})` }"
          style="
            height: 350px;
            background-size: cover;
            background-position: center;
          "
        ></div>
      </div>
      <div class="col-md-6">
        <div class="d-flex">
          <h2 class="fs-2 pt-3 fw-bold">{{ product.title }}</h2>
        </div>

        <span class="badge rounded-pill bg-primary">{{
          product.category
        }}</span>
        <p class="fs-5">
          {{ product.description }}
        </p>
        <p>尺寸 : {{ product.content }}</p>
        <div class="h4" v-if="product.price === product.origin_price">
          {{ product.origin_price }} / {{ product.unit }}
        </div>
        <div v-else>
          <p class="text-decoration-line-through h5">
            原價 NT$ : {{ $filters.numberAddComma(product.origin_price) }} /
            {{ product.unit }}
          </p>
          <p class="h4 text-danger fw-bold">
            特價 NT$ : {{ $filters.numberAddComma(product.price) }} /
            {{ product.unit }}
          </p>
        </div>

        <div class="row g-sm-2">
          <div class="col-md-9">
            <input
              type="number"
              min="1"
              class="form-control"
              v-model.number="quantity"
              @change="confirmNum(quantity)"
            />
          </div>
          <div class="col-md-3">
            <button
              type="button"
              class="btn btn-primary text-nowrap mt-3 mt-sm-0"
              :disabled="loadingState === product.id"
              @click="addToCart(product.id)"
            >
              加入購物車
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>

  <section class="bg-warning">
    <div class="container px-4">
      <div class="row">
        <div class="col-md-6">
          <div class="align-items-center lh-lg py-5">
            <h3 class="fs-3">產品資訊</h3>
            <ul class="list-unstyled">
              <li>
                <i class="bi bi-circle-fill"></i>
                原料：茶葉。
              </li>
              <li>
                <i class="bi bi-circle-fill"></i>
                產地：台灣。
              </li>
              <li>
                <i class="bi bi-circle-fill"></i>
                重量：二兩(75g)。
              </li>
              <li><i class="bi bi-circle-fill"></i> 保存期限：2 年。</li>
              <li>
                <i class="bi bi-circle-fill"></i>
                保存方式：請存放於陰涼乾燥處，避免陽光直射，開封後請儘早飲用。
              </li>
            </ul>
          </div>
        </div>
        <div class="col-md-6">
          <div class="align-items-center lh-lg py-5">
            <h3 class="fs-3">泡茶小撇步</h3>
            <ul class="list-unstyled">
              <li>
                <i class="bi bi-circle-fill"></i>
                沸騰滾水100度。
              </li>
              <li>
                <i class="bi bi-circle-fill"></i>
                約壺身1/2∼2/3的茶量。
              </li>
              <li>
                <i class="bi bi-circle-fill"></i>
                一沖30秒 二沖20秒 三沖40秒。
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </section>

  <section class="py-5">
    <div class="container p-4">
      <h3 class="fw-bold pb-3 ps-4">您可能會喜歡</h3>
      <Swiper
        :slides-per-view="1"
        :space-between="50"
        :modules="modules"
        navigation
        :pagination="{ clickable: true }"
        :breakpoints="{
          '576': {
            slidesPerView: 1,
            spaceBetween: 20,
          },
          '768': {
            slidesPerView: 2,
            spaceBetween: 40,
          },
          '1024': {
            slidesPerView: 3,
            spaceBetween: 50,
          },
        }"
      >
        <SwiperSlide v-for="item in products" :key="item.id">
          <div class="row">
            <div class="col">
              <div class="card h-100">
                <div class="card-body text-center position-relative">
                  <RouterLink :to="`${item.id}`" class="d-block">
                    <img
                      :src="item.imageUrl"
                      :alt="item.category"
                      class="img-fluid"
                      style="height: 280px; object-fit: cover"
                    />
                  </RouterLink>
                  <div class="text-center text-dark mt-2">
                    <RouterLink
                      :to="`${item.id}`"
                      class="text-decoration-none d-block stretched-link"
                      >{{ item.title }}</RouterLink
                    >
                  </div>
                  <div class="d-flex align-items-center justify-content-around">
                    <div
                      class="text-center text-muted text-decoration-line-through mt-2"
                    >
                      原價 NT$ <span class="fs-5">{{ item.origin_price }}</span>
                    </div>
                    <div class="text-center text-danger fw-bold mt-2">
                      特價 NT$ <span class="fs-5">{{ item.price }}</span>
                    </div>
                  </div>
                </div>
                <div class="card-body d-flex justify-content-between">
                  <RouterLink
                    :to="`${item.id}`"
                    class="btn btn-outline-primary d-block"
                  >
                    查看產品
                  </RouterLink>

                  <button
                    type="button"
                    class="btn btn-primary"
                    :disabled="loadingState === item.id"
                    @click="addToCart(item.id)"
                  >
                    加入購物車
                  </button>
                </div>
              </div>
            </div>
          </div>
        </SwiperSlide>
      </Swiper>
    </div>
  </section>
</template>

<script>
// Import Swiper Vue.js components
import { Swiper, SwiperSlide } from "swiper/vue";
import { Navigation, Pagination } from "swiper";
// Import Swiper styles
import "swiper/css";
import "swiper/css/navigation";
import "swiper/css/pagination";
import emitter from "@/libraries/emitter";
export default {
  components: {
    Swiper,
    SwiperSlide,
  },
  data() {
    return {
      product: {
        imageUrl: "",
      },
      products: [],
      quantity: 1,
      loadingState: "",
      isLoading: false,
      modules: [Navigation, Pagination],
      id: "",
    };
  },
  methods: {
    getProductInfo() {
      this.isLoading = true;
      const id = this.$route.params.id;
      const url = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/product/${id}`;
      this.$http
        .get(url)
        .then((res) => {
          this.product = res.data.product;
          this.getProduct(this.product.category);
          emitter.emit("toggle-menu");
          this.isLoading = false;
        })
        .catch((err) => {
          console.log(err.response.data);
        });
    },
    addToCart(id) {
      const data = {
        product_id: id,
        qty: this.quantity,
      };
      this.loadingState = id;
      this.$http
        .post(
          `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart`,
          { data },
        )
        .then(() => {
          this.loadingState = "";
          const Toast = this.$swal.mixin({
            toast: true,
            position: "top-end",
            showConfirmButton: false,
            timer: 1500,
            timerProgressBar: true,
            didOpen: (toast) => {
              toast.addEventListener("mouseenter", this.$swal.stopTimer);
              toast.addEventListener("mouseleave", this.$swal.resumeTimer);
            },
          });
          Toast.fire({
            icon: "success",
            title: "成功加入購物車",
          });
          emitter.emit("get-cart");
          setTimeout(() => {
            this.quantity = 1;
          }, 1000);
        })
        .catch((err) => {
          console.log(err.response.data);
        });
    },
    getProduct(category) {
      const url = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/products?category=${category}`;
      this.$http
        .get(url)
        .then((res) => {
          this.products = res.data.products;
          // 刪除當前頁面的同一個產品，避免重複
          const delIndex = this.products.findIndex((item) => {
            return item.id === this.$route.params.id;
          });
          this.products.splice(delIndex, 1);
        })
        .catch((err) => {
          console.log(err.response.data);
        });
    },

    // 確認數量最少為 1
    confirmNum(num) {
      if (!/(^[1-9]\d*$)/.test(num)) {
        this.quantity = 1;
      }
    },
  },

  watch: {
    $route(to) {
      this.id = to.params.id;
      if (this.$route.params.id !== undefined) {
        this.getProductInfo();
      }
    },
  },
  mounted() {
    this.getProductInfo();
  },
};
</script>
