<template>
  <li role="presentation" :class="processedAttrs.wrapperClass" v-bind="wrapperAttrs">
    <component
      :is="props.tag"
      class="dropdown-item-text"
      :class="[colorClasses, props.textClass]"
      v-bind="processedAttrs.textAttrs"
    >
      <slot>
        {{ props.text }}
      </slot>
    </component>
  </li>
</template>

<script setup lang="ts">
import {computed, useAttrs} from 'vue'
import {useDefaults} from '../../composables/useDefaults'
import type {BDropdownTextProps} from '../../types/ComponentProps'
import {useColorVariantClasses} from '../../composables/useColorVariantClasses'

defineOptions({
  inheritAttrs: false,
})
const attrs = useAttrs()
const processedAttrs = computed(() => {
  const {class: wrapperClass, ...textAttrs} = attrs
  return {wrapperClass, textAttrs}
})

const _props = withDefaults(defineProps<BDropdownTextProps>(), {
  textClass: undefined,
  tag: 'span',
  text: undefined,
  variant: null,
  wrapperAttrs: undefined,
})
const props = useDefaults(_props, 'BDropdownText')

const colorClasses = useColorVariantClasses(
  computed(() => ({
    textVariant: props.variant,
  }))
)
defineSlots<{
  // eslint-disable-next-line @typescript-eslint/no-explicit-any
  default?: (props: Record<string, never>) => any
}>()
</script>
