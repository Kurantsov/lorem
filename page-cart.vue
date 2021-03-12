<template>
  <main class="page-cart">
    <div class="container">
      <div class="section_header-row">
        <h1 class="title">Корзина</h1>
      </div>
      <div class="table">
        <div class="table-title">
          <h4 class="table-title_item">{{ tableTitle.name }}</h4>
          <h4 class="table-title_item">{{ tableTitle.cost }}</h4>
          <h4 class="table-title_item">{{ tableTitle.count }}</h4>
          <h4 class="table-title_item">{{ tableTitle.sum }}</h4>
        </div>
        <div class="table-data">
          <div v-for="(item, index) in items" :key="index" class="cart-item">
            <div class="cart-item_field">
              <div class="data-item_name">
                <img :src="item.image" alt="img" class="img-container" />
                <div class="data-item_info">
                  <h4 class="data-title">{{ item.title }}</h4>
                  <h3 class="data-amount">{{ item.amount }}</h3>
                </div>
              </div>
            </div>
            <div class="cart-item_field">
              <h3 class="data-amount">{{ item.price }} руб.</h3>
            </div>
            <div class="cart-item_field">
              <div class="field-count">
                <div
                  class="field-count_decrement"
                  :id="item.slug"
                  v-on:click="decrement"
                >
                  -
                </div>
                <div class="field-count_integer">{{ item.count }}</div>
                <div
                  class="field-count_increment"
                  :id="item.slug"
                  v-on:click="increnent"
                >
                  +
                </div>
              </div>
            </div>
            <div class="cart-item_field">
              <h3 class="data-amount">{{ item.sum }}</h3>
            </div>
          </div>
        </div>
      </div>
      <div class="table-footer">
        <div class="input-promo"><BlockSendPromoCode /></div>
        <div class="order-form">
          <div class="total-sum">
            <div class="total-sum_text">Итого:</div>
            <div class="total-sun-number">{{ total }} руб.</div>
          </div>
          <nuxt-link to="/catalog" class="next-button_white">
            Продолжить покупки</nuxt-link
          >
          <nuxt-link class="next-button_black" to="/">Оформить заказ</nuxt-link>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  name: 'PageCart',
  mounted() {
    this.cart = JSON.parse(localStorage.getItem('cart'))
  },
  data() {
    return {
      tableTitle: {
        name: 'Наименование',
        cost: 'Стоимость',
        count: 'Количество',
        sum: 'Общая сумма',
      },
      cart: [],
    }
  },
  computed: {
    items() {
      const items = this.cart
      return items
    },

    total() {
      let total = 0
      if (this.cart) {
        this.cart.map((el) => {
          total += el.sum
        })
      }
      return total
    },
  },
  methods: {
    increnent(event) {
      const element = event.path[0].id
      const oldCart = JSON.parse(localStorage.getItem('cart'))
      oldCart.map((item) => {
        if (item.slug === element) {
          item.count += 1
          item.sum = item.price * item.count
        }
      })
      this.cart = oldCart
      localStorage.setItem('cart', JSON.stringify(oldCart))
    },
    decrement(event) {
      const element = event.path[0].id
      const oldCart = JSON.parse(localStorage.getItem('cart'))
      oldCart.map((item) => {
        if (item.slug === element && item.count > 0) {
          item.count -= 1
          item.sum = item.price * item.count
        }
      })
      this.cart = oldCart
      localStorage.setItem('cart', JSON.stringify(oldCart))
    },
  },
}
</script>

<style lang="scss">
.page-cart {
  width: 100%;
  display: flex;
  justify-content: center;
}
.container {
  width: 1760px;
  display: flex;
  justify-content: center;
  flex-direction: column;
}
.section_header-row {
  margin: 64px 0 0 0;
  width: 100%;
  display: flex;
  justify-content: center;
}
.title {
  padding: 0;
  margin: 0;
  font-style: normal;
  font-weight: normal;
  font-size: 46px;
  line-height: 56px;
  color: #000000;
}
.table {
  display: flex;
  flex-direction: column;
  width: 100%;
  margin: 109px 0 0 0;
}
.table-title {
  width: 100%;
  display: flex;
  border-bottom: 1px solid rgba(0, 0, 0, 0.5);
}
.table-title_item {
  display: flex;
  flex: 1;
  white-space: nowrap;
  justify-content: center;
  text-transform: uppercase;
  font-style: normal;
  font-weight: normal;
  font-size: 19px;
  line-height: 23px;
  letter-spacing: 0.05em;
  padding-bottom: 14px;
}
.img-container {
  width: 100px;
  height: 100px;
  object-fit: cover;
  left: 0;
}
.data-title {
  font-family: 'Proxima Nova';
  font-style: normal;
  font-weight: normal;
  font-size: 19px;
  line-height: 23px;
  letter-spacing: 0.05em;
}
.data-amount {
  font-family: 'Proxima Nova';
  font-style: normal;
  font-weight: 300;
  font-size: 18px;
  line-height: 22px;
  color: #000000;
}
.data-item_name {
  display: flex;
  position: relative;
  width: 230px;
}
.data-item_info {
  display: flex;
  flex-direction: column;
  justify-content: center;
  margin: 0 0 0 27px;
}
.cart-item {
  display: flex;
  width: 100%;
  align-items: center;
  padding: 30px 0;
  border-bottom: 1px solid rgba(187, 187, 187, 0.5);
}
.cart-item_field {
  display: flex;
  flex: 1;
  justify-content: center;
}
.field-count {
  display: flex;
  width: 118px;
  height: 42px;
  border: 1px solid black;
}
.field-count_decrement,
.field-count_integer,
.field-count_increment {
  display: flex;
  flex: 1;
  font-family: 'Proxima Nova';
  font-style: normal;
  font-weight: normal;
  font-size: 20px;
  line-height: 24px;
  letter-spacing: 0.02em;
  justify-content: center;
  align-items: center;
}
.field-count_decrement,
.field-count_increment {
  cursor: pointer;
}
.field-count_decrement:hover,
.field-count_increment:hover {
  color: white;
  background-color: black;
}
.input-promo {
  width: 300px;
}
.table-footer {
  display: flex;
  margin: 30px 0 0 0;
  justify-content: space-between;
}
.order-form {
  width: 445px;
  display: flex;
  flex-direction: column;
}
.total-sum {
  display: flex;
  justify-content: space-between;
  font-family: 'Proxima Nova';
  font-style: normal;
  font-weight: normal;
  font-size: 28px;
  line-height: 34px;
  color: #000000;
}
.next-button_white {
  justify-content: center;
  align-items: center;
  text-align: center;
  outline: none;
  background-color: white;
  border: 1px solid black;
  font-family: 'Proxima Nova';
  font-style: normal;
  font-weight: 300;
  font-size: 14px;
  line-height: 17px;
  letter-spacing: 0.05em;
  color: #000000;
  border-radius: 0;
  padding: 17px;
  text-transform: uppercase;
  margin: 24px 0 0 0;
  cursor: pointer;
}
.next-button_black {
  justify-content: center;
  align-items: center;
  text-align: center;
  outline: none;
  background-color: black;
  border: 1px solid black;
  font-family: 'Proxima Nova';
  font-style: normal;
  font-weight: 300;
  font-size: 14px;
  line-height: 17px;
  letter-spacing: 0.05em;
  color: white;
  border-radius: 0;
  padding: 17px;
  text-transform: uppercase;
  margin: 24px 0 0 0;
  cursor: pointer;
}
</style>
