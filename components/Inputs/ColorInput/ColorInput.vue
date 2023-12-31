<script setup lang="ts">
// Types
import type { IColorProps } from '~/components/Inputs/ColorInput/types/color-props.type'

// Functions
import { useFieldUtils } from '~/components/Field/functions/useFieldUtils'

// Components
import MenuProxy from '~/components/MenuProxy/MenuProxy.vue'
import Field from '~/components/Field/Field.vue'

const props = withDefaults(defineProps<IColorProps>(), {
  icon: 'material-symbols:format-color-text-rounded',
  stackLabel: undefined,
  labelInside: undefined,
  inline: undefined,
})
const emits = defineEmits<{
  (e: 'update:modelValue', value: any): void
}>()

// Lifcecycle
onMounted(() => {
  referenceEl.value = unrefElement(fieldEl as any)?.querySelector(
    '.wrapper-body'
  ) as HTMLDivElement
})

// Layout
const fieldEl = ref<InstanceType<typeof Field>>()
const referenceEl = ref<HTMLDivElement>()
const menuEl = ref<InstanceType<typeof MenuProxy>>()
const model = useVModel(props, 'modelValue', emits)

function handlePickColor(color?: string) {
  model.value = color
  fieldEl.value?.focus()

  menuEl.value?.hide()
  nextTick(() => {
    fieldEl.value?.blur()
  })
}

// Field
const { getFieldProps, handleClickWrapper, handleFocusOrClick } = useFieldUtils(
  {
    props,
    menuElRef: menuEl,
  }
)

const fieldProps = getFieldProps(props)
</script>

<template>
  <Field
    ref="fieldEl"
    v-bind="fieldProps"
    :no-content="!model"
    @click="handleClickWrapper"
    @focus="handleFocusOrClick"
  >
    <span :style="{ color: model }">
      {{ model || '&nbsp;' }}
    </span>

    <MenuProxy
      ref="menuEl"
      hide-header
      manual
      tabindex="-1"
      :fit="false"
      placement="bottom-start"
      :reference-target="referenceEl"
      no-uplift
    >
      <ColorBrandingPicker
        v-model="model"
        @update:model-value="handlePickColor"
      />
    </MenuProxy>

    <template #append>
      <div
        size="sm"
        :class="icon"
        m="x-2"
        tabindex="-1"
        cursor="pointer"
        :style="{ color: model }"
      />
    </template>
  </Field>
</template>
