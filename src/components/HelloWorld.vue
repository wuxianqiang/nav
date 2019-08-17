<template>
  <div class="nav-wrap">
      <ul
      ref="nav"
      class="nav">
      <li
        v-for="item in list"
        :key="item.id"
        class="nav-item">
        <span
          class="nav-text"
          @click="handleClick(item.id)"
          :class="{active: currentID===item.id}">
          {{ item.name }}
        </span>
      </li>
    </ul>
  </div>
</template>

<script>
  export default {
    props: {
      list: {
        type: Array,
        default: () => ([])
      },
      currentID: {
        type: Number,
        default: 0
      }
    },

    data () {
      return {
        innerWidth: window.innerWidth
      }
    },

    watch: {
      currentID: {
        immediate: true,
        handler (value) {
          const index = this.list.findIndex(item => item.id === value)
          this.$nextTick(() => {
            if (index < 0) {
              this.$refs.nav.scrollLeft = 0
              this.$emit('reset')
            } else {
              const maxWidth = this.$refs.nav.scrollWidth - this.innerWidth
              const left = this.$refs.nav.scrollLeft
              const el = this.$refs.nav.children[index]
              const offsetLeft = el.offsetLeft
              const width = el.offsetWidth / 2
              const center = this.innerWidth / 2
              let distance = offsetLeft - left
              if (distance < center) {
                let value = Math.max(left - (center - distance) + width, 0)
                this.move(value)
              } else {
                let value = Math.min(left + (distance - center) + width, maxWidth)
                this.move(value)
              }
            }
          })
        }
      }
    },

    mounted () {
      window.addEventListener('resize', () => {
        this.innerWidth = window.innerWidth
      })
    },

    methods: {
      handleClick (id) {
        this.$emit('selectItem', id)
      },
      move (distance) {
        let target = distance
        let source = this.$refs.nav.scrollLeft
        if (target > source) {
          let step = (target - source) / 200 * 20
          const move = () => {
            let currentScrollLeft = this.$refs.nav.scrollLeft
            currentScrollLeft += step
            if (currentScrollLeft >= target) {
              currentScrollLeft = target;
              this.$refs.nav.scrollLeft = currentScrollLeft
            } else {
              this.$refs.nav.scrollLeft = currentScrollLeft
              requestAnimationFrame(move)
            }
          }
          requestAnimationFrame(move)
        } else {
          let step = (source - target) / 200 * 20
          const move = () => {
            let currentScrollLeft = this.$refs.nav.scrollLeft
            currentScrollLeft -= step
            if (currentScrollLeft <= target) {
              currentScrollLeft = target;
              this.$refs.nav.scrollLeft = currentScrollLeft
            } else {
              this.$refs.nav.scrollLeft = currentScrollLeft
              requestAnimationFrame(move)
            }
          }
          requestAnimationFrame(move)
        }
      }
    }
  }
</script>

<style scoped>
.nav-wrap {
  position: relative;
  overflow: hidden;
}
.nav {
  overflow-x: auto;
  white-space: nowrap;
  z-index: 2;
}
.container {
  position: relative;

}
.nav-text {
  line-height: 32px;
  display: inline-block;
  border-bottom: 2PX solid transparent;
}
.active {
  border-bottom: 2PX solid red;
  color: red;
}
.nav-item {
  display: inline-block;
  padding: 0 20px;
}
.line {
  width: 0;
  height: 2px;
  background: red;
  position: absolute;
  left: 0;
  bottom: 0;
}
</style>
