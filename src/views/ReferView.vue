<template>
  <div>
    <HeaderCont />
    <TitleCont name1="reference" name2="api" />
    <section class="cont__refer">
      <div class="container">
        <div class="refer__inner">
          <h2>CSS</h2>
          <ul class="refer__list">
            <li v-for="refer in refers" :key="refer.title">
              <a herf="#">
                <span>{{ refer.num }}</span>
                <span>{{ refer.title }}</span>
                <span>{{ refer.desc }}</span>
                <span>{{ refer.descStar }}</span>
              </a>
            </li>
          </ul>
        </div>
      </div>
    </section>
    <ContactCont />
    <FooterCont />
  </div>
</template>

<script>
import HeaderCont from "@/components/HeaderCont.vue";
import FooterCont from "@/components/FooterCont.vue";
import TitleCont from "@/components/TitleCont.vue";
import ContactCont from "@/components/ContactCont.vue";
import { ref } from "vue";

export default {
  components: {
    HeaderCont,
    FooterCont,
    TitleCont,
    ContactCont,
  },

  setup() {
    const refers = ref([]);

    const references = () => {
      fetch("https://kkb75281.github.io/react_api/src/utils/reference.json")
        .then((response) => response.json())
        // .then((result) => console.log(result.cssRefer))
        .then((result) => (refers.value = result.cssRefer))
        .catch((error) => console.log("error", error));
    };
    references();

    return {
      refers,
      references,
    };
  },
};
</script>

<style lang="scss">
.refer__inner {
  padding-bottom: 200px;

  h2 {
    font-size: 30px;
    color: var(--black);
  }
}

.refer__list {
  border: 1px solid var(--bg-light-border);
  border-top-width: 2px;
  border-bottom-width: 2px;
  border-right: 0;
  border-left: 0;

  li {
    border-bottom: 1px solid var(--bg-light-border);
    transition: all 0.3s;

    &:hover {
      padding: 10px 0;
      background-color: rgba(255, 255, 255, 0.3);
    }
    a {
      display: flex;
      align-items: center;
      width: 100%;
      color: var(--black);
      font-family: var(--font-kor2);

      span {
        display: inline-block;
        padding: 15px 20px;
      }
      .num {
        flex: 1 1 5%;
        text-align: center;
        border-right: 1px solid var(--bg-light-border);
      }
      .name {
        flex: 1 1 20%;
        border-right: 1px solid var(--bg-light-border);
      }
      .desc {
        flex: 1 1 65%;
        border-right: 1px solid var(--bg-light-border);
      }
      .star {
        flex: 1 1 10%;
        text-align: center;
      }
    }
  }
}
</style>
