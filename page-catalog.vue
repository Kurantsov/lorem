<template>
  <main>
    <transition name="fade">
      <div v-if="!$route.params.item">
        <div
          v-if="banner"
          :class="$style.banner"
          :style="{ 'background-image': 'url(' + banner + ')' }"
        ></div>

        <div :class="$style.limiter">
          <BlockFilterMenu v-bind="{ filters }" />

          <ul v-if="items && items.length" :class="$style.list">
            <li
              v-for="(item, index) in items"
              :id="slugify(item.slug)"
              :key="index"
              :class="$style.item"
            >
              <BlockProductCard :data="item" />
              <div :class="$style.buttonField">
                <button
                  :class="$style.appendCart"
                  v-on:click="addProductToCart(item)"
                >
                  Добавить в корзину
                </button>
              </div>
            </li>
          </ul>
        </div>

        <BlockSuggestion v-bind="note" />
      </div>
    </transition>
    <nuxt-child
      v-if="$route.params.item"
      :page-type="'page-product'"
    ></nuxt-child>
  </main>
</template>

<script>
export default {
  name: 'PageCatalog',

  props: {
    page: {
      type: Object,
      default: () => ({}),
    },
  },

  data() {
    return {
      content: {},
    }
  },

  computed: {
    banner() {
      return this.page.banner
    },

    filters() {
      return this.page.filters || []
    },

    items() {
      const id = this.$route.query.filter
      let items = this.page.items || []

      if (this.filters.length && id)
        if (id !== 'all') {
          items = items.filter((item) =>
            (item.props || []).some((el) => String(el) === id)
          )
        }

      return items
    },

    note() {
      return this.page.note
    },
  },

  methods: {
    slugify(string) {
      return string
        .toLowerCase()
        .replace(/\s+/g, '-')
        .replace(/[^\w-]+/g, '')
    },
    addProductToCart(item) {
      item.count = 0
      item.sum = 0
      const cart = localStorage.getItem('cart')
      if (cart) {
        const oldStorage = JSON.parse(localStorage.getItem('cart'))
        const el = oldStorage.find((x) => x.slug === item.slug)
        if (el) {
        } else {
          const newStorage = oldStorage.concat([item])
          localStorage.setItem('cart', JSON.stringify(newStorage))
        }
      } else {
        localStorage.setItem('cart', JSON.stringify([item]))
      }
    },
  },
}
</script>

<style module lang="scss">
.limiter {
  margin-bottom: 80px;

  padding-left: var(--page-offset);
  padding-right: calc(var(--page-offset) - var(--scroll-width));

  &:last-child {
    margin-bottom: 0;
  }

  @include if-wider($m) {
    margin-bottom: 90px;
  }

  @include if-wider($l) {
    margin-bottom: 110px;
  }

  @include if-wider($xl) {
    margin-bottom: 120px;
  }

  @include if-wider($xxl) {
    margin-bottom: 190px;
  }
}

.banner {
  height: 152px;

  background: #ddd;

  background-size: cover;
  background-position: 50%;
  background-repeat: no-repeat;

  @include if-wider($s) {
    height: 180px;
  }

  @include if-wider($sm) {
    height: 426px;
  }

  @include if-wider($xl) {
    height: 460px;
  }

  @include if-wider($xxl) {
    height: 622px;
  }
}

.list {
  display: grid;
  gap: 15px;

  margin: 30px 0 0;
  padding: 0;

  list-style: none;

  @include if-wider($sm) {
    grid-template-columns: repeat(2, 1fr);
    gap: 20px;

    margin-top: 0;
  }

  @include if-wider($m) {
    grid-template-columns: repeat(3, 1fr);
    margin-top: 20px;
  }

  @include if-wider($l) {
    gap: 30px;
  }

  @include if-wider($xxl) {
    margin-top: 32px;
  }
}

.item {
  display: flex;
  flex-direction: column;
}
.buttonField {
  display: flex;
  width: 100%;
  background-color: #eeeeee;
  justify-content: center;
  align-items: center;
  padding: 0 0 30px 0;
}
.appendCart {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 80%;
  outline: none;
  border-radius: 0;
  border: 1px solid black;
  padding: 5% 30%;
  font-family: 'Proxima Nova';
  font-style: normal;
  font-weight: 300;
  font-size: 14px;
  line-height: 17px;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  color: #000000;
  cursor: pointer;
  white-space: nowrap;
}
</style>
