<template>
  <div class="client-mrf-wrapper">
    <crm-ui-panel class="crm-panel-wrapper">
      <mrf-title
      class="mrf-title"
      :mrf="mrf"
      :clientOut="clientOut"
      />
      <div :class="[mrfsCount >= 2 ? block : '', rightBlock]">
        <mrf-manager
          :personalManager="mrf.personalManager"
          v-if="segments"
          class="mrf-manager"
        />
        <mrf-organization
          :organizationName="mrf.organizationName"
          v-if="mrf.segment === 'VIP_ЛПР_ФЛ'"
          class="mrf-org"
        />
      </div>
    </crm-ui-panel>
  </div>
</template>
<script>
import { mapInstanceState } from '@urbn/vuex-helpers'
import MrfTitle from '../molecules/MrfTitle'
import MrfManager from '../molecules/MrfManager'
import MrfOrganization from '../molecules/MrfOrganization'

export default {
  inject: ['_$namespace'],
  components: {
    MrfTitle,
    MrfManager,
    MrfOrganization
  },
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
      block: 'block',
      rightBlock: 'right-block'
    }
  },
  computed: {
    ...mapInstanceState((c) => c._$namespace(), ['mrfsCount']),
    segments () {
      if (this.mrf.segment === 'VIP_ЛПР_ФЛ' || this.mrf.segment === 'VIP_ЛФОМ_ФЛ') {
        return true
      } else {
        return false
      }
    }
  },
  mounted () {
    console.log(this.mrf, 'this is mrf')
  }
}
</script>
<style scoped>

  .manager-card {
    position: absolute;
    z-index: 111;
  }

  .client-name-wrapper {
    position: relative;
  }

  .right-block {
    display: flex;
    justify-content: space-around;
    flex-grow: 24;
  }

  .block {
    display: block !important;
  }

  .crm-panel-wrapper {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    height: 100%;
    padding: 12px !important;
    overflow: unset !important;
  }

  .mrf-title {
    margin-right: 12px;
    flex-grow: 1;
  }
</style>
