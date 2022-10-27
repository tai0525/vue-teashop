<template>
  <VueLoading :active="isLoading" />
  <div class="container text-primary">
    <h3 class="coffee-header">台灣茶常見種類：帶你了解六大茶系</h3>

    <p class="pb-1">
      六大類的茶葉雖因地區不同而生產製程略有差異，但其生產過程會影響其茶葉產品品質，因此基本生產程序卻是有一定步驟，以下進行簡略說明：
    </p>

    <table class="table table-bordered border-secondary">
      <tbody>
        <thead>
          <th width="15%">茶類名稱</th>
          <th width="20%">茶葉種類</th>
          <th width="30%">加工方式</th>
        </thead>
        <tr>
          <td>綠茶</td>
          <td>屬不發酵茶</td>
          <td>茶菁經蒸菁或炒菁後進行揉捻後乾燥。</td>
        </tr>
        <tr>
          <td>黃茶</td>
          <td>屬不發酵茶</td>
          <td>茶菁經炒菁後，進行揉捻而後悶黃，後續加以乾燥。</td>
        </tr>
        <tr>
          <td>白茶</td>
          <td>屬部分發酵茶</td>
          <td>茶菁經長時間萎凋後，以烘菁再加以輕度揉捻，而後乾燥。</td>
        </tr>
        <tr>
          <td>青茶</td>
          <td>屬部分發酵茶</td>
          <td>
            將茶菁經日光萎凋，並經過一段時間之靜置與攪拌，而後進行炒菁、揉捻，最後乾燥。
          </td>
        </tr>
        <tr>
          <td>紅茶</td>
          <td>屬全發酵茶</td>
          <td>
            茶菁經萎凋後進行揉捻，並給予足夠時間進行發酵氧化，後續加以乾燥。
          </td>
        </tr>
        <tr>
          <td>黑茶</td>
          <td>屬後發酵茶</td>
          <td>
            茶菁經殺菁後進行揉捻，並進行渥堆微生物發酵後，再加以乾燥成為黑毛茶，後續則進一步加以蒸壓成形，形成緊壓之黑茶。
          </td>
        </tr>
      </tbody>
    </table>

    <h3 class="coffee-header">台灣茶的六大產區在哪裡？</h3>

    <p class="pb-1">
      台灣茶最早聞名中外是因為特有的烏龍茶而聞名，而台灣得天獨厚的地理環境與氣候更是讓台灣茶好喝的秘訣，在台灣，茶區幾乎遍佈全島，而不同的土壤環境及海拔更造就了不同台灣茶的口感，比較出名的六大產區分別為：
    </p>

    <ol class="text-dark pb-2">
      <li>
        阿里山茶區：最有名的就是阿里山烏龍茶，當地氣候涼爽寒冷，早晚雲霧繚繞，日照時間少，優質的環境才能培養出柔軟厚實的茶菁。阿里山茶區產的烏龍茶，香氣淡雅回甘，茶味不澀，是早期台灣高級茶的代稱，更是被許多海外市場所喜愛。
      </li>
      <li>
        鹿谷茶區：南投是台灣產茶重要的茶園，而鹿谷鄉最具有名的就是凍頂烏龍了，凍頂山上的鳳凰村,永隆村,彰雅村所產的凍頂烏龍更是一絕。而產於魚池鄉的日月潭紅茶，最出名的是紅玉紅茶，茶湯明亮鮮紅，是亞洲最有名的紅茶精品之一，茶香具有天然肉桂和淡淡的薄荷香，口感鮮明溫潤，帶著香氣與天然的甘甜。
      </li>
      <li>
        梨山茶區：海拔位在1600至2600公尺，是台灣平均高度最高的茶區，因為日夜溫差大、冬季常有下雪因此茶葉生長緩慢，也因而造就了茶菁結構厚實口感甘醇，加上海拔高度高，因此茶葉冷礦味特重。此外梨山盛產高山水果，茶園分佈在果樹之中，成長時會吸收天然果香，因此製作出來的梨山茶還帶有天然梨香，非常特別。
      </li>
      <li>
        玉山茶區：位在南投縣信義鄉的玉山茶區，海拔約為1000-1800公尺，主要茶種是青心烏龍和金萱茶。但玉山茶區常年雲霧繚繞，水氣豐沛，茶區產出來的茶特別香醇回甘，但因為地勢原因，茶葉產量不多。
      </li>
      <li>
        杉林溪茶區：海拔約為1800公尺左右，位於南投竹山鎮，是阿里山的支脈，衫林溪茶區終年生長在杉樹之間，沖泡出來的茶略帶杉樹味道，帶著杉木香和綠竹味是杉林溪茶區的特有茶香。
      </li>
      <li>
        大禹嶺茶區：海拔高度約為2800公尺，是最高海拔的茶區，由於生長在高海拔地區，茶葉苦澀度降低，葉片也因此比一般茶葉更厚實，因此大禹嶺茶茶湯氣味都更為厚實，香氣更足，特殊的醇厚豆香，帶有高山烏龍的霸氣香味，令茶人品嚐後趨之若鶩。
      </li>
    </ol>
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

<style scoped>
* {
  margin: auto;
}
.coffee-header {
  margin: 0.75rem auto;
  padding: 0.25rem 0.5rem;
  --bs-text-opacity: 1;
  --bs-bg-opacity: 1;
  color: rgba(var(--bs-light-rgb), var(--bs-text-opacity));
  background-color: rgba(var(--bs-primary-rgb), var(--bs-bg-opacity));
  font-weight: 700;
}
</style>
