<template>
  <div>
    <HeaderCont />
    <TitleCont name1="youtube" name2="reference api" />
    <section class="cont__refer">
      <div class="container">
        <div class="youtube__popular">
          <div class="container">
            <div class="youtube__inner">
              <swiper
                :direction="'vertical'"
                :initialSlide="5"
                :pagination="{
                  clickable: true,
                }"
                :modules="modules"
                class="mySwiper"
              >
                <swiper-slide v-for="random in randoms" :key="random.id">
                  <li>
                    <a
                      :href="`https://www.youtube.com/watch?v=${random.id.videoId}`"
                    >
                      <img
                        :src="`${random.snippet.thumbnails.medium.url}`"
                        :alt="`${random.snippet.description}`"
                      />
                    </a>
                  </li>
                </swiper-slide>
              </swiper>
            </div>
          </div>
        </div>
        <div className="unsplash__search">
          <div className="container">
            <h2>검색하기</h2>
            <form @submit.prevent="SearchYoutubes()">
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
        <div class="youtube__images">
          <ul>
            <li v-for="youtube in youtubes" :key="youtube.id">
              <a
                :href="`https://www.youtube.com/watch?v=${youtube.id.videoId}`"
              >
                <img
                  :src="youtube.snippet.thumbnails.medium.url"
                  :alt="youtube.snippet.description"
                />
              </a>
            </li>
          </ul>
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
import { ref } from "vue";

import { Swiper, SwiperSlide } from "swiper/vue";
import "swiper/css";
// import "swiper/css/effect-flip";
import "swiper/css/pagination";
// import "swiper/css/navigation";
import { Pagination } from "swiper";

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
    const youtubes = ref([]);
    const search = ref("[playlist]");
    const SearchYoutubes = (e) => {
      fetch(
        `https://youtube.googleapis.com/youtube/v3/search?part=snippet&q=${search.value}&key=AIzaSyD6QGqJEKoA6i3vi-8HlZO2Y6S0UoFTKb4&maxResults=30&type=video`
      )
        .then((response) => response.json())
        .then((result) => {
          youtubes.value = result.items;
          search.value = "";
          search.value = e;
        })
        .catch((error) => console.log("error", error));
    };
    SearchYoutubes();
    const RandomYoutubes = () => {
      fetch(
        "https://youtube.googleapis.com/youtube/v3/search?part=snippet&q=[playlist]&key=AIzaSyD6QGqJEKoA6i3vi-8HlZO2Y6S0UoFTKb4&maxResults=10&type=video"
      )
        .then((response) => response.json())
        .then((result) => (randoms.value = result.items))
        .catch((error) => console.log("error", error));
    };
    RandomYoutubes();
    return {
      youtubes,
      search,
      randoms,
      SearchYoutubes,
      RandomYoutubes,
      modules: [Pagination],
    };
  },
};
</script>

<style lang="scss">
.youtube__images {
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
.youtube__btn {
  margin: 30px 0 50px;
  button {
    margin-right: 10px;
    padding: 5px 10px;
    margin-bottom: 10px;
    border-radius: 10px;
    background: #000;
    color: #fff;
    cursor: pointer;
    &:hover {
      border: 1px solid #000;
      background: #fff;
      color: #000;
    }
  }
}
.youtube__inner {
  .swiper {
    width: 100%;
    height: 400px;
  }

  .swiper-slide {
    text-align: center;
    font-size: 18px;

    /* Center slide text vertically */
    // display: -webkit-box;
    // display: -ms-flexbox;
    // display: -webkit-flex;
    // display: flex;
    // -webkit-box-pack: center;
    // -ms-flex-pack: center;
    // -webkit-justify-content: center;
    // justify-content: center;
    // -webkit-box-align: center;
    // -ms-flex-align: center;
    // -webkit-align-items: center;
    // align-items: center;
  }

  .swiper-slide img {
    display: block;
    width: 50%;
    height: 100%;
    margin: 0 auto;
    object-fit: cover;
  }
}
</style>
