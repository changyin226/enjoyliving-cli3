<template>
  <div class="home-page">
    <div class="banner">
      <p>
        享住Enjoy Living
        <br>
        您最好的家具選擇
        <br>
        <router-link
          class="btn"
          to="/productlist/all/1"
        >
          立即選購產品
        </router-link>
      </p>
    </div>
    <div class="services pt-5">
      <div class="container py-5">
        <div class="row">
          <div class="col-md-4">
            <div class="feature">
              <span class="icon">
                <i class="fa fa-credit-card fa-3x" />
              </span>
              <h3>信用卡</h3>
              <p>
                本店所有商品皆可使用信用卡付款。
              </p>
            </div>
          </div>
          <div class="col-md-4">
            <div class="feature">
              <span class="icon">
                <i class="fa fa-truck fa-3x" />
              </span>
              <h3>免運費</h3>
              <p>
                本店所有商品凡是購買即可享有免運費服務，為您節省荷包負擔。
              </p>
            </div>
          </div>
          <div class="col-md-4">
            <div class="feature">
              <span class="icon">
                <i class="fa fa-calendar-o fa-3x" />
              </span>
              <h3>一年保固</h3>
              <p>
                本店所有商品皆享有一年免費保固，若為明顯人為惡意破壞，店家會斟酌收取額外費用。
              </p>
            </div>
          </div>
        </div>
      </div>
      <div
        v-waypoint="{ active: true, callback: onWaypoint }"
        class="coupon container mt-5 p-0 animate__animated"
      >
        <p>
          慶祝開店滿一週年
          <br>
          即刻起至8月31號
          <br>
          所有商品打９折
          <br>
          優惠碼請輸入
          <br>
          <span>happy1314</span>
        </p>
      </div>
    </div>
    <div class="container py-5">
      <div class="row justify-content-center py-5">
        <div class="col-md-8">
          <div
            class="section-heading"
          >
            <span>Enjoy Living</span>
            <h2>熱銷產品</h2>
            <p>我們提供價格實惠的產品，但這並不代表我們會犧牲產品的品質或安全。我們的產品皆重視產品安全，並對健康與環境友善。</p>
          </div>
        </div>
      </div>
      <div class="row py-5">
        <div
          v-for="product in firstSixProducts"
          :key="product.id"
          v-waypoint="{ active: true, callback: onWaypointList }"
          class="col-md-4 animate__animated"
        >
          <ProductCard
            :product="product"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ProductCard from '@/components/front/ProductCard.vue';

export default {
  components: {
    ProductCard,
  },
  data() {
    return {
      products: [],
    };
  },
  computed: {
    firstSixProducts() {
      return this.products.filter((index, i) => i < 6);
    },
  },
  mounted() {
    this.getProducts();
  },
  activated() {
    const animate = document.querySelectorAll('.animate__animated');
    animate.forEach((el) => {
      el.classList.remove('animate__fadeInUp');
    });
    this.$bus.$emit('hideOffCanvas');
  },
  methods: {
    getProducts() {
      this.$bus.$emit('update:loading', true);
      const api = `${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/products/all`;
      this.$http.get(api).then((response) => {
        if (response.data.success) {
          this.products = response.data.products.filter((product) => product.is_enabled);
          this.$bus.$emit('update:loading', false);
        } else {
          this.$bus.$emit('update:loading', false);
          this.$bus.$emit('message:push', response.data.messages);
        }
      });
    },
    onWaypoint({ el, going }) {
      if (going === this.$waypointMap.GOING_IN) {
        el.classList.add('animate__fadeInUp');
      }
    },
    onWaypointList({ el, going }) {
      if (going === this.$waypointMap.GOING_IN) {
        const animate = document.querySelectorAll('.col-md-4.animate__animated');
        const arr = Array.from(animate);
        const index = arr.indexOf(el);
        setTimeout(() => {
          el.classList.add('animate__fadeInUp');
        }, index * 200);
      }
    },
  },
};
</script>

<style lang="scss">
.home-page{
  .banner{
    background: url(../../assets/images/HomeBanner.jpg) center;
    background-size: cover;
    display: flex;
    justify-content: center;
    align-items: center;
    p{
      font-weight: bold;
      letter-spacing: 5px;
      color: #00d2ff;
      font-size: 24px;
      text-align: center;
      margin-bottom: 0;
      width: 100%;
      max-width: 600px;
      padding: 50px 0;
      background: rgba(0, 0, 0, 0.4);
    }
    a{
      font-weight: bold;
      margin-top: 30px;
    }
  }
  .services{
    .feature {
      text-align: center;
      .icon {
        display: flex;
        width: 90px;
        height: 90px;
        justify-content: center;
        align-items: center;
        color: #fff;
        background-image: linear-gradient(135deg, #00d2ff 0%, #00cdac 100%);
        margin: 0 auto 20px auto;
        border-radius: 50%;
      }
      h3{
        margin-bottom: 20px;
      }
    }
  }
  .coupon{
    height: 400px;
    background: url('../../assets/images/Coupon.jpg') center;
    background-size: cover;
    display: flex;
    justify-content: center;
    align-items: center;
    letter-spacing: 5px;
    p{
      font-size: 24px;
      font-weight: bold;
      color: #fff;
      text-align: center;
      width: 100%;
      padding: 50px 0;
      margin-bottom: 0;
      background: rgba(0, 0, 0, 0.25);
    }
    span{
      display: inline-block;
      padding: 5px;
      background: rgba(255, 255, 255, 0.7);
      color: rgb(255, 60, 60);
    }
  }
}
@media(max-width: 767px){
  .services{
    .col-md-4:not(.col-md-4:last-child){
      margin-bottom: 80px;
    }
  }
}
</style>
