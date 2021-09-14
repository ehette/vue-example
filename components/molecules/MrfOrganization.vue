
<template>
  <div v-if="organizationName" class="client-organization-wrapper">
    <p class="crm-ui-text-p p-description org-title">
      Компания
    </p>
    <span
      class="crm-ui-text-p org-name">
        {{ organizationName }}
      </span>
  </div>
</template>

<script>

export default {
  inject: ['_$namespace'],
  props: {
    organizationName: {
      type: String,
      default: ''
    }
  }
}
</script>

<style scoped>
  .client-organization-wrapper {
    display: block;
  }

  .org-title {
    font-size: 12px;
    line-height: 16px;
    margin: 0;
  }

  .org-name {
    line-height: 18px;
  }
</style>
