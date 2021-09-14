<template>
  <div class="wc-client-mrf"  @click="hideManager">
    <client-mrfs/>
  </div>
</template>

<script>
import { createModule, rootStore } from '../store'
import ClientMrfs from './segments/ClientMrfs'
import { mapInstanceState, mapInstanceMutations } from '@urbn/vuex-helpers'

const getNamespace = (comp) => comp.guid

export default {
  store: rootStore,
  name: require('../../package.json').name,
  components: {
    ClientMrfs
  },
  props: {
    guid: {
      type: String,
      default: require('../../package.json').name
    },
    instancePrefix: {
      type: String,
      default: ''
    },
    params: {
      type: Object,
      default: () => ({
        clientProfiles: []
      })
    }
  },
  provide () {
    if (!this.$guid) {
      this.$guid = this.guid
    }
    return {
      _$namespace: () => this.$guid,
      _$commit: (handler, payload) => this.$store.commit(`${this.$guid}/${handler}`, payload)
    }
  },
  watch: {
    params: {
      immediate: true,
      handler (val) {
        createModule(this.guid)
        console.log(val)
        this.setClientMrfs(val.clientProfiles)
      }
    },
    guid (newVal) {
      console.log(newVal)
      createModule(newVal)
      this.$guid = newVal
    }
  },
  data () {
    return {
      $guid: this.guid,
      showPanel: false,
      modalCoords: this.coords,
      colorS: 'red'
    }
  },
  created () {
    window.addEventListener('wheel', this.hideManager)
  },
  mounted () {
    console.log(this.clientMrfs)
  },
  destroyed () {
    window.removeEventListener('wheel', this.hideManager)
  },
  computed: {
    ...mapInstanceState(getNamespace, ['clientMrfs'])
  },
  methods: {
    ...mapInstanceMutations(getNamespace, ['setClientMrfs']),
    hideManager (e) {
      if (e.target.tagName !== 'A') {
        const openedCards = document.getElementsByClassName('manager-card')
        if (openedCards) {
          openedCards.forEach(openedCard => {
            openedCard.style.display = 'none'
          })
        }
      }
    }
  }
}
</script>

<style scoped>
  .wc-client-mrf {
    position: relative;
  }

  .user-profile {
    position: absolute;
  }
</style>
