<template>
<div>
    <div>
      <div class="d-flex justify-content-center mt-5">
        <div class="admin_products col-lg-6">
          <div class="card admin_products_content">
            <div class="card-body admin_products">

              <input type="file" class="form-control mb-3" :disabled="product.images.length > 0"  :key="refreshElements" multiple @change="previewFiles">

              <div>EN</div>
              <input v-model="product.title" class="form-control mb-3" type="text" placeholder="Name">

              <div>RU</div>
              <input v-model="product.title_ru" class="form-control mb-3" type="text" placeholder="Name">

              <div>AM</div>
              <input v-model="product.title_am" class="form-control mb-3" type="text" placeholder="Name">

              <div>EN</div>
              <input v-model="product.shortDescription" class="form-control mb-3" type="text" placeholder="Short Description">

              <div>RU</div>
              <input v-model="product.shortDescription_ru" class="form-control mb-3" type="text" placeholder="Short Description">

              <div>AM</div>
              <input v-model="product.shortDescription_am" class="form-control mb-3" type="text" placeholder="Short Description">

              <div class="mt-3">
                <div>EN</div>
                <vue-editor v-model="product.description" id="editorEN" />
              </div>

              <div class="mt-3">
                <div>RU</div>
                <vue-editor v-model="product.description_ru" id="editorRU"/>
              </div>

              <div class="mt-3">
                <div>AM</div>
                <vue-editor v-model="product.description_am" id="editorAM" />
              </div>

              <div class="mt-3">
                <div>EN</div>
                <input v-model="product.price" class="form-control mb-3" type="text" placeholder="Price">
              </div>

              <div class="mt-3">
                <div>RU</div>
                <input v-model="product.price_ru" class="form-control mb-3" type="text" placeholder="Price">
              </div>

              <div class="mt-3">
                <div>AM</div>
                <input v-model="product.price_am" class="form-control mb-3" type="text" placeholder="Price">
              </div>

              <div class="mt-3">
                <input v-model="product.video" class="form-control mb-3" type="text" placeholder="Video">
                <iframe
                    v-if="product.video && product.video !== 'null'"
                    width="300"
                    height="200"
                    :src="product.video"
                    frameborder="0"
                    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
                    allowfullscreen
                >

                </iframe>
              </div>

              <div class="mt-3">
                <div>Favourite</div>
                <input v-model="product.isFavourite" class="mb-3" type="checkbox">
              </div>
              <div class="mt-2">
                Select Categories
                <treeselect  v-model="product.categories"  :multiple="true" :options="categoriesOptions"/>
              </div>

              <div class="d-flex justify-content-end mt-3">
                <button v-if="!product.id" class="btn btn-primary add_testimonials" @click="createProduct()">Add Product</button>
                <button v-if="product.id" class="btn btn-primary add_testimonials" @click="updateProduct()">Update Product</button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

  <div class="mt-2">
    <products
        v-if="refresh"
        admin
        @selectProduct="chooseProduct"
        @deleteProduct="deleteProduct"
    />

  </div>
</div>
</template>

<script>
import Products from "../../pages/products/Products.vue";
import ProductsService from "../../../services/ProductsService";
import Treeselect from "@riophae/vue-treeselect";
import CategoriesService from "../../../services/CategoriesService";
import {generateFormData} from "../../../../helper/generateFormData";
import { VueEditor } from 'vue2-quill-editor'


export default {
  components: {
    Treeselect,
    Products,
    VueEditor
  },
  data() {
    return {
      refreshElements: 0,
      categoriesOptions: [],
      totalCount: 0,
      refresh: true,
      product: {
        title: "",
        title_ru: "",
        title_am: "",
        images: [],
        price: "",
        price_ru: "",
        price_am: "",
        description: "",
        description_ru: "",
        description_am: "",
        shortDescription: "",
        shortDescription_ru: "",
        shortDescription_am: "",
        categories: [],
        isFavourite: false
      },
      products: []
    };
  },

  mounted() {
    this.getCategories()
  },

  methods: {
    async getCategories() {
      const { data } = await new CategoriesService().get()

      this.categoriesOptions = data.map(el => {
        return {
          id: el.name,
          label: el.name,
          data: el
        }
      })
    },

    async createProduct() {
      try {
        if (!this.product.title) return
        if (!this.product.images.length) return

        this.refresh = false

        const formData = generateFormData(this.product)

        for (const i of Object.keys(this.product.images)) {
          formData.append('images', this.product.images[i])
        }

        await new ProductsService().post(formData)

        this.product = {
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

        this.refreshElements++
      } finally {
        this.refresh = true
      }

    },

    async deleteProduct(id) {
      try {
        if (!id) return
        this.refresh = false

        await new ProductsService().delete(id)
      } finally {
        this.refresh = true
      }
    },

    chooseProduct(product) {
      this.product = {...product, categories: product.categories.map(el => el.name)}
    },

    previewFiles(event) {
      this.product.images = event.target.files;
    },

    async updateProduct() {
      try {
        if (!this.product.id) return

        if (!this.product.title) return

        this.refresh = false

        const formData = generateFormData(this.product)

        for (const i of Object.keys(this.product.images)) {
          formData.append('images', this.product.images[i])
        }

        await new ProductsService().put(formData, this.product.id)

        this.product = {
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

        this.refreshElements++
      } finally {
        this.refresh = true
      }
    },
  }
};
</script>

<style>

.btn-primary{
  background-color: #2490EB !important;
}

@media (max-width: 910px) {
  .admin_products{
    width: 85%;
  }

  .card-body{
    width: 100%;
  }
}

</style>
