<script setup lang="ts">
import type { FlatConfigItem } from '~~/shared/types'
import { computed } from 'vue'
import { payload } from '~/composables/payload'

const plugins = computed(() => {
  const res = new Map<string, {
    meta: string
    configs: FlatConfigItem[]
  }>()

  payload.value.configs.forEach((config) => {
    if (!config.plugins)
      return

    Object.entries(config.plugins).forEach(([namespace, meta]) => {
      if (!res.has(namespace)) {
        res.set(namespace, {
          meta: meta as unknown as string,
          configs: [],
        })
      }

      const data = res.get(namespace)!

      data.configs.push(config)

      res.set(namespace, data)
    })
  })

  return res
})
</script>

<template>
  <div flex="~ gap-2 col">
    <PluginItem
      v-for="[namespace, { meta, configs }] in plugins" :key="namespace"
      :namespace
      :meta
      :configs
    />
  </div>
</template>
