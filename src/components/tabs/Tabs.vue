<template>
  <ul :class="navClasses">
    <li v-for="(index, items) in data" :class="{'active': active == index, 'disabled': items.disabled, 'drpodown': items.showdrop}"
      @click="_toggleTab(index, items.disabled || false)">
      <a href="javascript:void(0)" v-if="items.disabled">{{items.value}}</a>
      <a v-else v-link="{path: items.link}" :class="{'dropdown-toggle': items.showdrop}" @click="_showdrop(items.showdrop)" v-on:blur="_closedrop()">
        {{items.value}}
        <span class="caret" v-if="items.showdrop"></span>
      </a>
      <ul class="dropdown-menu" v-if="items.showdrop && visible" transition="slide">
        <li v-for="item in items.dropdown" :class="{'disabled': item.disabled}">
          <a href="javascript:void(0)" v-if="item.disabled">{{items.value}}</a>
          <a @click.prevent="item.onclick" @click="_closedrop(items.showdrop)" v-if="item.onclick">{{item.value}}</a>
          <a v-if="!item.disabled && !item.onclick" v-link="{path: item.link}">{{item.value}}</a>
        </li>
      </ul>
    </li>
  </ul>
</template>
<script>
  import { defaultProps, oneOf } from '../../views/utils/props'
  import cx from 'classnames'
  export default {
    props: defaultProps({
      prefixCls: 'nav',
      type: oneOf(['tabs', 'pills']),
      stacked: false,
      justified: false,
      data: [],
      visible: false,
      active: 0
    }),
    ready () {
      window.addEventListener('click', this.close)
    },
    beforeDestroy() {
      window.removeEventListener('click', this.close)
    },
    computed: {
      navClasses () {
        return cx({
          [this.prefixCls]: 1,
          [`${this.prefixCls}-${this.type}`]: this.type,
          [`${this.prefixCls}-stacked`]: this.stacked,
          [`${this.prefixCls}-justified`]: this.justified
        })
      }
    },
    methods: {
      _showdrop (val) {
        if (val) {
          this.visible = !this.visible
        }
      },
      _closedrop () {
        this.visible = false
      },
      close(e) {
        var self = this
        if (!self.$el.contains(e.target)) {
          self._closedrop()
        }
      },
      _toggleTab (index, val) {
        var self = this
        if (val) {
          return
        } else {
          self._closedrop()
          self.active = index
        }
      }
    }
  }

</script>
