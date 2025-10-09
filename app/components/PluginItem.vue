<script setup lang="ts">
import type { FlatConfigItem } from '~~/shared/types'
import { useRouter } from '#app/composables/router'

defineProps<{
  namespace: string
  meta: string
  configs: FlatConfigItem[]
}>()

const router = useRouter()
function goToConfig(idx: number) {
  router.push(`/configs?index=${idx}`)
}
</script>

<template>
  <!-- :open="open" -->
  <details
    border="~ base rounded-lg" relative
  >
    <!-- @toggle="open = $event.target.open" -->
    <summary block>
      <div badge>
        {{ meta }}
      </div>
    </summary>

    <div flex="~ col gap-4" of-auto px4 py4>
      <div flex="~">
        <div>Namespace</div>
        <div>{{ namespace }}</div>
      </div>

      <div flex="~ gap-2 items-center">
        <div i-ph-stack-duotone flex-none />
        <div>Configs Specific to the Plugin ({{ configs.length }})</div>
      </div>

      <div flex="~ col gap-1" ml6 mt--2>
        <div v-for="config, idx of configs" :key="idx" font-mono flex="~ gap-2">
          <VDropdown>
            <button badge text-start>
              <ColorizedConfigName :name="config.name" :index="idx" />
            </button>
            <template #popper="{ shown }">
              <div v-if="shown" max-h="50vh" min-w-100>
                <div flex="~ items-center gap-2" p3>
                  <button
                    btn-action-sm
                    title="Copy"
                    @click="goToConfig(idx)"
                  >
                    <div i-ph-stack-duotone />
                    Go to this config
                  </button>
                  <slot name="popup-actions" />
                </div>
              </div>
            </template>
          </VDropdown>
        </div>
      </div>
    </div>
  </details>
</template>
