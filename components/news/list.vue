<template>
  <div class="news-bloc">
    <div class="bg-right">
      <div v-if="news.length" class="container ">
        <transition name="faded">
          <loader v-if="!load" :responsive="true" />
        </transition>
        <div class="zayvki-filter-main zayvki-filter-main-new" v-if="!pagination">
          <div class="zayvki-filter-main--top">
            <h3 class="title title-n-b">{{ $t("page.news.title") }}</h3>
            <!-- <div class="zayvki-filter-main--top__btn-group">
              <nuxt-link tag="button" :to="localePath(`/news/`)" class="btn btn-look">{{ $t("page.news.all")
                }}</nuxt-link>
            </div> -->
          </div>
        </div>
        <div :class="[{ 'news-bloc_new': !pagination }]">
          <ul :style="{ display: load ? 1 : 0.5 }">
            <li class="news-item" v-for="news in news.slice(0, limit)" :key="news._id" :style="news.title
              ? { cursor: 'pointer' }
              : { background: '#EEE', transition: 'all .3s', minHeight: '370px' }
              " @click="$router.push(localePath(`/news/${news.link}/`))">
              <div class="news-bloc-img">
                <nuxt-link v-if="news.image" :to="localePath(`/news/${news.link}/`)">
                  <img :src="$rest.urlImg(news.image).original" alt="" />
                </nuxt-link>
              </div>

              <div v-if="news.title" class="news-bloc-info">
                <p class="news-bloc-title">{{ news.title }}</p>
                <div class="news-bloc-categor">{{ news.description }}</div>
                <div class="news-bloc-wr-link">
                  <nuxt-link :to="localePath(`/news/${news.link}/`)" class="btn-ne-type-o">Детальніше</nuxt-link>
                </div>

                <!--  {{ $t("page.news.category") }} -->
                <!--<div class="news-bloc-info_item">
                <div class="news-bloc-info_item-photo-name">
                  <div class="news-bloc-info_item-photo">
                    <img src="~assets/img/user-main.svg" alt=""/>
                  </div>
                  <div class="news-bloc-info_item-name">
                    <p>{{ news.author && news.author.name ? news.author.name : "Admin"}}</p>
                    <span>{{ $moment(news.createdAt).format("DD MMM YYYY | HH:mm") }}</span>
                  </div>
                  <div class="news-bloc-info_item-data">
                <span>
                  <img src="~assets/img/visible.svg" alt=""/>
                  <span>{{ news.views }}</span>
                </span>
                    <a href="" class="news-bloc-flag"><img src="~/assets/img/bookmark.svg" alt=""></a>
                  </div>
                </div>
              </div> -->
              </div>
            </li>
          </ul>
        </div>
        <div class="news-wr-btn" v-if="!pagination">
          <nuxt-link :to="localePath(`/news/`)" class="btn-new-all">
            {{ $t("page.main.button_show_all") }}
          </nuxt-link>
        </div>
        <pagination v-if="pagination" :current-page="current_page" :total-pages="total_pages" @page-changed="getNewsFull({
          page: $event,
          limit: limit,
          lang: $root.$i18n.locale
        })" />
      </div>
      <div v-else class="not-found-news">{{ $t("page.news.news-not-found") }}</div>
    </div>

  </div>
</template>

<script>
import { mapActions, mapGetters } from "vuex";
import pagination from "~/components/pagination";

export default {
  components: { pagination },
  props: { pagination: { type: Boolean, default: true }, limit: { type: Number, default: 999 } },
  computed: {
    ...mapGetters({
      news: "news/getData",
      total_pages: "news/getTotalPages",
      current_page: "news/getCurrentPage",
      load: "news/getLoad"
    }),
    lang() {
      return this.$root.$i18n.locale;
    }
  },
  watch: {
    lang() {
      this.getNewsFull({ lang: this.$root.$i18n.locale, limit: this.limit });
    },
  },
  created() {
    this.getNewsFull({ lang: this.$root.$i18n.locale, limit: this.limit });
  },
  methods: {
    ...mapActions({
      getNewsFull: "news/getNewsFull"
    })
  }
};
</script>
