<template>
  <form
    :class="$style.el"
    :action="formInfo.action"
    method="GET"
    @submit.prevent="submit"
  >
    <div :class="$style.controls">
      <label>
        <input
          v-model="form"
          requred
          :class="$style.input"
          type="text"
          name="subsribe"
          placeholder="ВВЕСти промокод"
        />
      </label>
      <button
        :class="[$style.button, { [$style.visible]: form.length }]"
        type="submit"
      >
        <SvgArrow />
      </button>
    </div>
  </form>
</template>

<script>
export default {
  name: 'BlockSendPromoCode',
  data() {
    return {
      form: '',
    }
  },
  computed: {
    formInfo() {
      return this.$store.state.globalData.form || []
    },
  },
  methods: {
    async submit() {
      const data = await this.$axios.$get(
        'https://elemax.life/api/cart/promocode?promocode=NEWYEAR'
      )
      console.log(await data)
      // this.$axios.get(this.formInfo.action, this.form).then((res) => {
      //   console.log(res)
      //   this.form = ''
      // })
    },
  },
}
</script>

<style module lang="scss">
.el {
  padding-right: var(--page-offset);
  margin-bottom: 60px;

  @include if-wider($sm) {
    width: 56%;
    padding-right: 0;
  }

  @include if-wider($m) {
    width: 41%;
  }

  @include if-wider($l) {
    width: 100%;
    margin-bottom: 130px;
  }
}
.controls {
  position: relative;
}
.input {
  &::placeholder {
    color: $black;
    text-transform: uppercase;
    opacity: 1;
  }
  outline: none;
  width: 100%;
  border: none;
  border-radius: 0;
  border-bottom: 1px solid $light-black;
  padding-bottom: 10px;
  margin-bottom: 10px;
  padding-right: 30px;
}

.button {
  position: absolute;
  opacity: 0;
  top: 0;
  right: 0;
  background: transparent;
  border: none;
  padding: 0;

  svg {
    width: 24px;
    height: auto;

    &:hover {
      fill: $light-black;
    }
  }
}

.visible {
  opacity: 1;
  cursor: pointer;
}

.text {
  color: $light-black;
}
</style>
