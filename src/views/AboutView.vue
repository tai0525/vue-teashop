<!-- eslint-disable no-irregular-whitespace -->
<template>
  <VueLoading :active="isLoading" />

  <div class="wrap bg-warning bg-gradient">
    <div class="container py-3">
      <h2 class="text-center px-3">關於我們</h2>
      <div class="row justify-content-center">
        <div
          class="col-md-12 col-lg-6 d-flex justify-content-center align-items-center"
        >
          <img
            src="../assets/images/about.jpg"
            alt="關於我們"
            style="height: 70%; object-fit: cover"
            class="img-fluid w-md-50 w-sm-100"
          />
        </div>
      </div>
    </div>
    <div class="container pb-5">
      <div class="row mb-5">
        <div class="col d-flex flex-column mw-100 align-items-center">
          <div class="fs-5">
            <p>
              「茶」，這個字拆解後，就是人處在草木之間，對千千萬萬華人而言，茶就好似清風明月一樣的與我們同在。喝茶確實已不是為了止渴而為的一樁小事，而是具有精神意義的行為。
            </p>
          </div>
          <div class="fs-5">
            <p>
              沁記茶行，位於台中市太平區，走過了百年的日子，目前由第四代吳維合先生經營。傳承了選茶的專業經驗，走訪台灣各地茶區、越過各個山頭，從選茶開始，製茶、分級、包裝都親手監製，以時間所積累的深厚底蘊，及無可取代的焙茶技術，謹守最完美的茶品細節，並在各大茶葉賽事中屢獲大獎。
            </p>
          </div>
        </div>
      </div>
      <div class="row mt-3">
        <div class="col-md-6">
          <div class="mt-3 d-flex flex-column">
            <h2 class="fw-bold">聯絡我們</h2>
            <p class="py-2 h5">有任何問題，歡迎與我們聯繫。</p>

            <div class="m-2">
              <ul class="list-unstyled fs-4">
                <li class="pb-2">
                  <a href="tel:04-2022-2022" class="text-decoration-none"
                    ><i class="bi bi-telephone-fill pe-2"></i>04-2022-2022</a
                  >
                </li>
                <li class="pb-2">
                  <a
                    href="mailto:chinji2022@example.com"
                    class="text-decoration-none"
                    ><i class="bi bi-envelope-fill pe-2"></i
                    >chinji2022@example.com</a
                  >
                </li>
                <li>
                  <i class="bi bi-geo-alt-fill">台中市太平區新平路二段100號</i>
                </li>
              </ul>
            </div>
          </div>
        </div>
        <div class="col-md-6">
          <iframe
            src="https://www.google.com/maps/embed?pb=!1m14!1m8!1m3!1d7281.833363231639!2d120.715064!3d24.139564!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x0%3A0xc67044ef4a06e8d1!2z6Ie65Lit5biC5aSq5bmz5Y2A5a6c5qyj5ZyL5rCR5bCP5a24!5e0!3m2!1szh-TW!2stw!4v1666576625627!5m2!1szh-TW!2stw"
            width="100%"
            height="400"
            style="border: 0"
            loading="lazy"
            class="pt-3"
          ></iframe>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import emitter from "@/libraries/emitter";
export default {
  data() {
    return {
      isLoading: false,
    };
  },
  methods: {
    LoadingEffect() {
      this.isLoading = true;
      setTimeout(() => {
        this.isLoading = false;
      }, 500);
    },
  },
  mounted() {
    this.LoadingEffect();
    emitter.emit("toggle-menu");
  },
};
</script>
