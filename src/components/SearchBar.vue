<script lang="ts" setup>
import { defineEmits, defineProps } from 'vue';
import Icon from './Icons.vue';

interface IProps {
  modelValue?: string;
  disableAddBtn?: boolean;
}

const props = defineProps<IProps>();

const emit = defineEmits(['update:modelValue', 'addItemToLIst']);

function updateValue(event: any): void {
  emit('update:modelValue', event.target.value);
}
function clearInput(): void {
  emit('update:modelValue', '');
}
function submit(): void {
  if (props.disableAddBtn === false) {
    emit('addItemToLIst');
  }
}
</script>

<template>
  <div :class="$style['search-bar-box']">
    <input
      ref="searchBarRef"
      type="text"
      :value="props.modelValue"
      name=""
      placeholder="Search or Add..."
      @keyup.enter="submit"
      @keydown.esc="clearInput"
      @input="updateValue" />
    <span v-if="props.modelValue" :class="$style['search-bar-icons']">
      <span :class="$style['cancel-icon']" @click="clearInput"
        ><Icon show-icon="cancel"
      /></span>
      <span
        :class="[
          $style['add-icon'],
          props.disableAddBtn ? $style['disable-btn'] : '',
        ]"
        @click="submit"
        ><Icon show-icon="add"
      /></span>
    </span>
  </div>
</template>

<style lang="scss" scoped module>
@use 'sass/color';
.search-bar-box {
  position: relative;

  input {
    width: 800px;
    height: 60px;
    background: color.$input-bg;
    border-radius: 6px;
    border: 0;
    font: normal normal normal 14px/19px Open Sans;
    letter-spacing: 0px;
    color: color.$input-color;
    padding-left: 15px;

    :focus,
    :focus-visible {
      border: 0;
    }
  }

  .search-bar-icons {
    position: absolute;
    display: block;
    right: 20px;
    top: 20px;

    span.cancel-icon {
      margin-right: 17px;
      color: color.$red-color;
      cursor: pointer;
    }

    span.add-icon {
      color: color.$add-color;
      cursor: pointer;

      &.disable-btn {
        cursor: not-allowed;
        color: color.$input-color;
      }
    }
  }
}
// Style
</style>
