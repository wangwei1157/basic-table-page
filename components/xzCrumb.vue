<template>
  <Crumb :title="'催单回复'">
    <a-config-provider :auto-insert-space-in-button="false">
    </a-config-provider>
  </Crumb>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { Crumb } from 'xiezan-ui';

export default defineComponent({
  name: 'XzCrumb',
  components: {
    Crumb,
  },
});
</script>

<style></style>
