<template>
  <main>
    <div class="container-fluid">
      <div class="row">
        <div class="col-sm-3"></div>

        <div class="col-sm-6">
          <div class="a-section">
            <div class="a-spacing-top-medium"></div>
            <h2 style="text-align: center">Update {{ product.title }}</h2>
            <!-- category dropdown -->
            <form>
              <div class="a-spacing-top-medium">
                <label>Category</label>
                <select class="a-select-option" v-model="categoryID">
                  <option
                    v-for="category in categories"
                    :value="category._id"
                    :key="category._id"
                    >{{ category.type }}</option
                  >
                </select>
              </div>
              <!-- owner dropdown -->
              <div class="a-spacing-top-medium">
                <label>Owner</label>
                <select class="a-select-option" v-model="ownerID">
                  <option
                    v-for="owner in owners"
                    :value="owner._id"
                    :key="owner._id"
                    >{{ owner.name }}</option
                  >
                </select>
              </div>

              <div class="a-spacing-top-medium">
                <label style="margin-bottom: 0px">Title</label>
                <input
                  type="text"
                  class="a-input-text"
                  style="width: 100%"
                  v-model="title"
                  :placeholder="product.title"
                />
              </div>

              <div class="a-spacing-top-medium">
                <label style="margin-bottom: 0px">Price</label>
                <input
                  type="number"
                  class="a-input-text"
                  style="width: 100%"
                  v-model="price"
                  :placeholder="product.price"
                />
              </div>

              <div class="a-spacing-top-medium">
                <label style="margin-bottom: 0px">Stock Quantity</label>
                <input
                  type="number"
                  class="a-input-text"
                  style="width: 100%"
                  v-model="stockQuantity"
                  :placeholder="product.stockQuantity"
                />
              </div>

              <div class="a-spacing-top-medium">
                <label style="margin-bottom: 0px">Description</label>
                <textarea
                  style="width: 100%"
                  v-model="description"
                  :placeholder="product.description"
                />
              </div>

              <div class="a-spacing-top-medium">
                <label style="margin-bottom: 0px">Add Photo</label>
                <div class="a-row a-spacing-top-medium">
                  <label for class="choosefile-button">
                    <i class="fal fa-plus"></i>
                    <input type="file" name id />
                    <p style="margin-top: -70px">name of</p>
                  </label>
                </div>
              </div>

              <div class="a-spacing-top-medium">
                <label style="margin-bottom: 0px">Photo Url</label>
                <input
                  type="text"
                  class="a-input-text"
                  style="width: 100%"
                  v-model="photo"
                  :placeholder="product.photo"
                />
              </div>

              <hr />
              <div class="a-spacing-top-large">
                <span class="a-button-register">
                  <span class="a-button-inner">
                    <span class="a-button-text" @click="onUpdateProduct"
                      >Update Product</span
                    >
                  </span>
                </span>
              </div>
            </form>
          </div>
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
      let categories = $axios.$get("http://localhost:3000/api/categories");
      let owners = $axios.$get("http://localhost:3000/api/owners");
      let product = $axios.$get(
        `http://localhost:3000/api/products/${params.id}`
      );

      const [
        categoryResponse,
        ownerResponse,
        productResponse
      ] = await Promise.all([categories, owners, product]);

      console.log(productResponse);

      return {
        categories: categoryResponse.categories,
        owners: ownerResponse.owners,
        product: productResponse.product
      };
    } catch (err) {
      console.log(err);
    }
  },

  data() {
    return {
      categoryID: null,
      ownerID: null,
      title: "",
      price: "",
      description: "",
      photo: "",
      stockQuantity: ""
    };
  },

  methods: {
    async onUpdateProduct() {
      try {
        let data = {
          title: this.title,
          price: this.price,
          description: this.description,
          ownerID: this.ownerID,
          categoryID: this.categoryID,
          photo: this.photo,
          stockQuantity: this.stockQuantity
        };
        let result = await this.$axios.$put(
          `http://localhost:3000/api/products/${this.$route.params.id}`,
          data
        );

        this.$router.push("/");
      } catch (err) {
        console.log(err);
      }
    }
  }
};
</script>
