<template>
    <div class="fags-home">
        <div class="container">
            <h2 class="title title-n-b" v-html="$t('page.main.fags')"></h2>
            <div class="fags-home_desc">
                <p v-html="$t('page.main.fags_description')"></p>
            </div>
            <div class="fags-items">
                <ul class="fags-items_ul">
                    <item v-for="faq in info" :content="faq.content" :title="faq.title" :key="faq._id" />
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
import { mapActions, mapGetters } from "vuex";
import item from "~/components/faq/item";

export default {
    components: { item },
    data() {
        return {
            filter: {
                lang: this.$root.$i18n.locale || "en"
            }
        };
    },
    computed: {
        ...mapGetters({
            info: "faq/getData",
        }),
    },
    created() {
        this.getFaqFull(this.filter);
    },
    methods: {
        ...mapActions({
            getFaqFull: "faq/getFaqFull"
        })
    }
};
</script>