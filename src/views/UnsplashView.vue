<template>
  <div>
    <HeaderCont />
    <TitleCont name1="unsplash" name2="reference api" />
    <section class="cont__refer">
      <div class="container">
        <div class="unsplash__inner">
          <div class="unsplash__slider">
            <div class="container">
              <swiper
                :effect="'cards'"
                :initialSlide="5"
                :grabCursor="true"
                :modules="modules"
                class="mySwiper"
              >
                <swiper-slide v-for="random in randoms" :key="random.id">
                  <li>
                    <a :href="`https://unsplash.com/photos/${random.id}`">
                      <img
                        :src="`${random.urls.regular}`"
                        :alt="`${random.urls.alt_description}`"
                      />
                    </a>
                  </li>
                </swiper-slide>
              </swiper>
            </div>
          </div>
          <div class="unsplash__search">
            <div class="container">
              <form @submit.prevent="SearchSplashes()">
                <input
                  type="search"
                  id="search"
                  placeholder="검색하세요!"
                  v-model="search"
                />
                <button type="submit">검색</button>
              </form>
            </div>
          </div>
          <div class="unsplash__images">
            <div class="container">
              <ul>
                <li v-for="splash in splashes" :key="splash.id">
                  <a href="#">
                    <img :src="splash.urls.regular" :alt="splash.id" />
                  </a>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </section>
    <FooterCont />
  </div>
</template>

<script>
import HeaderCont from "@/components/HeaderCont.vue";
import FooterCont from "@/components/FooterCont.vue";
import TitleCont from "@/components/TitleCont.vue";
// import ContactCont from "@/components/ContactCont.vue";
import { ref } from "vue";
import { Swiper, SwiperSlide } from "swiper/vue";
// Import Swiper styles
import "swiper/css";
import "swiper/css/effect-coverflow";
import "swiper/css/pagination";
// import "./style.css";
import { Autoplay, EffectCoverflow, Pagination } from "swiper";

export default {
  components: {
    HeaderCont,
    FooterCont,
    TitleCont,
    Swiper,
    SwiperSlide,
  },

  setup() {
    const randoms = ref([]);
    const splashes = ref([]);
    const search = ref("landscape");

    const SearchSplashes = (e) => {
      fetch(
        `https://api.unsplash.com/search/photos?client_id=jW8UKjJxJ8WDQL7iVnxoQWyaRiLVgBbu-pH9Me1slhM&query=${search.value}`
      )
        .then((response) => response.json())
        .then((result) => {
          splashes.value = result.results;
          search.value = "";
          search.value = e;
        })
        .catch((error) => console.log(error));
    };
    SearchSplashes();

    const RandomSplashes = () => {
      fetch(
        "https://api.unsplash.com/photos/random?client_id=jW8UKjJxJ8WDQL7iVnxoQWyaRiLVgBbu-pH9Me1slhM&count=20"
      )
        .then((response) => response.json())
        .then((result) => (randoms.value = result))
        .catch((error) => console.log(error));
    };
    RandomSplashes();

    return {
      splashes,
      search,
      randoms,
      SearchSplashes,
      RandomSplashes,
      modules: [EffectCoverflow, Pagination, Autoplay],
    };
  },
};
</script>

<style lang="scss">
.unsplash__slider {
  .swiper {
    width: 100%;
    // padding-bottom: 70px;
  }
  .swiper-slide {
    background-position: center;
    background-size: cover;
    width: 700px !important;
    max-height: 400px !important;
    overflow: hidden;

    &:hover img {
      scale: 1;
    }

    &:hover .rank {
      z-index: 1;
      opacity: 1;
    }
  }
  .swiper-slide img {
    position: relative;
    display: block;
    width: 100%;
    scale: 0.93;
    transition: all 0.3s;
  }
  .rank {
    position: absolute;
    top: -10px;
    left: 20px;
    font-family: var(--font-main);
    font-size: 5vw;
    // top: -60px;
    // left: 0px;
    z-index: 1;
    opacity: 0.5;
    transition: all 0.3s;
    color: var(--white);
  }
  .title {
    padding: 5px 0;
    display: inline-block;
  }
}

.unsplash__search {
  margin-top: 100px;
  margin-bottom: 100px;

  .container {
    position: relative;
  }
  h2 {
    color: var(--black);
    font-size: 40px;
    text-indent: -9999px;
    height: 0;
  }
  input {
    background: rgba(255, 255, 255, 0.5);
    border: 1px solid var(--white);
    border-radius: 50px;
    color: var(--black);
    width: 100%;
    padding: 14px 30px;
    font-family: var(--font-kor2);
  }
  button {
    position: absolute;
    right: 10px;
    top: 6px;
    width: 40px;
    height: 40px;
    border-radius: 50%;
    border: 0;
    font-family: var(--font-kor2);
    cursor: pointer;
    z-index: 1000;
  }
}

.unsplash__images {
  .container {
    ul {
      column-count: 4;
      column-gap: 20px;
      width: 100%;

      li {
        margin-bottom: 20px;

        img {
          border-radius: 5px;
        }
      }
    }
  }
}
</style>
