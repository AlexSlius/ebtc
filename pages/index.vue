<template>
  <div class="pages">
    <div class=" right-bg form-index-top-wr">
      <div class="left-bg">
        <div class="form-index">
          <div class="container container--form">
            <div v-if="webAlerts && webAlerts.length">
              <template v-for="alert in webAlerts.filter(el => el.lang === $root.$i18n.locale && isNeedShowAlert(el))"
                v-if="!warning.includes(alert._id)">
                <div :key="alert._id" class="notification-box" :class="alert.level">
                  <div class="notification-box_content" v-html="alert.content"></div>
                  <button class="notification-box-button_x" @click="closeWarning(alert._id)">
                    <svg xmlns="http://www.w3.org/2000/svg" width="10" height="10" viewBox="0 0 10 10" fill="none">
                      <g clip-path="url(#clip0_0_777)">
                        <path
                          d="M6.64999 5.00031L9.74362 1.90668C10.0854 1.56487 10.0854 1.0107 9.74362 0.669421L9.3312 0.257001C8.98928 -0.0849194 8.43511 -0.0849194 8.09383 0.257001L5.00031 3.35052L1.90668 0.25636C1.56487 -0.0854535 1.0107 -0.0854535 0.669421 0.25636L0.25636 0.66878C-0.0854535 1.0107 -0.0854535 1.56487 0.25636 1.90615L3.35052 5.00031L0.257001 8.09383C-0.0849194 8.43575 -0.0849194 8.98992 0.257001 9.3312L0.669421 9.74362C1.01124 10.0854 1.5654 10.0854 1.90668 9.74362L5.00031 6.64999L8.09383 9.74362C8.43575 10.0854 8.98992 10.0854 9.3312 9.74362L9.74362 9.3312C10.0854 8.98928 10.0854 8.43511 9.74362 8.09383L6.64999 5.00031Z"
                          fill="white" />
                      </g>
                      <defs>
                        <clipPath id="clip0_0_777">
                          <rect width="10" height="10" fill="white" />
                        </clipPath>
                      </defs>
                    </svg>
                  </button>
                </div>
              </template>
            </div>
            <!--<div class="title-text">
              <h1 class="text-center" v-html="$t('page.main.title')">
              </h1>
              <p class="text-center">{{ $t("page.main.descr") }}</p>
            </div> -->
            <!-- <div class="relative">
              <exchanger />
              <client-only>
                <technicalWork v-if="getTechnicalWorkStatus" />
              </client-only>
            </div> -->
            <div class="new-calculator">
              <newCaclucator />
              <client-only>
                <technicalWork v-if="getTechnicalWorkStatus" />
              </client-only>
            </div>
          </div>
        </div>
      </div>
    </div>
    <stars />
    <div class="gb-img-net"></div>
    <faqs />
    <reserves />
    <news :limit="3" :pagination="false" />
    <!-- <reviews /> -->
    <!-- <div class="partner-block">
      <partner />
    </div> -->
  </div>
</template>

<script>
import { mapGetters, mapActions } from "vuex";
import partner from "~/components/homepage/partner";
import news from "~/components/news/list";
import reviews from "~/components/homepage/reviews";
import reserves from "~/components/homepage/reserves";
import exchanger from "~/components/homepage/formindex";
import stars from "~/components/homepage/stars";
import technicalWork from "~/components/technical-work";
import newCaclucator from "~/components/homepage/exchangeForms/v4c.vue";
import faqs from "~/components/homepage/faqs";

export default {
  data() {
    return {
      warning: []
    };
  },
  components: {
    partner,
    news,
    reviews,
    reserves,
    exchanger,
    stars,
    technicalWork,
    newCaclucator,
    faqs
  },
  computed: {
    ...mapGetters({ info: "exchange/info", getTechnicalWorkStatus: "params/getTechnicalWorkStatus" }),
    ...mapGetters("notifications", ["webAlerts", "load"])
  },
  head() {
    const { meta, title } = this.$seo({
      name: this.$i18n.t("meta.main.title"),
      title: this.$i18n.t("meta.main.title"),
      description: this.$i18n.t("meta.main.description"),
      openGraph: {
        title: this.$i18n.t("meta.main.title"),
        description: this.$i18n.t("meta.main.description"),
        content: this.$i18n.t("meta.main.description"),
        image: { url: this.$rest.baseUrl + this.$rest.faviconPath }
      }
    })
    return { meta, title };
  },
  async asyncData({ store }) {
    if (process.server) {
      // seo generate main page.
      await store.dispatch("exchange/routes")
    }
  },
  methods: {
    ...mapActions("notifications", ["getList"]),
    isNeedShowAlert({ timeShowFrom, timeShowTo }) {
      const currTime = this.$moment().utc().format('HH:mm');
      if (timeShowFrom <= timeShowTo) {
        return currTime >= timeShowFrom && currTime <= timeShowTo;
      } else {
        if (currTime >= timeShowTo) {
          return currTime >= timeShowFrom;
        } else {
          return currTime <= timeShowTo;
        }
      }
    },
    closeWarning(id) {
      this.warning.push(id)
    }
  },
  created() {
    this.getList({ lang: this.$root.$i18n.locale });
    if (process.client && this.$route.query.ref) {
      console.log("Set new ref:", this.$route.query.ref)
      localStorage.setItem("partner", this.$route.query.ref);
    }
  }
};
</script>

<style lang="scss">
.relative {
  position: relative;
}
</style>