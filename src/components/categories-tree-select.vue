<template>
  <treeselect
      :options="options"
      v-model="value"
      :normalizer="normalizer"
  />
</template>
<script>

import Treeselect from "@riophae/vue-treeselect";
import '@riophae/vue-treeselect/dist/vue-treeselect.css'
import CategoriesService from "../services/CategoriesService";

export default {
  components: { Treeselect },

  data() {
    return {
      value: null,
      options: [],
    }
  },

  watch: {
    value: function () {
      if (this.value) {
        const category = this.options.find(el => el.key === this.value)

        this.$emit('onChangeValue', category)
      } else  {
        this.$emit('onChangeValue', undefined)
      }
    },
    locale: function () {
      this.getCategories()
    }
  },

  computed: {
    locale() {
      return this.$i18n.locale
    }
  },

  mounted() {
    this.getCategories()
  },

  methods: {
    normalizer(node) {
      return {
        id: node.key,
        label: node.name
      }
    },

    chooseCategory(val) {
      this.value = val
    },

    async getCategories() {
      const { data } = await new CategoriesService().get()

      this.options = data.map(el => {
        const name =  this.locale === 'en' ? el.name : this.locale === 'ru' ? el.name_ru : el.name_am
        return {
          key: el.name,
          name,
          data: el
        }
      })
    }
  }
}
</script>

<style>
.vue-treeselect {
  width: 300px;
  font-size: 20px;
}

.vue-treeselect__multi-value-item {
  font-size: 14px;
}

.vue-treeselect__control {
  height: 50px;
}

.vue-treeselect div{
  padding-top: 1%;
}

@media (max-width: 390px) {
  .vue-treeselect{
    width: 220px;
  }
}
</style>
