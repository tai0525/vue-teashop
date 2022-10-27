<template>
  <VueLoading :active="isLoading" />
  <div class="wrap">
    <template v-if="cartData.carts.length !== 0">
      <ProgressView step="1" />
    </template>
    <section class="pt-5">
      <div class="container">
        <div class="row">
          <template v-if="cartData.carts.length == 0">
            <div class="col-12 pb-4">
              <div class="d-flex justify-content-center">
                <div class="d-flex flex-column align-items-center">
                  <i
                    class="bi bi-cart-fill cart-icon"
                    style="font-size: 10rem"
                  ></i>
                  <p class="text-danger fs-5 fw-bold">您的購物車沒有任何商品</p>
                  <RouterLink to="/user/products" class="btn btn-warning">
                    購物去</RouterLink
                  >
                </div>
              </div>
            </div>
          </template>
        </div>

        <template v-if="cartData.carts.length !== 0">
          <div class="row py-5">
            <div class="col">
              <div class="card">
                <div class="card-header bg-primary text-white">
                  <div class="text-end">
                    <button
                      class="btn btn-outline-warning"
                      type="button"
                      @click="confirmDeleteAllCart"
                      :disabled="cartData.carts.length === 0"
                    >
                      清空購物車
                    </button>
                  </div>
                </div>
                <div class="card-body">
                  <div class="table-responsive">
                    <table class="table">
                      <thead>
                        <tr class="align-middle">
                          <th scope="col"></th>
                          <th scope="col" class="text-wrap" style="width: 25%">
                            產品名稱
                          </th>
                          <th scope="col" class="text-nowrap">單價</th>
                          <th
                            scope="col"
                            class="text-nowrap"
                            style="width: 30%"
                          >
                            數量
                          </th>
                          <th scope="col" class="text-nowrap">單位</th>
                          <th class="text-center text-nowrap" scope="col">
                            價格
                          </th>
                        </tr>
                      </thead>

                      <tbody class="text-nowrap">
                        <tr v-for="item in cartData.carts" :key="item.id">
                          <td>
                            <i
                              class="bi bi-x-circle fs-5"
                              style="color: red"
                              :disabled="loadingState === item.id"
                              @click="removeCartItem(item.id)"
                            ></i>
                          </td>
                          <td scope="row">
                            {{ item.product.title }}
                            <div class="text-success" v-if="item.coupon">
                              已套用優惠券
                            </div>
                          </td>
                          <td>$ {{ item.product.price }}</td>
                          <td>
                            <input
                              min="1"
                              type="number"
                              class="form-control"
                              v-model.number="item.qty"
                              :disabled="loadingState === item.id"
                              @change="updateCartNumber(item)"
                            />
                          </td>
                          <td>
                            <span>{{ item.product.unit }}</span>
                          </td>
                          <td class="text-center">
                            <div v-if="item.final_total !== item.total">
                              <small class="text-success">折扣價：</small>
                              $
                              {{
                                $filters.numberAddComma(
                                  item.total - item.final_total,
                                )
                              }}
                            </div>
                            <div v-else>
                              ${{ $filters.numberAddComma(item.final_total) }}
                            </div>
                          </td>
                        </tr>
                      </tbody>

                      <tfoot>
                        <tr>
                          <td colspan="5" class="text-end">總計</td>
                          <td class="text-end">
                            $ {{ $filters.numberAddComma(cartData.total) }}
                          </td>
                        </tr>
                        <tr v-if="cartData.final_total !== cartData.total">
                          <td colspan="5" class="text-end">折扣後價格</td>
                          <td class="text-end text-danger">
                            $
                            {{
                              $filters.numberAddComma(
                                cartData.total - cartData.final_total,
                              )
                            }}
                          </td>
                        </tr>
                      </tfoot>
                    </table>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <div class="row justify-content-between align-items-center pb-3">
            <div class="col-md-5">
              <div class="input-group mb-3">
                <input
                  type="text"
                  class="form-control"
                  v-model="discountCode"
                  placeholder="請輸入優惠碼"
                />

                <button
                  type="button"
                  class="btn btn-outline-secondary text-primary"
                  :disabled="cartData.carts.length === 0 || enterCouponState"
                  @click="addCoupon"
                >
                  套用優惠碼
                </button>
              </div>
            </div>

            <div
              class="col-md-3 d-flex justify-content-end align-items-start"
              v-if="cartData.carts.length !== 0"
            >
              <RouterLink to="/user/order" class="btn btn-lg btn-primary"
                >結帳去</RouterLink
              >
            </div>
          </div>
        </template>
      </div>
    </section>
  </div>
</template>

<script>
import emitter from "@/libraries/emitter";
import ProgressView from "@/components/ProgressView.vue";

export default {
  components: {
    ProgressView,
  },
  data() {
    return {
      cartData: {
        carts: [],
      },
      discountCode: "",
      enterCouponState: true,
      loadingState: "",
      isLoading: false,
    };
  },
  watch: {
    discountCode() {
      if (this.discountCode !== "") {
        this.enterCouponState = false;
      } else {
        this.enterCouponState = true;
      }
    },
  },
  methods: {
    getCartList() {
      const url = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart`;
      this.$http
        .get(url)
        .then((res) => {
          this.cartData = res.data.data;
          emitter.emit("toggle-menu");
        })
        .catch((err) => {
          console.log(err.response.data);
        });
    },
    updateCartNumber(item) {
      if (item.qty == 0 || item.qty < 0) {
        this.$swal.fire({
          title: `數量至少為一個`,
          icon: "warning",
          confirmButtonColor: "#994e3d",
          showClass: {
            popup: "animate__animated animate__fadeOutDown",
          },
          hideClass: {
            popup: "animate__animated animate__fadeOutUp",
          },
        });
        this.getCartList();
        return;
      }
      this.loadingState = item.id;
      const data = {
        product_id: item.id,
        qty: item.qty,
      };

      const url = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart/${item.id}`;
      this.$http
        .put(url, { data })
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
            title: "已變更商品數量",
          });
          this.getCartList();
          this.loadingState = "";
          emitter.emit("get-cart");
        })
        .catch((err) => {
          console.log(err.response.data);
        });
    },
    removeCartItem(id) {
      this.loadingState = id;
      const url = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart/${id}`;
      this.$http
        .delete(url)
        .then(() => {
          this.getCartList();
          this.loadingState = "";
          emitter.emit("get-cart");
        })
        .catch((err) => {
          console.log(err.response.data);
        });
    },
    confirmDeleteAllCart() {
      this.$swal
        .fire({
          title: "確定是否刪除購物車全部商品",
          icon: "warning",
          showCancelButton: true,
          confirmButtonColor: "#994e3d",
          cancelButtonColor: "#d33",
          confirButtonmText: "確定",
          cancelButtonText: "取消",
        })
        .then((result) => {
          if (result.isConfirmed) {
            this.confirmDeleteAllCartItem();
            this.$swal.fire("已全部刪除購物車商品", "", "success");
          }
        });
    },
    confirmDeleteAllCartItem() {
      const url = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/carts`;
      this.$http
        .delete(url)
        .then(() => {
          this.getCartList();
          emitter.emit("get-cart");
        })
        .catch((err) => {
          console.log(err.response.data);
        });
    },
    addCoupon() {
      this.isLoading = true;
      const url = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/coupon`;
      const couponCode = {
        code: this.discountCode,
      };
      this.$http
        .post(url, { data: couponCode })
        .then(() => {
          this.getCartList();
          this.isLoading = false;
        })
        .catch(() => {
          this.isLoading = false;
          this.$swal.fire({
            title: `折扣碼失效`,
            icon: "warning",
            confirmButtonColor: "#994e3d",
            showClass: {
              popup: "animate__animated animate__fadeInDown",
            },
            hideClass: {
              popup: "animate__animated animate__fadeOutUp",
            },
          });
        });
    },
    LoadingEffect() {
      this.isLoading = true;
      setTimeout(() => {
        this.isLoading = false;
      }, 500);
    },
  },

  mounted() {
    this.getCartList();
    this.LoadingEffect();
  },
};
</script>
