<template>
  <header class="header">
    <div v-if="favoriteRates.length" class="course">
      <ul>
        <li v-for="rate in favoriteRates" :key="rate.id">
          <div>
            <p class="course-header">
              <span>{{ rate.name }}</span>
              <span>{{ rate.service }}</span>
            </p>
            <p class="course-footer green">
              <span>{{ rate.rate.toFixed(2) }}</span>
            </p>
          </div>
        </li>
      </ul>
    </div>
    <div class="container">
      <div class="heaer-row">
        <div class="logo">
          <nuxt-link :to="localePath('/')">
            <img src="~/assets/img/logo-new-des.svg" alt="" />
          </nuxt-link>
        </div>

        <!--Загрузка Лого с админки а с 19 по 23 вставил картинку logo3.svg-->
        <!-- <div class="logo"> 
        <nuxt-link :to="localePath('/')">
          <img :src="$rest.logoPath" alt="" />
        </nuxt-link>
      </div> -->

        <nav class="header-menu">
          <div class="burger" @click="changeMenu(!showMenu)">
            <div class="burger-icon" />
          </div>
          <ul :class="{ 'menu-open': showMenu }" class="menu-list">
            <li class="only-mobile">
              <nuxt-link :to="localePath('/')">
                {{ $t("ui.header.exchange") }}
              </nuxt-link>
            </li>
            <li>
              <nuxt-link :to="localePath('/reviews/')">
                {{ $t("ui.header.reviews") }}
              </nuxt-link>
            </li>
            <li>
              <nuxt-link :to="localePath('/partners/')">
                {{ $t("ui.header.partners") }}
              </nuxt-link>
            </li>
            <li>
              <nuxt-link :to="localePath('/rules/')">
                {{ $t("ui.header.rules") }}
              </nuxt-link>
            </li>
            <li>
              <nuxt-link :to="localePath('/rules/aml-kyc-policy/')">
                {{ $t("ui.header.kyc_aml") }}
              </nuxt-link>
            </li>
            <li>
              <nuxt-link :to="localePath('/faq/')">
                {{ $t("ui.header.faq") }}
              </nuxt-link>
            </li>
            <li>
              <nuxt-link :to="localePath('/news/')">
                {{ $t("ui.header.news") }}
              </nuxt-link>
            </li>
            <li v-for="b in buttons" v-if="b.lang === $i18n.locale && b.isHeaderMenu" :key="b.id">
              <nuxt-link :to="localePath('/' + b.link + '/')">
                {{ b.nameLink }}
              </nuxt-link>
            </li>
          </ul>
        </nav>

        <div class="right-block">
          <div class="headr-time">
            <p>{{ $t("ui.header.time_top") }}</p>
            <div class="headr-time_center">
              <div>08<span>00</span></div> - <div>19<span>00</span></div></div>
            <p>{{ $t("ui.header.time_bot") }}</p>
          </div>
          <div class="dropdown-lang">
            <div class="inner_dropdown" @click="isShow = !isShow">
              <div class="dropdown-lang-inner-img">
                <img :src="icon">
              </div>
            </div>
            <div v-if="isShow" class="lang-list-wr">
              <ul>
                <li v-for="locale in $i18n.locales" :key="locale.code" v-if="locale.code != $i18n.locale">
                  <nuxt-link :to="switchLocalePath(locale.code)" @click.prevent.stop="$i18n.setLocale(locale.code)"
                    :class="{ 'active-lang-link': locale.code === lang, pointer: true }">
                    <div v-if="locale.code === 'ru'">
                      <img src="~/assets/img/flag/ru.svg">
                      <span>{{ locale.name }}</span>
                    </div>
                    <div v-else-if="locale.code === 'ua' || locale.code === 'uk'">
                      <img src="~/assets/img/flag/uk.svg">
                      <span>{{ locale.name }}</span>
                    </div>
                    <div v-else-if="locale.code === 'en'">
                      <img src="~/assets/img/flag/us.svg">
                      <span>{{ locale.name }}</span>
                    </div>
                    <div v-else-if="locale.code === 'pl'">
                      <img src="~/assets/img/flag/pl.svg">
                      <span>{{ locale.name }}</span>
                    </div>
                    <div v-else-if="locale.code === 'es'">
                      <img src="~/assets/img/flag/es.svg">
                      <span>{{ locale.name }}</span>
                    </div>
                    <div v-else>
                      <span>[Lang]</span>
                    </div>
                  </nuxt-link>
                </li>
              </ul>
            </div>
          </div>
          <logo-sign />
        </div>
      </div>
    </div>
  </header>
</template>

<script>
import { mapActions, mapGetters, mapMutations } from "vuex";
import logoSign from "~/components/header/logosign";

export default {
  data: () => {
    return {
      isShow: false
    };
  },
  components: {
    logoSign
  },

  computed: {
    lang() {
      return this.$i18n.locale;
    },
    icon() {
      if (this.$i18n.locale === 'ua' || this.$i18n.locale === 'uk') {
        return require('~/assets/img/flag/uk.svg')
      } else if (this.$i18n.locale === 'eu') {
        return require('~/assets/img/flag/us.svg')
      } else if (this.$i18n.locale === 'pl') {
        return require('~/assets/img/flag/pl.svg')
      } else if (this.$i18n.locale === 'es') {
        return require('~/assets/img/flag/es.svg')
      } else if (this.$i18n.locale === 'ru') {
        return require('~/assets/img/flag/ru.svg')
      } else {
        return require('~/assets/img/flag/us.svg')
      }
    },

    ...mapGetters({
      showMenu: "menu/menu",
      buttons: "menu/buttons",
      link: "rules/getLink"
    }),
    ...mapGetters({ favoriteRates: "exchange/favoriteRates" })
  },
  created() {
    this.favRoutes();
    this.getButtons();
  },
  methods: {
    ...mapMutations({ changeMenu: "menu/menu" }),
    ...mapActions({
      favRoutes: "exchange/favRoutes",
      getButtons: "menu/buttons",
      menuAllChange: "menu/all"
    })
  }
};
</script>
