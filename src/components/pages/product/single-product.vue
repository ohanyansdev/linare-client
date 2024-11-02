<template>
  <div>
      <div v-if="admin" class="d-flex justify-content-end mb-2">
        <div class="d-flex justify-content-end gap-2">
          <button class="icon_btn" @click="deleteProduct(product.id)">
            <i class="fa-solid fa-trash"></i>
          </button>
          <button class="icon_btn" @click="chooseProduct(product)">
            <i class="fa-solid fa-pen-to-square"></i>
          </button>
        </div>
      </div>
    <img class="card-carousel-img" :src="product.images[0]" alt=""/>

    <button v-b-modal="'modal-center-' + product.id" class="quick_view">{{ $t('landing.quick_view') }}
    </button>
    <div class="product-card--footer">
      <p v-if="locale === 'en'">{{ handleLongText(product.title) }}</p>
      <p v-if="locale === 'ru'">{{ handleLongText(product.title_ru) }}</p>
      <p v-if="locale === 'am'">{{ handleLongText(product.title_am) }}</p>

      <p v-if="locale === 'en'" class="price">{{ product.price }}</p>
      <p v-if="locale === 'ru'" class="price">{{ product.price_ru }}</p>
      <p v-if="locale === 'am'" class="price">{{ product.price_am }}</p>
    </div>

    <b-modal :id="'modal-center-' + product.id" centered>
      <div class="modal_content">
        <img class="modal_img" :src="product.images[0]" alt=""/>
        <div class="modal_info">
          <p v-if="locale === 'en'" class="modal_img_name">{{ product.title }}</p>
          <p v-if="locale === 'ru'" class="modal_img_name">{{ product.title_ru }}</p>
          <p v-if="locale === 'am'" class="modal_img_name">{{ product.title_am }}</p>

          <p v-if="locale === 'en'" class="modal_desc">{{ product.shortDescription }}</p>
          <p v-if="locale === 'ru'" class="modal_desc">{{ product.shortDescription_ru }}</p>
          <p v-if="locale === 'am'" class="modal_desc">{{ product.shortDescription_am }}</p>

          <p v-if="locale === 'en'" class="modal_price">{{ product.price }}</p>
          <p v-if="locale === 'ru'" class="modal_price">{{ product.price_ru }}</p>
          <p v-if="locale === 'am'" class="modal_price">{{ product.price_am }}</p>

          <div style="display: flex; gap: 30px; align-items: center">
            <p class="types">{{ $t('landing.categories') }}</p>
            <p v-for="(category, index) in product.categories" :key="index" class="modal_type">
              <span v-if="locale === 'en'">{{ category.name }}</span>
              <span v-if="locale === 'ru'">{{ category.name_ru }}</span>
              <span v-if="locale === 'am'">{{ category.name_am }}</span>
            </p>
          </div>

          <button class="full_details_btn" @click="onClickFullDetail(product.id)">
            {{ $t('landing.full_details') }}
          </button>
        </div>
      </div>
    </b-modal>
  </div>
</template>

<script>

export  default {
  name: "single-product",
  props: {
    product: {
      type: Object,
      default: () => {
        return {
          title: "",
          title_ru: "",
          title_am: "",
          images: [],
          price: "",
          description: "",
          description_ru: "",
          description_am: "",
          shortDescription: "",
          shortDescription_ru: "",
          shortDescription_am: "",
          categories: [],
          isFavourite: false
        }
      }
    },
    admin: {
      type: Boolean,
      default: false
    }
  },

  computed: {
    locale() {
      return this.$i18n.locale
    }
  },

  methods: {
    handleLongText(text) {
      if(text.length > 25) {
        return  text.slice(0, 25).concat('...')
      }

      return text
    },

    onClickFullDetail(id) {
      this.$router.push({path: '/product/' + id })
    },

    chooseProduct(product) {
      this.$emit('selectProduct', product)
    },

    deleteProduct(id) {
      this.$emit('deleteProduct', id)
    },
  }
}
</script>

<style scoped>
>>>.close {
  width: 30px;
  height: 30px;
  padding-bottom: 3px;
  border: none;
  background-color: #2490EB !important;
  color: white;
  border-radius: 3px;
  cursor: pointer;
  text-align: center;
}

.modal_info {
  margin-top: 3%;
  width: 59%;
}

 .product-card {
  cursor: pointer;
  width: 350px;
  box-shadow: 0 4px 15px 0 rgba(40, 44, 53, 0.06), 0 2px 2px 0 rgba(40, 44, 53, 0.08);
  background-color: #fff;
  border-radius: 4px;
  z-index: 3;
  margin: 10px;
}

.card-carousel-img {
  vertical-align: bottom;
  border-top-left-radius: 4px;
  border-top-right-radius: 4px;
  transition: opacity 150ms linear;
  user-select: none;
  width: 100%;
  height: 350px
}

.product-card img:hover {
  opacity: 0.5;
}

.product-card--footer {
  height: 125px;
  padding: 10px;
  background-color: #86BFEB;
  border-top: 0;
}

.full_details_btn{
  background: #2490EB;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 50px;
  text-align: center;
  display: flex;
  justify-content: center;
  flex-direction: column;
}

.product-card--footer p {
  padding: 3px 0;
  margin: 0 0 2px;
  font-size: 19px;
  font-weight: 500;
  color: #FFFFFF;
  user-select: none;
}

.product-card--footer p.price {
  font-size: 15px;
  font-weight: 500;
  padding: 4px;
  color: #FFFFFF;
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

.product-card {
  position: relative;
}

.quick_view {
  width: 100%;
  border-radius: unset;
  background-color: #2490EB;
  color: white;
  padding: 3% 0;
  transform: translate(0%, 0%);
  opacity: 0;
  transition: opacity 0.3s ease;
  border: none;
  position: absolute;
  z-index: 9999;
  left: 0;
  bottom: 125px;
}

.product-card:hover .quick_view {
  opacity: 1;
}

>>> .modal-dialog {
  max-width: 50% !important;
}

>>> .modal-footer {
  display: none !important;
}

.modal_img {
  width: 45%;
}

.modal_content {
  display: flex;
  gap: 20px;
}

.modal_img_name {
  color: #2490EB;
  font-size: 25px;
  font-weight: 700;
  margin-bottom: 10%;
}

.modal_price {
  font-size: 22px;
  color: #2490EB;
  font-weight: 500;
}

.modal_desc {
  color: #9EA7A8;
  font-weight: 500;
  margin-bottom: 4%;
}

.types {
  color: #2490EB;
  font-weight: 500;
  font-size: 17px;
}

.modal_type {
  color: #A2A2A2;
}

>>> .modal-body {
  padding-bottom: 5% !important;
  padding-top: 0 !important;
}

>>> .modal-header {
  border-bottom: unset !important;
}

@media only screen and (max-width: 1635px) {
  .slide-left > p {
    font-size: 35px;
  }
}

@media only screen and (max-width: 1315px) {
  .modal_content {
    flex-direction: column;
    overflow-y: auto;
    max-height: 373px;
  }

  .modal_img {
    width: 55%;
    margin: 0 auto;
  }

  .modal_info {
    width: 100%;
    padding-left: 3%;
  }

  .modal_img_name {
    margin-bottom: 4%;
  }
}

@media only screen and (max-width: 815px) {
  >>> .modal-dialog {
    max-width: 100% !important;
  }

  .modal_content {
    max-height: 450px;
  }
}
</style>
