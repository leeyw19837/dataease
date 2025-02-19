<script lang="ts" setup>
import { toRefs, onBeforeMount, type PropType } from 'vue'
interface SelectConfig {
  conditionValueOperatorF: string
  conditionValueF: string
  conditionValueOperatorS: string
  conditionValueS: string
  defaultConditionValueOperatorF: string
  defaultConditionValueF: string
  defaultConditionValueOperatorS: string
  defaultConditionValueS: string
  conditionType: number
}

const operators = [
  {
    label: '精准匹配',
    value: 'eq'
  },
  {
    label: '模糊匹配',
    value: 'like'
  }
]

const props = defineProps({
  config: {
    type: Object as PropType<SelectConfig>,
    default: () => {
      return {
        conditionType: 0,
        conditionValueOperatorF: 'eq',
        conditionValueF: '',
        conditionValueOperatorS: 'like',
        conditionValueS: '',
        defaultConditionValueOperatorF: 'eq',
        defaultConditionValueF: '',
        defaultConditionValueOperatorS: 'like',
        defaultConditionValueS: ''
      }
    }
  }
})
const { config } = toRefs(props)
const setParams = () => {
  const {
    defaultConditionValueOperatorF,
    defaultConditionValueF,
    defaultConditionValueOperatorS,
    defaultConditionValueS
  } = config.value
  config.value.conditionValueOperatorF = defaultConditionValueOperatorF
  config.value.conditionValueF = defaultConditionValueF
  config.value.conditionValueOperatorS = defaultConditionValueOperatorS
  config.value.conditionValueS = defaultConditionValueS
}
onBeforeMount(() => {
  setParams()
})
</script>

<template>
  <div class="text-search-select">
    <div class="condition-type">
      <el-select
        class="condition-value-select"
        popper-class="condition-value-select-popper"
        v-model="config.conditionValueOperatorF"
      >
        <el-option v-for="ele in operators" :key="ele.value" :label="ele.label" :value="ele.value">
        </el-option>
      </el-select>
      <el-input class="condition-value-input" v-model="config.conditionValueF" />
      <div class="bottom-line"></div>
    </div>
    <div class="condition-type" v-if="[1, 2].includes(config.conditionType)">
      <sapn class="condition-type-tip">{{ config.conditionType === 1 ? '与' : '或' }}</sapn>
      <el-select
        class="condition-value-select"
        popper-class="condition-value-select-popper"
        v-model="config.conditionValueOperatorS"
      >
        <el-option v-for="ele in operators" :key="ele.value" :label="ele.label" :value="ele.value">
        </el-option>
      </el-select>
      <el-input class="condition-value-input" v-model="config.conditionValueS" />
      <div class="bottom-line next-line"></div>
    </div>
  </div>
</template>

<style lang="less" scoped>
.text-search-select {
  display: flex;
  align-items: center;
  .condition-type {
    display: flex;
    position: relative;
    :deep(.ed-input__wrapper) {
      border: none;
      border-radius: 0;
      box-shadow: none !important;
      height: 32px;
      word-wrap: break-word;
      text-align: left;
      color: rgba(0, 0, 0, 0.65);
      list-style: none;
      user-select: none;
      cursor: pointer;
      line-height: 32px;
      box-sizing: border-box;
      max-width: 100%;
      overflow: hidden;
      white-space: nowrap;
      text-overflow: ellipsis;
      opacity: 1;
    }

    :deep(.ed-select .ed-input.is-focus .ed-input__wrapper),
    :deep(.ed-select:hover:not(.ed-select--disabled) .ed-input__wrapper),
    :deep(.ed-select .ed-input__wrapper.is-focus) {
      box-shadow: none !important;
    }

    :deep(.ed-select) {
      width: 120px;
      .ed-input__wrapper {
        padding: 0;
      }
    }

    .condition-type-tip {
      font-size: 12px;
      color: #646a73;
      line-height: 32px;
      margin-right: 8px;
    }

    .bottom-line {
      box-sizing: border-box;
      height: 1px;
      background-color: #000;
      opacity: 0.3;
      position: absolute;
      right: 5px;
      bottom: 5px;
      width: 195px;
      z-index: 10;

      &.next-line {
        width: 195px;
      }
    }
  }
}
</style>
<style lang="less">
.condition-value-select-popper {
  .ed-select-dropdown__item.selected::after {
    display: none;
  }
}
</style>
