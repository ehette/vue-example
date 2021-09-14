<template>
  <div class="client-mrf-title">
    <h3 class="crm-ui-text-h1 mrf-name">{{ mrf.mrfName }}</h3>
    <div class="hint"
      @mouseover="shownHint = true"
      @mouseleave="shownHint = false"
    >
      <div class="mrf-hint" v-if="shownHint">
        <p class="crm-ui-text-p mrf-hint-text">{{ segmentHint }}</p>
      </div>
      <crm-ui-icon
        class="segment-icon"
        :src="currentSVG()"
        size="24"
      />
      <crm-ui-icon
        class="text-icon lpr"
        :src="lprSVG"
        v-if="isLpr"
      />
      <crm-ui-icon
        class="text-icon lform"
        :src="lfomSVG"
        v-if="isLfom"
      />
      <crm-ui-icon
        class="text-icon"
        :src="newStandart"
        v-if="isStandartNew"
      />
      <crm-ui-icon
        class="text-icon"
        :src="newSilver"
        v-if="isSilverNew"
      />
      <crm-ui-icon
        class="text-icon"
        :src="newGold"
        v-if="isGoldNew"
      />
      <crm-ui-icon
        class="text-icon"
        :src="newPlatinum"
        v-if="isPlatinumNew"
      />
    </div>
    <div
      class="hint"
      @mouseover="shownOut = true"
      @mouseleave="shownOut = false"
    >
      <div class="mrf-hint out" v-if="shownOut">
        <p class="crm-ui-text-p mrf-hint-text">Клиент склонен к оттоку</p>
      </div>
      <crm-ui-icon
        :src="customOut"
        v-if="clientOut"
      />
    </div>
  </div>
</template>
<script>
import { mapInstanceState } from '@urbn/vuex-helpers'
import vipSVG from '@/assets/icons/segments/vip.svg?inline'
import undefinedStandartSVG from '@/assets/icons/segments/undefined-standart.svg?inline'
import undefinedSilvertSVG from '@/assets/icons/segments/undefined-silver.svg?inline'
import undefinedGoldSVG from '@/assets/icons/segments/undefined-gold.svg?inline'
import undefinedPlatinumSVG from '@/assets/icons/segments/undefined-platinum.svg?inline'
import economStandartSVG from '@/assets/icons/segments/econom-standart.svg?inline'
import economSilvertSVG from '@/assets/icons/segments/econom-silver.svg?inline'
import economGoldSVG from '@/assets/icons/segments/econom-gold.svg?inline'
import economPlatinumSVG from '@/assets/icons/segments/econom-platinum.svg?inline'
import premiumStandartSVG from '@/assets/icons/segments/premium-standart.svg?inline'
import premiumSilvertSVG from '@/assets/icons/segments/premium-silver.svg?inline'
import premiumGoldSVG from '@/assets/icons/segments/premium-gold.svg?inline'
import premiumPlatinumSVG from '@/assets/icons/segments/premium-platinum.svg?inline'
import newStandart from '@/assets/icons/new-standart.svg?inline'
import newSilver from '@/assets/icons/new-silver.svg?inline'
import newGold from '@/assets/icons/new-gold.svg?inline'
import newPlatinum from '@/assets/icons/new-platinum.svg?inline'
import lprSVG from '@/assets/icons/lpr.svg?inline'
import lfomSVG from '@/assets/icons/lfom.svg?inline'
import customOut from '@/assets/icons/custom-out.svg?inline'
export default {
  inject: ['_$namespace'],
  props: {
    mrf: {
      type: Object,
      default: () => ({})
    },
    clientOut: {
      type: Boolean,
      default: false
    }
  },
  data () {
    return {
      shownHint: false,
      shownOut: false,
      segmentLink: '',
      vipSVG,
      customOut,
      undefinedStandartSVG,
      undefinedSilvertSVG,
      undefinedGoldSVG,
      undefinedPlatinumSVG,
      economStandartSVG,
      economSilvertSVG,
      economGoldSVG,
      economPlatinumSVG,
      premiumStandartSVG,
      premiumSilvertSVG,
      premiumGoldSVG,
      premiumPlatinumSVG,
      newStandart,
      newSilver,
      newPlatinum,
      newGold,
      lprSVG,
      lfomSVG,
      isLpr: false,
      isLfom: false,
      isStandartNew: false,
      isSilverNew: false,
      isGoldNew: false,
      isPlatinumNew: false
    }
  },
  computed: {
    ...mapInstanceState((c) => c._$namespace(), ['clientMrfs']),
    segmentHint () {
      return `Сегмент клиента: ${this.mrf.segment}. ${this.mrf.subSegment === 'Не определено' ? '' : this.mrf.subSegment}`
    }
  },
  methods: {
    showHint () {
      console.log(this.shownHint)
      this.shownHint = true
    },
    currentSVG () {
      const subSegment = this.mrf.subSegment
      if (this.mrf.segment === 'VIP_ЛПР_ФЛ') {
        this.isLpr = true
        return this.vipSVG
      } else if (this.mrf.segment === 'VIP_ЛФОМ_ФЛ') {
        this.isLfom = true
        return this.vipSVG
      } else if (this.mrf.segment === 'Не определено') {
        switch (subSegment) {
        case 'Не определено':
          return this.undefinedStandartSVG
        case 'Стандарт':
          return this.undefinedStandartSVG
        case 'Новичок':
          this.isStandartNew = true
          return this.undefinedStandartSVG
        case 'Золото':
          return this.undefinedGoldSVG
        case 'Золото-Новичок':
          this.isGoldNew = true
          return this.undefinedGoldSVG
        case 'Серебро':
          return this.undefinedSilvertSVG
        case 'Серебро-Новичок':
          this.isSilverNew = true
          return this.undefinedSilvertSVG
        case 'Платина':
          return this.undefinedPlatinumSVG
        case 'Платина-Новичок':
          this.isPlatinumNew = true
          return this.undefinedPlatinumSVG
        default:
          return ''
        }
      } else if (this.mrf.segment === 'Эконом') {
        switch (subSegment) {
        case 'Не определено':
          return this.economStandartSVG
        case 'Стандарт':
          return this.economdStandartSVG
        case 'Новичок':
          this.isStandartNew = true
          return this.economStandartSVG
        case 'Золото':
          return this.economGoldSVG
        case 'Золото-Новичок':
          this.isGoldNew = true
          return this.economGoldSVG
        case 'Серебро':
          return this.economSilvertSVG
        case 'Серебро-Новичок':
          this.isSilverNew = true
          return this.economSilvertSVG
        case 'Платина':
          return this.economPlatinumSVG
        case 'Платина-Новичок':
          this.isPlatinumNew = true
          return this.economPlatinumSVG
        default:
          return ''
        }
      } else if (this.mrf.segment === 'Премиум') {
        switch (subSegment) {
        case 'Не определено':
          return this.premiumStandartSVG
        case 'Стандарт':
          return this.premiumStandartSVG
        case 'Новичок':
          this.isStandartNew = true
          return this.premiumStandartSVG
        case 'Золото':
          return this.premiumGoldSVG
        case 'Золото-Новичок':
          this.isGoldNew = true
          return this.premiumGoldSVG
        case 'Серебро':
          return this.premiumSilvertSVG
        case 'Серебро-Новичок':
          this.isSilverNew = true
          return this.premiumSilvertSVG
        case 'Платина':
          return this.premiumPlatinumSVG
        case 'Платина-Новичок':
          this.isPlatinumNew = true
          return this.premiumPlatinumSVG
        default:
          return ''
        }
      }
    }
  }
}
</script>
<style scoped lang="scss">
  .mrf-name {
    font-size: 14px;
    line-height: 20px;
    font-weight: 700;
  }

  .lform {
    width: 33px;
    display: inline !important;
  }

  .lpr {
    display: inline !important;
  }

  .segment-icon {
    position: relative;
    right: 3px;
  }

  .hint {
    display: inline-block !important;
    position: relative;
  }

  .mrf-hint {
    position: absolute;
    width: max-content;
    top: -48px;
    z-index: 999;
    padding: 0.5rem 0.75rem;
    background: rgb(135, 139, 147);
    border-radius: 8px;
    animation: mounted 250ms ease-in-out;

    &-text {
      color: #fff;
      padding: 0;
      margin: 0;

      &:not(:last-of-type) {
        margin-bottom: 0.5rem;
      }
    }

    &::after {
      display: block;
      content: '';
      position: absolute;
      width: 16px;
      height: 16px;
      background: rgb(135, 139, 147);
      transform: rotateZ(45deg);
      left: calc(50% - 47%);
    }
  }

  .out {
    &::after {
      left: calc(50% - 85px);
    }
  }

  @keyframes mounted {
    0% {
      opacity: 0;
    }

    100% {
      opacity: 1;
    }
  }
</style>
