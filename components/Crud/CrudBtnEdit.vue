<script setup lang="ts">
// TYPES
import type { IBtnProps } from '~/components/Button/types/btn-props.type'
import type { ICrudBtnProps } from '~/components/Crud/types/crud-btn-props.type'

type IProps = ICrudBtnProps & {
  to?: IBtnProps['to']
}

withDefaults(defineProps<IProps>(), {
  loaderType: 'block',
})
</script>

<template>
  <Btn
    preset="EDIT"
    :label="labels && (label || $t('edit'))"
    :loader-type="loaderType"
    :loading="loading"
    :to="to"
    no-dim
  />
</template>
