<!---->
<template>
  <div class="services text-center">
    <div class="container">
      <h2 class="title title-n-b" v-html="$t('page.main.stars.title')"></h2>
      <ul class="services_items">
        <li>
          <div class="service-item text-center">
            <div class="service-item_img">
              <img src="~assets/img/like-new.svg" alt=""/>
            </div>
            <p class="service-item_title">
              {{ $t("page.main.stars.guarantee_t") }}
            </p>
            <div class="services-txt">{{ $t("page.main.stars.guarantee") }}</div>
          </div>
        </li>
        <li>
          <div class="service-item text-center">
            <div class="service-item_img">
              <img src="~assets/img/credit-card-new.svg" alt=""/>
            </div>
            <p class="service-item_title">
             {{ $t("page.main.stars.privacy_t") }}
            </p>
            <div class="services-txt">{{ $t("page.main.stars.privacy") }}</div>
          </div>
        </li>
        <li>
          <div class="service-item text-center">
            <div class="service-item_img">
              <img src="~assets/img/telephone-new.svg" alt=""/>
            </div>
            <p class="service-item_title">
             {{ $t("page.main.stars.support_t") }}
            </p>
            <div class="services-txt">{{ $t("page.main.stars.support") }}</div>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>
