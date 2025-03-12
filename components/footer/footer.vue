<template>
  <footer class="footer">
    <div class="footer-bottom">
      <div class="container">
        <div class="footer__row">
          <div class="footer_left">
            <div class="footer__icon">
              <img src="~/assets/img/logo-new-des.svg" alt="" />
            </div>
            <div class="footer__socials">
              <a href="#" class="icon-footer-social i-f-g"></a>
              <a href="#" class="icon-footer-social i-f-f"></a>
              <a href="#" class="icon-footer-social i-f-x"></a>
              <a href="#" class="icon-footer-social i-f-i"></a>
              <a href="#" class="icon-footer-social i-f-in"></a>
            </div>
            <div class="footer_left-bot">
              {{ $t("ui.footer.dev_by") }}</br>
              <a href="https://www.smartexpay.com/" target="_blank">
                {{ $t("ui.footer.box_exchanger") }}
              </a>
            </div>
          </div>
          <div class="footer_menu">
            <ul>
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
                <nuxt-link :to="localePath('/rules/aml-kyc-policy/')">
                  {{ $t("ui.header.kyc_aml") }}
                </nuxt-link>
              </li>
              <li>
                <nuxt-link :to="localePath('/faq/')">
                  {{ $t("ui.header.faq") }}
                </nuxt-link>
              </li>
              
            </ul>

            <ul>
              <li v-for="b in buttons" v-if="b.lang === $root.$i18n.locale && b.isFooterMenu" :key="b.id">
                <nuxt-link :to="localePath('/' + b.link + '/')">
                  {{ b.nameLink }}
                </nuxt-link>
              </li>
              <li>
                <nuxt-link :to="localePath('/rules/')">
                  {{ $t("ui.footer.rules") }}
                </nuxt-link>
              </li>
              <li>
                <nuxt-link :to="localePath('/contacts/')">
                  {{ $t("ui.footer.contacts") }}
                </nuxt-link>
              </li>
              <li>
                <nuxt-link :to="localePath('/news/')">
                  {{ $t("ui.header.news") }}
                </nuxt-link>
              </li>
            </ul>
          </div>
          <div class="footer_right">
            <div class="footer-title">{{ $t("ui.header.time_top") }}</div>
            <div class="footer-time">08:00 - 19:00</div>
            <div class="footer-week">{{ $t("ui.header.time_bot") }}</div>
          </div>
        </div>
      </div>
    </div>
  </footer>
</template>

<script>
import { mapGetters } from "vuex";

export default {
  computed: { ...mapGetters("menu", ["menu", "buttons"]) }
};
</script>
