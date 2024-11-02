<template>
  <div class="product_page">

    <active-page-template v-if="!admin" :page="$t('products.products')"/>

    <div class="product_content" :class="admin ? 'p-3' : ''">
      <div class="grid_content">
        <div class="filter_products">
          <div class="filter_category_text">
            <p class="filter_text">
              {{ $t('products.filter_category') }}
            </p>
            <div>
              <categories-tree-select @onChangeValue="onChangeCategory"/>
            </div>
          </div>
        </div>
        <div class="grid_part">
          <span class="search_text">{{ $t('products.search_heading') }}</span>
          <div>
            <input v-model="search" class="form-control" type="text" :placeholder="$t('products.search')">
            <i v-if="search" class="close-icon fa-solid fa-xmark" @click="clearSearch"></i>
          </div>
        </div>
      </div>

      <div class="product_cards" :class="currentGridClass">
        <div class="products_cards_child"  >
          <div class="product-card mt-2" v-for="product in products" :key="product.id">
            <single-product
                :product="product"
                :admin="admin"
                @selectProduct="chooseProduct"
                @deleteProduct="deleteProduct"
            />
          </div>
        </div>

        <div v-if="!products.length">
          {{ $t('products.not_found') }}
        </div>
      </div>
    </div>


    <div class="d-flex  mt-5 justify-content-center">
      <b-pagination
          v-model="currentPage"
          :total-rows="totalCount"
          per-page="12"
      ></b-pagination>
    </div>

  </div>
</template>

<script>
import ActivePageTemplate from "@/components/pages/active-page-template.vue";
import ProductsService from "../../../services/ProductsService";
import CategoriesTreeSelect from "../../categories-tree-select.vue";
import SingleProduct from "../product/single-product.vue";

export default {
  name: "Products",
  components: {CategoriesTreeSelect, ActivePageTemplate, SingleProduct},
  metaInfo: {
    title: 'Linare Medical',
    titleTemplate: '%s | Products',
  },
  props: {
    admin: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      selectedCategory: null,
      currentPage: 1,
      totalCount: 0,
      search: '',
      currentOffset: 0,
      products: [],
      currentGridClass: 'product_cards',
    }
  },

  watch: {
    currentPage: function () {
      if (this.selectedCategory) {
        this.getProductsFiltertedByCategory(this.selectedCategory)
      } else if (this.search) {
        this.getProductsFiltertedBySearch(this.search)
      } else {
        this.getProducts()
      }
    },

    search: {
      handler() {
        if (this.search) {
          this.getProductsFiltertedBySearch(this.search)
        } else if (this.selectedCategory) {
          this.getProductsFiltertedByCategory(this.selectedCategory)
        } else {
          this.getProducts()
        }
      }
    },
  },

  computed: {
    locale() {
      return this.$i18n.locale
    }
  },

  mounted() {
    this.getProducts()
  },

  methods: {
    clearSearch() {
      this.search = '';
    },

    async getProducts() {
      const products = await new ProductsService().get({
        page: this.currentPage,
        limit: 12
      })

      this.products = products.data.products
      this.totalCount = products.data.count
    },

    async getProductsFiltertedByCategory(categoryName) {
      const products = await new ProductsService().get({
        category: categoryName,
        page: this.currentPage,
        limit: 12
      })

      this.products = products.data.products
      this.totalCount = products.data.count
    },

    async getProductsFiltertedBySearch(search) {
      const products = await new ProductsService().get({
        search,
        page: this.currentPage,
        limit: 12
      })

      this.products = products.data.products
      this.totalCount = products.data.count
    },

    onChangeCategory(value) {
      this.selectedCategory = value ? value.key : null
      this.currentPage = 1

      if (this.selectedCategory) {
        this.getProductsFiltertedByCategory(value.key)
      } else {
        this.getProducts()
      }
    },

    handleLongText(text) {
      if (text.length > 25) {
        return text.slice(0, 25).concat('...')
      }

      return text
    },

    chooseProduct(product) {
      this.$emit('selectProduct', product)
    },

    deleteProduct(id) {
      this.$emit('deleteProduct', id)
    },
  },

}
</script>

<style scoped>

.form-control{
  height: 50px;
  width: 250px;
}

.form-control::placeholder {
  color: #BDBDC3;
  font-size: 20px;
  font-weight: 400;
}

.close-icon {
  cursor: pointer;
  position: absolute;
  right: 10px;
  bottom: 17px;
  color: #CCCCCC;
  font-size: 13px;
}

.search_text{
  font-size: 17px;
  font-weight: 600;
  color: #2490EB;
}

.close-icon:hover{
  color: red;
}

.btn {
  padding: 0 !important;
}

.one-card-per-row .card-product--card {
  width: 100%;
}

.two-cards-per-row .card-product--card {
  width: 48%;
}

.three-cards-per-row .card-product--card {
  width: 30%;
}

.product_content {
  padding: 3% 13%;
}

.grid_content {
  background-color: #E9E9E9;
  padding: 1%;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  row-gap: 20px;
}

.filter_products {
  width: 520px;
}

.filter_text {
  font-size: 17px;
  font-weight: 600;
  color: #2490EB;
  margin-bottom: 0;
}

.grid_part {
  position: relative;
  display: flex;
  gap: 10px;
  align-items: center;
  flex-wrap: wrap;
}

.btn-group > .btn {
  background-color: #2490EB !important;
  border: none !important;
  padding: 11% 31%;
  gap: 35px;
  display: flex;
  align-items: center;
  font-weight: 600;
}

.filter_category_text {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.products_cards_child {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  transition: transform 150ms ease-out;
  transform: translatex(0px);
}

.products_cards_child .product-card {
  cursor: pointer;
  width: 350px;
  box-shadow: 0 4px 15px 0 rgba(40, 44, 53, 0.06), 0 2px 2px 0 rgba(40, 44, 53, 0.08);
  background-color: #fff;
  border-radius: 4px;
  z-index: 3;
  margin: 10px;
}

.products_cards_child .product-card img:hover {
  opacity: 0.5;
}

.product-card {
  position: relative;
}

.products_cards_child .product-card--footer {
  height: 125px;
  padding: 10px;
  background-color: #86BFEB;
  border-top: 0;
}

.products_cards_child .product-card--footer p {
  padding: 3px 0;
  margin: 0 0 2px;
  font-size: 19px;
  font-weight: 500;
  color: #FFFFFF;
  user-select: none;
}

.products_cards_child .product-card--footer p.price {
  font-size: 15px;
  font-weight: 500;
  padding: 4px;
  color: #FFFFFF;
}

.card-product--card--footer p {
  padding: 3px 0;
  margin: 0 0 2px;
  font-size: 19px;
  font-weight: 500;
  color: #FFFFFF;
  user-select: none;
}

.card-product--card--footer p.product-price {
  font-size: 15px;
  font-weight: 500;
  padding: 4px;
  margin-left: 4px;
  color: #FFFFFF;
}

.product_cards {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  row-gap: 20px;
  margin-top: 6%;
  text-align: center;
}

@media only screen and (max-width: 1245px) {
  .product_content {
    padding: 3% 5%;
  }
}

@media only screen and (max-width: 510px) {
  .filter_category_text {
    flex-direction: column;
    align-items: start;
    justify-content: start;
    row-gap: 10px;
  }

  .grid_content {
    padding: 3%;
  }

  .grid_part{
    flex-direction: column;
    align-items: flex-start;
  }
}


@media (max-width: 367px) {
  .form-control{
    width: 220px;
  }
}
</style>