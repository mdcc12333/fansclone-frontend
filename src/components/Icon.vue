// components/Icon.vue

<script>
export default {
  props: {
    name: {
      type: String,
      required: true,
    },
  },

  render(createElement) {
    try {
      const component = require(`~/assets/icons/${this.name}.svg?inline`)

      const data = {
        class: ['icon'],
      }
      const children = [createElement(component)]

      return createElement('span', data, children)
    } catch(e) {
      // console.log(this.name, e)

      return null
    }
  },
}
</script>

<style lang="scss">
.icon {
  display: inline-block;

  > svg {
    width: 1em;
    height: 1em;
    margin-top: -4px;
    fill: currentColor;
  }
}
</style>
