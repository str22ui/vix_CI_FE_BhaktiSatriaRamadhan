<script>
import axios from 'axios';
import "@/assets/styles/page.css";

export default {
  name: "ProductDisplay",
  data() {
    return {
      singleData: null,
      productId: 1,
      maxProductId: 0,
      categoryColor: '',
      isWomenClothing: false,
      isLoading: false,
    };
  },
  created() {
    this.fetchMaxProductId();
    this.fetchSingleData();

  },
  methods: {
    fetchSingleData() {
      this.isLoading = true;
      axios
        .get(`https://fakestoreapi.com/products/${this.productId}`)
        .then(response => {
          this.singleData = response.data;
          this.getCategoryColor(this.singleData.category);
            this.isLoading = false;
        })
        .catch(error => {
          console.error(error);
             this.isLoading = false;
        });
    },
    fetchMaxProductId() {
      axios
        .get('https://fakestoreapi.com/products')
        .then(response => {
          const products = response.data;
          this.maxProductId = products.length;
        })
        .catch(error => {
          console.error(error);
        });
    },
    previousProduct() {
      if (this.productId > 1) {
        this.productId--;
        this.fetchSingleData();
      }
    },
    nextProduct() {
        if (this.productId < this.maxProductId) {
            this.productId++;
        } else {
            this.productId = 1; // Set productId to 1 if it reaches the maximum value
        }
        this.fetchSingleData();

        },
        getCategoryColor(category) {

          if (category === "women's clothing") {
          this.categoryColor = 'color-women';
          document.body.style.background = 'url("/src/assets/images/dot.png"), linear-gradient(to bottom, #FDE2FF 65%, #FFFF 50%)';

        } else if (category === "men's clothing") {
          this.categoryColor = 'color-men';
          document.body.style.background = 'url("/src/assets/images/dot.png"), linear-gradient(to bottom, #D6E6FF 65%, #FFFF 50%)';

          document.body.style.backgroundSize = '100% auto';
          document.body.style.backgroundSize = '1000px';
        } else {
          this.categoryColor = 'color-default';
          document.body.style.background = 'linear-gradient(to bottom, #DCDCDC 65%, #FFFFFF 50%)';
        }


        },
  },
};
</script>
<template>
  <div
    class="background"
  >
    <p
      class="category"
      >{{ singleData && singleData.id }}</p
    >
    <div
      class="container"
    >
      <div
        v-if="isLoading"
        class="loader-container"
      >
        <div
          class="loader"
        ></div>
      </div>
      <div
        v-else
      >
        <div
          v-if="singleData && (singleData.category === 'women\'s clothing' || singleData.category === 'men\'s clothing')"
          class="card"
        >
          <div
            class="left-side"
          >
            <img
              v-if="singleData && singleData.image"
              :src="singleData.image"
              alt="product-image"
            />
          </div>
          <div
            class="right-side"
          >
            <h2
              class="title"
              :class="categoryColor"
              >{{ singleData && singleData.title }}</h2
            >
            <div
              class="content"
            >
              <div
                class="kategori"
              >
                <div>
                  <p
                    class="category"
                    >{{ singleData && singleData.category }}</p
                  >
                </div>
                <div
                  class="rating"
                >
                  <span
                    class="rating-value"
                    >2.9/5</span
                  >
                  <div
                    class="stars"
                    :class="categoryColor"
                  >
                    <span
                      class="star"
                    ></span>
                    <span
                      class="star"
                    ></span>
                    <span
                      class="star"
                    ></span>
                    <span
                      class="star2"
                    ></span>
                    <span
                      class="star2"
                    ></span>
                  </div>
                </div>
              </div>
              <hr />
              <textarea
                name=""
                id=""
                cols="58"
                rows="10"
                >{{ singleData && singleData.description }}</textarea
              >
              <hr />
              <p
                class="price"
                :class="categoryColor"
                >${{ singleData && singleData.price }}</p
              >
              <div
                class="button"
              >
                <button
                  class="btn-buy"
                  :class="categoryColor"
                  >Buy
                  Now</button
                >
                <button
                  class="btn-next"
                  @click="nextProduct"
                  :class="categoryColor"
                  >Next
                  Product</button
                >
              </div>
            </div>
          </div>
        </div>
        <div
          v-else
          class="card"
        >
          <div
            class="content-pass"
          >
            <h2
              >This
              Product
              is
              unavailable
              to
              show</h2
            >
            <button
              class="next-button"
              @click="nextProduct"
              >Next
              Product</button
            >
          </div>
          <img
            class="background-image"
            src="@/assets/images/sad-face1.png"
            alt="Image sad face"
          />
        </div>
      </div>
    </div>
  </div>
</template>
