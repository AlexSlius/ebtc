<template>
  <li class="faq-qst-title-li">
    <div class="faq-qst-title" @click="open = !open">
      {{ title }}
      <i class="faq-qst-arr"><svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 32 32"
          fill="none">
          <circle cx="16" cy="16" r="16" fill="#3B406D" />
          <path d="M10.6667 13.7144L16.0001 18.2858L21.3334 13.7144" stroke="white" stroke-width="2"
            stroke-linecap="round" />
        </svg></i>
    </div>
    <div v-if="open" ref="block" class="b-toggle" v-html="content" />
  </li>
</template>
<script>
export default {
  props: {
    title: { type: String, default: "" },
    content: { type: String, default: "" }
  },
  data() {
    return {
      open: false
    };
  }
};
</script>

<style scoped>
.b-toggle {
  max-height: none;
  opacity: 1;
}
</style>
