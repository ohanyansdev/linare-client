<template>
  <div id="app">
    <HeaderComponent v-if="isRouteBelongsToUser" />

    <admin-header-component  v-if="!isNotFound  && !isRouteBelongsToUser" />

    <back-to-top-button></back-to-top-button>

    <div  v-if="isRouteBelongsToUser || isNotFound" class="content">
      <router-view></router-view>
    </div>

    <FooterComponent v-if=" isRouteBelongsToUser" />
  </div>
</template>

<script>
import BackToTopButton from "@/components/pages/backToTopButton/BackToTopButton.vue";
import FooterComponent from "./components/pages/footer/FooterComponent.vue";
import HeaderComponent from "./components/pages/header/HeaderComponent.vue";
import AdminHeaderComponent from "@/components/admin/AdminHeaderComponent.vue";

export default {
  name: 'App',
  metaInfo: {
    title: 'Linare Medical',
    titleTemplate: null,
    htmlAttrs: {
      lang: 'en-US'
    },
    meta: [
      { charset: 'utf-8' },
      { name: 'description', content: `Armenian medical devices store | We deliver health and social care products |
            Our technology-driven health and social care products are designed to elevate the quality of patient care |
            Embracing the digital era, our products leverage the power of data and connectivity to create intelligent and intuitive solutions |
            We are committed to making advanced health technologies accessible to a wide range of users. |
            Staying ahead of the curve requires ongoing commitment to research and development. |
            We recognize the importance of collaboration with healthcare professionals.`
      },
      { name: 'viewport', content: 'width=device-width, initial-scale=1' }
    ]

  },
  components: {AdminHeaderComponent, FooterComponent, HeaderComponent, BackToTopButton },
  computed: {
    isNotFound() {
      return this.$route.name ==='NotFound'
    },

    isRouteBelongsToUser() {
      return !!( this.$route.name !=='NotFound' &&  this.$route.meta && this.$route.meta.userRoute);

    }
  },

  mounted() {
    if(!localStorage.getItem('accessToken')) this.$store.dispatch("setCurrentUser", null);
  }

}
</script>

<style scoped>
.content {
  padding-top: 70px;
}
</style>
