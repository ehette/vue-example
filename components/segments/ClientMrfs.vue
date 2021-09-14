<template>
  <div class="client-mrfs-wrapper">
    <client-mrf
      class="mrf-item"
      v-for="mrf in clientMrfs"
      :key="mrf.id"
      :mrf="mrf"
      :clientOut="mrf.isOutflow"
    />
  </div>
</template>
<script>
import { mapInstanceState } from '@urbn/vuex-helpers'
import ClientMrf from '../organisms/ClientMrf'

export default {
  inject: ['_$namespace'],
  components: {
    ClientMrf
  },
  computed: {
    ...mapInstanceState((c) => c._$namespace(), ['clientMrfs'])
  }
}
</script>
<style lang="scss" scoped>
  .client-mrfs-wrapper {
    position: relative;
    display: flex;
    flex-wrap: wrap;
    width: 100%;
    align-items: stretch;
  }

  .client-mrfs-wrapper > *:not(:nth-child(4n)) {
    margin-right: 12px;
  }

  .mrf-item {
    position: relative;
    flex-grow: 1;
    flex-shrink: 1;
    min-width: 0;
    width: calc(25% - 12px);
    margin-bottom: 12px;

    &:last-child {
      margin-right: 0;
    }
  }
</style>
