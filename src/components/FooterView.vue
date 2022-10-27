<!-- eslint-disable prettier/prettier -->
<template>
  <footer class="bg-primary bg-opacity-30 py-1 py-md-2 py-lg-2">
    <div class="container">
       <div class="row justify-content-center mt-2">
            <ul
              class="list-unstyled d-flex justify-content-center align-items-center"
            >
              <li class="fs-3 px-3">
                <a href="#" class="link-light"
                  ><i class="bi bi-instagram"></i
                ></a>
              </li>
              <li class="fs-3 px-3">
                <a href="#" class="link-light"
                  ><i class="bi bi-facebook"></i
                ></a>
              </li>
              <li class="fs-3 px-3">
                <a href="#" class="link-light"><i class="bi bi-github"></i></a>
              </li>
              <li class="fs-3 px-3">
                <RouterLink class="link-light" to="/login"
                ><i class="bi bi-person-circle"></i>
                </RouterLink
              >
              </li>
            </ul>
            <div class="text-center">
              <p>本網站僅供個人作品使用 <br>
              Copyright © 2022 made by tai0525</p>
            </div>
          </div>
        </div>
      </footer>
   <div id="go-top" @click="goTop()">
    <i class="bi bi-arrow-up fs-4"></i>
  </div>
</template>

<script>
export default {
  data() {
    return {
      scrollNum: 0, // 滾動距離
    };
  },
  methods: {
    goTop() {
      document.documentElement.scrollTop = 0;
    },
  },
  mounted() {
    window.addEventListener("scroll", () => {
      const top =
        document.documentElement.scrollTop ||
        document.body.scrollTop ||
        window.pageYOffset;
      this.scrollNum = top;
    });
  },
};
</script>

<style lang="scss" scoped>
#go-top {
  position: fixed;
  right: 10px;
  bottom: 20px;
  text-decoration: none;
  display: inline-block;
  color: #000;
  background: #f5f5f5;
  padding: 5px;
  cursor: pointer;
}
</style>
