<template>
  <VueLoading :active="isLoading" />
  <html>
    <body>
      <div class="container login mt-5">
        <form
          class="row g-3 border border-4 border-warning p-4"
          @submit.prevent="login"
        >
          <h1 class="h3 mb-3 text-center font-weight-normal">請先登入</h1>
          <div class="col-12">
            <label for="username" class="form-label">帳號</label>
            <input
              type="text"
              class="form-control"
              id="username"
              placeholder="name@example.com"
              v-model="user.username"
              required
              autofocus
            />
          </div>
          <div class="col-12">
            <label for="password" class="form-label">密碼</label>
            <input
              type="password"
              class="form-control"
              id="password"
              v-model="user.password"
              required
            />
          </div>

          <div class="col-md-6 text-nowrap">
            <button type="submit" class="w-100 btn btn-primary">登入</button>
          </div>
          <div class="col-md-6 text-nowrap">
            <RouterLink to="/" class="w-100 btn btn-primary">回首頁</RouterLink>
          </div>
        </form>
      </div>
    </body>
  </html>
</template>

<script>
export default {
  data() {
    return {
      user: {
        username: "",
        password: "",
      },
      isLoading: false,
    };
  },
  methods: {
    login() {
      const url = `${process.env.VUE_APP_API}/admin/signin`;
      this.$http
        .post(url, this.user)
        .then((res) => {
          const { token, expired } = res.data;
          // 回傳的 token 存到 hexToken
          document.cookie = `myToken=${token}; expires=${new Date(expired)};`;
          this.$router.push("/admin/products");
        })
        .catch((err) => {
          alert(err.response.data.message);
          this.user.password = "";
        });
    },
    loadingEffect() {
      this.isLoading = true;
      setTimeout(() => {
        this.isLoading = false;
      }, 500);
    },
  },
  mounted() {
    this.loadingEffect();
  },
};
</script>

<style lang="scss" scoped>
html,
body {
  height: 100vh;
}

body {
  background-image: url("https://i.pinimg.com/564x/1b/fd/3c/1bfd3cda9146c40b5759244349318146.jpg");
  background-repeat: no-repeat;
  background-position: center center;
  background-size: cover;
  display: flex;
  justify-content: center;
  align-items: center;
}
.login {
  width: 400px;
  display: flex;
  flex-direction: column;
}
.login > form {
  background: rgba(0, 0, 0, 0.1);
}
</style>
