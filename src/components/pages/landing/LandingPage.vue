<template>
  <div>
    <div class="landing_slide">
      <b-carousel
          id="carousel-fade"
          fade
          :interval="10000"
          controls
      >
        <b-carousel-slide v-for="(item, index) in introCarousel" :key="index">
          <template #img>
            <div class="slide-block-image">
              <img
                  v-if="index === 0"
                  class="d-block img-fluid w-100"
                  src="../../../assets/linare-landing-2.jpg"
                  style="height: 100%; object-fit: cover"
                  alt=""
              >
              <img
                  v-if="index === 1"
                  class="d-block img-fluid w-100"
                  src="../../../assets/linare-landing-1.jpg"
                  style="height: 100%; object-fit: cover"
                  alt=""
              >
              <img
                  v-if="index === 2"
                  class="d-block img-fluid w-100"
                  src="../../../assets/linare-landing-3.jpg"
                  style="height: 100%; object-fit: cover"
                  alt=""
              >
            </div>

          </template>
          <div class="text_content">
            <div class="slide-left">
              <p>{{ $t(item.title) }}</p>
            </div>
          </div>
        </b-carousel-slide>
      </b-carousel>
    </div>


    <div class="about">
      <div class="icons_part" v-for="(item, index) in about" :key="index">
        <i class="fa_icons" :class="['fas', item.class]"></i>
        <div>
          <p>{{ $t(item.text1) }}</p>
          <p>{{ $t(item.text2) }}</p>
        </div>
      </div>
    </div>

    <div class="products">
      <h1 class="products_heading">{{ $t('landing.products') }}</h1>

      <div v-if="products.length" class="card-carousel-wrapper">
        <div>
          <div class="products-cards">
            <div class="products-card-child">
              <div class="product-card mt-2" v-for="product in products" :key="product.id">
                <single-product :product="product" />
              </div>
            </div>
          </div>
        </div>
      </div>

      <div v-else>
        {{ $t('landing.no_data') }}
      </div>
    </div>

    <app-partners />


    <div class="contact_us">
      <h3 class="contact_us_heading">{{ $t('landing.welcome_support') }}</h3>
      <p class="contact_us_text">{{ $t('landing.need_help') }}</p>

      <div class="contact_us_info">
        <i class="fa-solid fa-phone-volume"></i>
        <div class="info_text">
          <div class="info_text_column">
            <div>
              <p>{{ $t('landing.contact_us') }}</p>
              <h4 class="info_heading">{{ $t('landing.doubts') }}</h4>
            </div>
            <div>
              <p>{{ contacts.phone_1 }}</p>
              <p>{{ contacts.phone_2 }}</p>
            </div>
          </div>
          <router-link to="/contacts">
            <button class="know_more">{{ $t('landing.know_more') }}</button>
          </router-link>
        </div>
      </div>
    </div>

  </div>
</template>
<script>

import ContactsService from "../../../services/ContactsService";
import CollaboratorsService from "../../../services/CollaboratorsService";
import ProductsService from "../../../services/ProductsService";
import SingleProduct from "../product/single-product.vue";
import AppPartners from "../../app/app-partners.vue";

const introCarousel = [
  {
    title: 'landing.carousel_text2'
  },
  {
    title: 'landing.carousel_text3'
  },
  {
    title: 'landing.carousel_text1'
  }
]

const about = [
  {
    class: "fa-hotel",
    text1: "landing.helpline"
  },
  {
    class: "fa-truck-fast",
    text1: "landing.free_shipping"
  },
  {
    class: "fa-percent",
    text2: "landing.save_big"
  }
]

export default {
  name: "LandingPage",
  components: {AppPartners, SingleProduct},
  metaInfo: {
    title: 'Linare Medical',
    titleTemplate: '%s | Home',
  },
  data() {
    return {
      currentOffset: 0,
      windowSize: 3,
      paginationFactor: 220,
      collaborators: [],
      introCarousel,
      about,
      products: [],
      contacts: {
        phone_1: '',
        phone_2: '',
        email: '',
        address: '',
        facebook: '',
        instagram: '',
      }
    }
  },

  computed: {
    locale() {
      return this.$i18n.locale
    }
  },

  mounted() {
    this.getContacts()
    this.getCollaborators()
    this.getProducts()
  },

  methods: {
    async getProducts() {
      const products = await new ProductsService().getFavouriteProducts()
      this.products = products.data.products
    },

    async getContacts() {
      const contacts = await new ContactsService().get()
      if (contacts.data?.contacts) {
        this.contacts = contacts.data.contacts
      }
    },

    async getCollaborators() {
      const collaborators = await new CollaboratorsService().get()
      this.collaborators = collaborators.data.collaborators
    }
  }
}
</script>

<style scoped>
:deep(.sr-only) {
  display: none;
}

.text_content {
  width: 100%;
}

:deep(.carousel-caption) {
  position: absolute;
  right: unset !important;
  top: 30% !important;
  left: 15% !important;
  padding-top: 1.25rem;
  padding-bottom: 1.25rem;
  color: #fff;
  text-align: start !important;
  width: 28% !important;
}

.img-fluid {
  object-fit: cover;
  height: 700px;
}

.slide-block-image {
  display: flex;
  height: 79vh;
}

.slide-left-block {
  width: 60%
}

.slide-left {
  width: 100%;
  animation: 3s slide-left;
  text-align: center;
}

.slide-left > p {
  font-size: 38px;
  text-align: center;
  font-weight: 500;
  color: #14457B;
}

@keyframes slide-left {
  from {
    margin-left: 100%;
  }
  to {
    margin-left: 0;
  }
}

:deep(.carousel-control-prev) {
  opacity: 1 !important;
}

:deep(.carousel-control-next) {
  opacity: 1 !important;
}

:deep(.carousel-control-next-icon), :deep(.carousel-control-prev-icon) {
  filter: brightness(0.2) sepia(1) hue-rotate(193deg) saturate(5);
  border-radius: 3px;
  height: 2.5rem !important;
  width: 2.5rem !important;
}

.fa_icons {
  font-size: 25px;
}

.products {
  padding: 3%;
  text-align: center;
}

.card-carousel-wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 20px 0 40px;
  color: #666a73;
}

.products-cards {
  overflow: hidden;
}

.products-card-child {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  transition: transform 150ms ease-out;
  transform: translatex(0px);
}

.products-card-child .product-card {
  cursor: pointer;
  width: 350px;
  box-shadow: 0 4px 15px 0 rgba(40, 44, 53, 0.06), 0 2px 2px 0 rgba(40, 44, 53, 0.08);
  background-color: #fff;
  border-radius: 4px;
  z-index: 3;
  margin: 10px;
}

.products-card-child .product-card img:hover {
  opacity: 0.5;
}

.products-card-child .product-card--footer {
  height: 125px;
  padding: 10px;
  background-color: #86BFEB;
  border-top: 0;
}

.products-card-child .product-card--footer p {
  padding: 3px 0;
  margin: 0 0 2px;
  font-size: 19px;
  font-weight: 500;
  color: #FFFFFF;
  user-select: none;
}

.products-card-child .product-card--footer p.price {
  font-size: 15px;
  font-weight: 500;
  padding: 4px;
  color: #FFFFFF;
}

.products_heading {
  color: #2490EB;
  font-size: 35px;
  padding-bottom: 2%;
}

.about {
  background-color: #86BFEB;
  padding: 2%;
  display: flex;
  flex-wrap: wrap;
  row-gap: 15px;
  justify-content: space-evenly;
}

.icons_part {
  display: flex;
  gap: 20px;
  color: #FFFFFF;
  font-weight: 500;
  align-items: center;
}

.icons_part > div > p {
  margin: 0;
}

.contact_us {
  padding: 3%;
  text-align: center;
}

.contact_us_heading {
  color: #1C8DEA;
  font-weight: 500;
}

.contact_us_text {
  color: #949494;
  font-weight: 500;
  padding-top: 1%;
  font-size: 20px;
}

.contact_us_info {
  padding: 2%;
  border: 3px solid #14457B;
  width: 35%;
  border-radius: 15px;
  margin: 4% auto 0;
  display: flex;
  gap: 30px;
  align-items: center;
}

.info_text {
  text-align: start;
  color: #3F3F3F;
}

.info_text_column {
  display: flex;
  flex-direction: column;
  gap: 10px;
  font-size: 20px;
  font-weight: 500;
  padding-bottom: 7%;
}

.info_text > div > div > p {
  margin: 0;
}

.know_more {
  background-color: #2490EB;
  color: #ffffff;
  border: none;
  padding: 0.7%;
  border-radius: 5px;
  font-weight: 500;
  position: absolute;
  margin-top: 7px;
  width: auto;
}

.product-card {
  position: relative;
}

.fa-phone-volume::before {
  font-size: 35px;
}

@media only screen and (max-width: 1635px) {
  .slide-left > p {
    font-size: 30px;
  }
}

@media only screen and (max-width: 1250px) {
  .slide-left-block {
    display: none;
  }
}

@media only screen and (max-width: 1315px) {
  :deep(.carousel-caption) {
    width: 50% !important;
  }
}

@media only screen and (max-width: 1195px) {
  .info_heading {
    font-size: 22px;
  }

  .info_text_column {
    font-size: 17px;
  }
}

@media only screen and (max-width: 920px) {
  .know_more {
    width: 15%;
    margin-top: -7px;
  }

  .contact_us_info {
    flex-direction: column;
    align-items: center;
    gap: 10px;
  }
}

@media only screen and (max-width: 885px) {
  .contact_us_info {
    padding: 2% 7%;
    width: 100%;
    flex-direction: unset;
    align-items: center;
    gap: 30px;
  }

  .know_more {
    width: auto;
    padding: 1.5% 8%;
  }

  .contact_us {
    padding: 7% 4%;
  }

  .img-fluid {
    height: 550px !important;
  }

  .slide-block-image{
    height: auto;
  }
}

@media only screen and (max-width: 668px) {
  .about {
    row-gap: 20px;
    justify-content: flex-start;
    flex-direction: column;
    padding: 3% 3% 3% 10%;
  }
}

@media only screen and (max-width: 655px) {
  .slide-left > p {
    font-size: 25px;
    margin-top: 8%;
  }
}

@media only screen and (max-width: 495px) {
  .slide-left > p {
    font-size: 22px;
    max-height: 20%;
  }

  :deep(.carousel-caption) {
    width: 73% !important;
  }

  .products_heading{
    font-size: 25px;
  }

  .contact_us {
    padding: 10% 4%;
  }

  .contact_us_heading {
    font-size: 20px;
  }

  .contact_us_text {
    display: none;
  }

  .contact_us_text {
    font-size: 17px;
  }
}

@media only screen and (max-width: 400px) {
  .products-card-child .product-card{
    width: 100%;
    object-fit: contain;
  }
}
</style>