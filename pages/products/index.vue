<template>
  <main>
    <div class="container-fluid">
      <div class="row">
        <div class="col-sm-3"></div>
        <div class="col-sm-6">
          <div class="a-section">
            <div class="a-spacing-top-medium" />
            <h2 style="text-align: center">Add a new Product</h2>
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
              <input type="text" class="a-put-text" style="with: 100%" v-model="title"/>
            </div>
            <!-- Price input stockQuantity-->
            <div class="a-spacing-top-medium">
              <label style="margin-bottom: 0px;">Price</label>
              <input type="number" class="a-put-text" style="with: 100%" v-model="price"/>
            </div>
            <!-- stockQuantity input -->
            <div class="a-spacing-top-medium">
              <label style="margin-bottom: 0px;">StockQuantity</label>
              <input type="number" class="a-put-text" style="with: 100%" v-model="stockQuantity"/>
            </div>
            <!-- Description Textarea -->
            <div class="a-spacing-top-medium">
              <label style="margin-button: 0px;">Description</label>
              <textarea
                placeholder="Provide details such as product description"
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
                  <span class="a-button-text" @click="onAddProduct">Add product</span>
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
  async asyncData({ $axios }) {
    try {
      let categories = $axios.$get('http://localhost:3000/api/categories')
      let owners = $axios.$get('http://localhost:3000/api/owners')

      // We wanted to run parallel at the same time
      const [categoryRes, ownerRes] = await Promise.all([
        categories,
        owners
      ])
      // console.log(categoryRes)
      return {
        categories: categoryRes.categories,
        owners: ownerRes.owners
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
      price: 0,
      description: "",
      selectedFile: null,
      stockQuantity: 1,
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
    async onAddProduct() {
      let data = new FormData()
      data.append("title", this.title)
      data.append("price", this.price)
      data.append("description", this.description)
      data.append("ownerID", this.ownerID)
      data.append("stockQuantity", this.stockQuantity)
      data.append("categoryID", this.categoryID)
      data.append("photo", this.selectedFile, this.selectedFile.name)

      let result = await this.$axios.$post('http://localhost:3000/api/products', data)
      this.$router.push("/")
    }
  }
}
</script>
