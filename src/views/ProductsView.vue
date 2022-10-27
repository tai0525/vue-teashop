<template>
  <VueLoading :active="isLoading" />
  <div class="container">
    <div class="row">
      <div class="col-md-3 pt-3 text-center">
        <h4 class="fs-4 bg-warning text-dark py-3 mb-0">產品分類</h4>
        <div class="list-group">
          <button
            type="button"
            class="list-group-item btn-hover-primary"
            @click="getProduct(1)"
          >
            全部
          </button>
          <button
            type="button"
            class="list-group-item btn-hover-primary"
            @click="getProduct(1, '阿里山茶系列')"
          >
            阿里山茶系列
          </button>
          <button
            type="button"
            class="list-group-item btn-hover-primary"
            @click="getProduct(1, '凍頂烏龍茶系列')"
          >
            凍頂烏龍茶系列
          </button>
          <button
            type="button"
            class="list-group-item btn-hover-primary"
            @click="getProduct(1, '台灣紅茶系列')"
          >
            台灣紅茶系列
          </button>
          <button
            type="button"
            class="list-group-item btn-hover-primary"
            @click="getProduct(1, '茶具全系列')"
          >
            茶具全系列
          </button>
        </div>
      </div>
      <div class="col-md-9 pb-4">
        <div class="row row-cols-1 row-cols-md-2 row-cols-lg-3 g-4">
          <div class="col" v-for="item in products" :key="item.id">
            <div class="card border-0 h-100">
              <div class="card-body text-center position-relative">
                <RouterLink :to="`product/${item.id}`">
                  <img
                    :src="item.imageUrl"
                    :alt="item.category"
                    class="img-fluid"
                    style="height: 280px; width: 100%; object-fit: cover"
                  />
                </RouterLink>

                <div class="text-center text-dark mt-2">
                  <RouterLink
                    :to="`product/${item.id}`"
                    class="text-decoration-none stretched-link"
                    >{{ item.title }}</RouterLink
                  >
                </div>
                <div class="d-flex align-items-center justify-content-around">
                  <div
                    class="text-center text-muted text-decoration-line-through mt-2"
                  >
                    原價 NT$ <span>{{ item.origin_price }}</span>
                  </div>
                  <div class="text-center text-danger mt-2">
                    特價 NT$
                    <span class="fw-bold">{{ item.price }}</span>
                  </div>
                </div>
              </div>
              <div
                class="card-body d-flex justify-content-between text-wrap text-sm-nowrap"
              >
                <RouterLink
                  :to="`product/${item.id}`"
                  class="btn btn-outline-primary"
                >
                  查看產品
                </RouterLink>

                <button
                  type="button"
                  class="btn btn-primary"
                  :disabled="loadingState"
                  @click="addToCart(item.id)"
                >
                  加入購物車
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <PaginationView :pages="pagination" @emit-get-page="getProduct" />
</template>

<script>
import emitter from "@/libraries/emitter";
import PaginationView from "@/components/PaginationView.vue";

export default {
  components: {
    PaginationView,
  },
  data() {
    return {
      products: [],
      isLoading: false,
      loadingState: false,
      pagination: {},
    };
  },
  methods: {
    getProduct(page = 1, category) {
      this.isLoading = true;

      let url = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/products/?page=${page}`;
      if (category) {
        url = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/products?category=${category}`;
      }
      this.$http
        .get(url)
        .then((res) => {
          this.products = res.data.products;
          this.pagination = res.data.pagination;
          emitter.emit("get-cart");
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
        qty: 1,
      };
      this.loadingState = true;
      this.$http
        .post(
          `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart`,
          { data },
        )
        .then(() => {
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
          this.loadingState = false;
        })
        .catch((err) => {
          console.log(err.response.data);
        });
    },
  },
  mounted() {
    if (this.$route.query.category) {
      this.getProduct(1, this.$route.query.category);
    } else {
      this.getProduct();
    }
  },
};
</script>

<style lang="scss" scoped>
@media (min-width: 992px) {
  .btn-hover-primary:hover {
    color: white;
    background-color: #994e3d;
  }
}
</style>
