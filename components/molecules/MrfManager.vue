
<template>
  <div
    class="client-manager-wrapper"
  >
    <p class="crm-ui-text-p p-description manager-title">
      Персональный Менеджер
    </p>
    <a
      @click="showModal"
      class="crm-ui-text-p link"
      >
        {{ personalManager.name }}
    </a>
    <wc-user-profile
      class="manager-card"
      :params.prop="{loginAmdocs: currentManager}"
      @click.stop
    />
  </div>
</template>

<script>
import { mapInstanceMutations, mapInstanceState } from '@urbn/vuex-helpers'
export default {
  inject: ['_$namespace'],
  props: {
    personalManager: {
      type: Object,
      default: () => ({})
    }
  },
  computed: {
    ...mapInstanceState(c => c._$namespace(), ['currentManager'])
  },
  methods: {
    ...mapInstanceMutations(c => c._$namespace(), ['showUser']),
    showModal () {
      const openedCards = document.getElementsByClassName('manager-card')
      const openedCardsArray = [...openedCards]
      const windowWidth = window.innerWidth
      console.log(openedCards)
      if (openedCards) {
        openedCards.forEach(openedCard => {
          if (openedCard.style.display === 'block') {
            openedCard.style.display = 'none'
          }
        })
      }
      this.showUser({ login: this.personalManager.login })
      this.$el.children[2].style.display = 'block'
      openedCardsArray.map((card, index) => {
        openedCards[index].style.right = 'unset'
        if (windowWidth < card.getBoundingClientRect().right) {
          openedCards[index].style.position = 'fixed'
          openedCards[index].style.right = '16px'
        }
      })
    }
  },
  watch: {
    personalManager: {
      immediate: true,
      handler (val) {
        this.showUser({ login: this.personalManager.login })
      }
    }
  }
}
</script>

<style scoped>
  .client-manager-wrapper {
    margin-bottom: 8px;
  }

  .link {
    line-height: 18px;
    font-size: 14px;
    text-decoration: none;
    color: #2f9aff;
  }

  .crm-ui-text-p {
    cursor: pointer;
  }

  .manager-card {
    display: none;
    position: absolute;
    z-index: 999;
    width: 800px;
  }

  .manager-title {
    font-size: 12px;
    line-height: 16px;
    margin: 0 0 2px 0;
  }
</style>
