<template>
  <main>
    <div class="container-fluid">
      <div class="row">
        <div class="col-sm-3"></div>
        <div class="col-sm-6">
          <div class="a-section">
            <div class="a-spacing-top-medium" />
            <h2 style="text-align: center">Update {{ product.title }}</h2>
          </div>
          <form>
            <!-- Category Dropdown -->
            <div class="a-spacing-top-medium">
              <label>Category</label>
              <select class="a-select-option" v-model="categoryID">
                <option
                  v-for="category in categories"
                  :value="category._id" :key="category._id"
                >
                {{ category.type }}
                </option>
              </select>
            </div>
            <!-- Owner Dropdown -->
            <div class="a-spacing-top-medium">
              <label>Owner</label>
              <select class="a-select-option" v-model="ownerID">
                <option
                  v-for="owner in owners"
                  :value="owner._id" :key="owner._id"
                >
                {{ owner.name }}
                </option>
              </select>
            </div>
            <!-- Title input -->
            <div class="a-spacing-top-medium">
              <label style="margin-bottom: 0px;">Titile</label>
              <input
                type="text"
                class="a-put-text"
                style="with: 100%"
                v-model="title"
                :placeholder="product.title"
              />
            </div>
            <!-- Price input stockQuantity-->
            <div class="a-spacing-top-medium">
              <label style="margin-bottom: 0px;">Price</label>
              <input
                type="number"
                class="a-put-text"
                style="with: 100%"
                v-model="price"
                :placeholder="product.price"
              />
            </div>
            <!-- stockQuantity input -->
            <div class="a-spacing-top-medium">
              <label style="margin-bottom: 0px;">StockQuantity</label>
              <input
                type="number"
                class="a-put-text"
                style="with: 100%"
                v-model="stockQuantity"
                :placeholder="product.stockQuantity"
              />
            </div>
            <!-- Description Textarea -->
            <div class="a-spacing-top-medium">
              <label style="margin-button: 0px;">Description</label>
              <textarea
                :placeholder="product.description"
                style="width: 100%"
                v-model="description"
              />
            </div>
            <!-- Photo Upload -->
            <div class="a-spacing-top-medium">
              <label style="margin-bottom: 0px;">Add Photo</label>
              <div class="a-row a-spacing-top-medium">
                <label class="choosefile-button">
                  <i class="fal fa-plus"></i>
                  <input type="file" @change="onFileSelected"/>
                  <p style="margin-top: -70px">{{ fileName }}</p>
                </label>
              </div>
            </div>
            <hr />
            <!-- Button -->
            <div class="a-spacing-top-large">
              <span class="a-button-register">
                <span class="a-button-inner">
                  <span class="a-button-text" @click="onUpdateProduct">Add product</span>
                </span>
              </span>
            </div>
          </form>
        </div>
        <div class="col-sm-3"></div>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  async asyncData({ $axios, params }) {
    try {
      let categories = $axios.$get('http://localhost:3000/api/categories')
      let owners = $axios.$get('http://localhost:3000/api/owners')
      let product = $axios.$get(`http://localhost:3000/api/products/${params.id}`)

      // We wanted to run parallel at the same time
      const [categoryRes, ownerRes, productRes] = await Promise.all([
        categories,
        owners,
        product
      ])
      console.log(product)
      return {
        categories: categoryRes.categories,
        owners: ownerRes.owners,
        product: productRes.product
      }
    } catch (err) {
      console.log(err)
    }
  },
  data() {
    return {
      categoryID: null,
      ownerID: null,
      title: "",
      price: "",
      description: "",
      selectedFile: null,
      stockQuantity: "",
      fileName: ""
    }
  },
  methods: {
    onFileSelected(event) {
      this.selectedFile = event.target.files[0]
      // console.log(this.selectedFile)
      this.fileName = event.target.files[0].name
      // console.log(this.fileName)
    },
    async onUpdateProduct() {
      let data = new FormData()
      data.append("title", this.title)
      data.append("price", this.price)
      data.append("description", this.description)
      data.append("ownerID", this.ownerID)
      data.append("stockQuantity", this.stockQuantity)
      data.append("categoryID", this.categoryID)
      data.append("photo", this.selectedFile, this.selectedFile.name)

      let result = await this.$axios.$put(
        `http://localhost:3000/api/products/${this.$route.params.id}`
        , data
      )
      this.$router.push("/")
    }
  }
}
</script>
