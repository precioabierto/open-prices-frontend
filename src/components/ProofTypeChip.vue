<template>
  <v-chip label size="small" density="comfortable">
    <v-icon start icon="mdi-image" />
    <span v-if="proof.type === 'GDPR_REQUEST'">
      <a :href="OFF_WIKI_GDPR_REQUEST_URL" target="_blank">
        {{ proofTypeName }}
        <v-icon size="x-small" icon="mdi-open-in-new" />
      </a>
    </span>
    <span v-else>
      {{ proofTypeName }}
    </span>
  </v-chip>
</template>

<script>
import constants from '../constants'

export default {
  props: {
    proof: {
      type: Object,
      default: null
    },
  },
  data() {
    return {
      OFF_WIKI_GDPR_REQUEST_URL: constants.OFF_WIKI_GDPR_REQUEST_URL,
    }
  },
  computed: {
    proofTypeName() {
      return this.$t(`ProofCard.${this.proof.type}`)
    }
  }
}
</script>
