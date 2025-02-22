<template>
  <app-sheet>
    <v-table class="api-table" density="comfortable">
      <thead>
        <tr>
          <th
            v-for="header in headers"
            :key="header"
            :class="header"
          >
            <div
              class="text-capitalize"
              v-text="header"
            />
          </th>
        </tr>
      </thead>

      <tbody>
        <template v-for="item in items" :key="item.name">
          <slot
            name="row"
            v-bind="{
              ...item,
              props: {
                class: theme.dark ? 'bg-grey-darken-3' : 'bg-grey-lighten-4'
              }
            }"
          />

          <tr v-if="item.description || (DEV && item.source)">
            <td colspan="3" class="text-mono pt-4">
              <template v-if="item.description">
                <app-markdown
                  v-if="localeStore.locale !== 'eo-UY'"
                  :content="item.description"
                  class="mb-0"
                />
                <span v-else>{{ item.description }}</span>
              </template>

              <p v-if="DEV && item.source">
                <strong>source: {{ item.source }}</strong>
              </p>
            </td>
          </tr>
        </template>
      </tbody>
    </v-table>
  </app-sheet>
</template>

<script setup lang="ts">
  // Composables
  import { useTheme } from 'vuetify'

  // Utilities
  import { PropType } from 'vue'
  import { useLocaleStore } from '@/store/locale'

  defineProps({
    headers: {
      type: Array as PropType<string[]>,
      default: () => ([]),
    },
    items: {
      type: Array as PropType<any[]>,
      default: () => [],
    },
  })

  const { current: theme } = useTheme()
  const localeStore = useLocaleStore()

  const DEV = import.meta.env.DEV
</script>
