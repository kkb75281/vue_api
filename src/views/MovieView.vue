<template>
  <div>
    <HeaderCont />
    <TitleCont name1="movie" name2="reference api" />
    <section class="cont__refer">
      <div class="container">
        <div class="movie__inner">
          <div class="movie__slider">
            <swiper
              :effect="`coverflow`"
              :grabCursor="true"
              :centeredSlides="true"
              :initialSlide="5"
              :slidesPerView="`auto`"
              :autoplay="{
                delay: 2500,
                disableOnInteraction: false,
              }"
              :coverflowEffect="{
                rotate: 50,
                stretch: 0,
                depth: 100,
                modifier: 1,
                slideShadows: false,
              }"
              :pagination="true"
              :modules="modules"
              class="mySwiper"
            >
              <swiper-slide v-for="(slider, index) in sliders" :key="slider.id">
                <div class="item">
                  <a :href="`https://image.tmdb.org/movie/${slider.id}`">
                    <span class="rank">{{ index }}</span>
                    <img
                      :src="`https://image.tmdb.org/t/p/w500/${slider.poster_path}`"
                      :alt="slider.title"
                    />
                    <em>
                      <span class="title">{{ slider.title }}</span>
                      <!-- <span className="star">{props.movie.vote_average}</span> -->
                    </em>
                  </a>
                </div>
              </swiper-slide>
            </swiper>
          </div>
          <!-- movie__slider -->
          <div class="movie__search">
            <div class="container">
              <form @submit.prevent="SearchMovies()">
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
          <div class="movie__movies">
            <div class="container">
              <div class="movie__list">
                <ul>
                  <li v-for="movie in movies" :key="movie.id">
                    <a :href="`https://image.tmdb.org/movie/${movie.id}`">
                      <img
                        :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
                        :alt="movie.title"
                      />
                      <em>
                        <span class="title">{{ movie.title }}</span>
                        <span class="star">{{ movie.vote_average }}</span>
                      </em>
                    </a>
                  </li>
                </ul>
              </div>
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
    const movies = ref([]);
    const sliders = ref([]);
    const search = ref("marvel");

    // 데이터 다운받고 싱크 맞추기(await async), 비동기 맞춰주는 작업
    const SearchMovies = async () => {
      await fetch(
        `https://api.themoviedb.org/3/search/movie?api_key=540588f0940b5e6bdcf873b4608b86f0&query=${search.value}`
      )
        .then((response) => response.json())
        .then((result) => {
          movies.value = result.results;
          search.value = "";
        })
        .catch((error) => console.log(error));
    };
    SearchMovies();

    const TopMovies = () => {
      fetch(
        "https://api.themoviedb.org/3/movie/popular?api_key=540588f0940b5e6bdcf873b4608b86f0"
      )
        .then((response) => response.json())
        .then((result) => (sliders.value = result.results))
        .then((result) => console.log(result))
        .catch((error) => console.log(error));
    };
    TopMovies();

    return {
      movies,
      sliders,
      search,
      SearchMovies,
      modules: [Autoplay, EffectCoverflow, Pagination],
    };
  },
};
</script>

<style lang="scss">
.movie__inner {
  ul {
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;

    li {
      width: 23%;
      position: relative;

      &:hover img {
        scale: 1;
      }

      em {
        display: block;
        height: 80px;
        margin-bottom: 30px;
        font-family: var(--font-kor2);
      }

      img {
        scale: 0.9;
        transition: all 0.3s;
      }

      .title {
        padding: 5px 0;
        display: inline-block;
      }

      .star {
        background: rgba(255, 255, 255, 0.5);
        color: #000;
        position: absolute;
        left: 20px;
        top: 27px;
        width: 30px;
        height: 30px;
        border-radius: 50px;
        text-align: center;
        line-height: 30px;
        font-weight: 800;
      }
    }
  }
  a {
    color: var(--black);
  }
}

.movie__slider {
  .swiper {
    width: 100%;
    padding-bottom: 70px;
  }
  .swiper-slide {
    background-position: center;
    background-size: cover;
    width: 350px;

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

.movie__search {
  margin-top: 100px;
  margin-bottom: 50px;

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
</style>
