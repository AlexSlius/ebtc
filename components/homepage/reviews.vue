<template>
  <div class="reviews-bloc">
    <div class="container">
      <h3 class="title-other title-n-b">
        {{ $t("page.main.reviews.title") }}
      </h3>
      <ul>
        <li v-for="reviewItem in reviewsArray" :key="reviewItem._id">
          <div class="reviews-bloc-txt">
            <svg class="rew-icon-top" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"
              fill="none">
              <path fill-rule="evenodd" clip-rule="evenodd"
                d="M18.617 9.41992C20.971 9.87592 22.75 11.9499 22.75 14.4359C22.75 17.2559 20.461 19.5449 17.641 19.5449C14.821 19.5449 12.532 17.2559 12.532 14.4359C12.532 10.6779 13.798 8.34392 15.284 6.88492C17.559 4.65092 20.408 4.45692 20.408 4.45692C20.698 4.43592 20.973 4.58492 21.115 4.83792C21.258 5.08992 21.241 5.40292 21.073 5.63892C21.073 5.63892 19.719 7.54492 18.875 8.95492C18.787 9.10292 18.698 9.26592 18.617 9.41992Z"
                fill="#1C457D" />
              <path fill-rule="evenodd" clip-rule="evenodd"
                d="M7.33502 9.41992C9.68902 9.87592 11.468 11.9499 11.468 14.4359C11.468 17.2559 9.17902 19.5449 6.35902 19.5449C3.53902 19.5449 1.25002 17.2559 1.25002 14.4359C1.25002 10.6779 2.51602 8.34392 4.00202 6.88492C6.27702 4.65092 9.12602 4.45692 9.12602 4.45692C9.41602 4.43592 9.69102 4.58492 9.83302 4.83792C9.97502 5.08992 9.95902 5.40292 9.79102 5.63892C9.79102 5.63892 8.43702 7.54492 7.59302 8.95492C7.50502 9.10292 7.41602 9.26592 7.33502 9.41992Z"
                fill="#1C457D" />
            </svg>
            <p class="reviews-bloc__">{{ reviewItem.message }}</p>
            <svg class="rew-icon-bot" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"
              fill="none">
              <path fill-rule="evenodd" clip-rule="evenodd"
                d="M5.383 14.5801C3.029 14.1241 1.25 12.0501 1.25 9.56408C1.25 6.74408 3.539 4.45508 6.359 4.45508C9.179 4.45508 11.468 6.74408 11.468 9.56408C11.468 13.3221 10.202 15.6561 8.716 17.1151C6.441 19.3491 3.592 19.5431 3.592 19.5431C3.302 19.5641 3.027 19.4151 2.885 19.1621C2.742 18.9101 2.759 18.5971 2.927 18.3611C2.927 18.3611 4.281 16.4551 5.125 15.0451C5.213 14.8971 5.302 14.7341 5.383 14.5801Z"
                fill="#1C457D" />
              <path fill-rule="evenodd" clip-rule="evenodd"
                d="M16.665 14.5801C14.311 14.1241 12.532 12.0501 12.532 9.56408C12.532 6.74408 14.821 4.45508 17.641 4.45508C20.461 4.45508 22.75 6.74408 22.75 9.56408C22.75 13.3221 21.484 15.6561 19.998 17.1151C17.723 19.3491 14.874 19.5431 14.874 19.5431C14.584 19.5641 14.309 19.4151 14.167 19.1621C14.025 18.9101 14.041 18.5971 14.209 18.3611C14.209 18.3611 15.563 16.4551 16.407 15.0451C16.495 14.8971 16.584 14.7341 16.665 14.5801Z"
                fill="#1C457D" />
            </svg>
          </div>

          <div class="reviews-bloc-client">
            <div class="reviews-bloc-photo">
              <img src="~/assets/img/user-main.svg" alt="" />
            </div>

            <div>
              <p>
                {{ reviewItem.name || "Anonymous" }}
              </p>
            </div>
          </div>

        </li>
      </ul>
      <div class="news-wr-btn" v-if="!pagination">
        <nuxt-link :to="localePath(`/reviews/`)" class="btn-new-all">
          {{ $t("page.main.button_show_all") }}
        </nuxt-link>
      </div>
    </div>
  </div>
</template>

<script>
import { mapActions } from "vuex";

export default {
  data() {
    return { reviews: [] };
  },
  computed: {
    reviewsArray() {
      return this.reviews.slice(0, 3);
    },
    lang() {
      return this.$root.$i18n.locale;
    }
  },
  watch: {
    lang() {
      this.updateList();
    }
  },
  async created() {
    await this.updateList();
  },
  methods: {
    ...mapActions({
      updateListReviews: "reviews/updateListReviews"
    }),
    async updateList() {
      const result = await this.updateListReviews({ lang: this.lang });
      if (result) {
        this.reviews = result.reviews;
      }
    }
  }
};
</script>
