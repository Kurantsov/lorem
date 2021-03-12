<template>
  <component :is="page.code" :page="page"></component>
</template>

<script>
import FormatHeadMixin from '@/components/mixins/format-head'

import PageHome from '@/components/pages/page-home.vue'
import PageFaq from '@/components/pages/page-faq.vue'
import PageCatalog from '@/components/pages/page-catalog.vue'
import PageCart from '@/components/pages/page-cart.vue'
import PageOrder from '@/components/pages/page-order.vue'
import PageBuy from '@/components/pages/page-buy.vue'
import PageBlog from '@/components/pages/page-blog.vue'
import PageAbout from '@/components/pages/page-about.vue'
import PagePrivacy from '@/components/pages/page-privacy.vue'
import PageCustom from '@/components/pages/page-custom.vue'
import PageContacts from '@/components/pages/page-contacts.vue'
export default {
  name: 'PageContainer',
  components: {
    PageHome,
    PageFaq,
    PageCatalog,
    PageOrder,
    PageCart,
    PageBuy,
    PageBlog,
    PageAbout,
    PagePrivacy,
    PageCustom,
    PageContacts,
  },
  mixins: [FormatHeadMixin],
  async asyncData({ store, params, error }) {
    const slug = params.page ? params.page : 'home'
    const pageCode =
      slug === 'cart'
        ? 'page-cart'
        : slug === 'order'
        ? 'page-order'
        : store.state.globalData.pageSlug[slug]
    if (!pageCode) {
      error({ statusCode: 404, message: 'Страница не найдена' })
      return
    }

    const query =
      pageCode !== 'page-custom' ? pageCode.split('-').pop() : 'custom/' + slug
    const page = await store.dispatch('getPage', query)

    if (store.state.error) {
      error({ statusCode: 404, message: 'Страница не найдена' })
      return
    }

    return { page: { ...page, code: pageCode } }
  },
  head() {
    return this.generateHead({ localHead: this.page.meta })
  },
}
</script>

<style module lang="scss"></style>
