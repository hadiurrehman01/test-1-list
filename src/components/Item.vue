<script lang="ts" setup>
import { formatDistanceStrict } from 'date-fns';
import {
  defineEmits,
  defineProps,
  onBeforeUnmount,
  onMounted,
  reactive,
} from 'vue';
import Icon from './Icons.vue';

type IItem = {
  id: number;
  value?: string;
  time: string;
};

const props = defineProps<{
  itemData: IItem;
  isExact?: boolean;
}>();

const state = reactive({
  time_value: '',
  interval: 0,
});

const emit = defineEmits(['removeItemFromList']);

onMounted(() => {
  let date = new Date();
  let add_time = new Date(props.itemData.time);
  state.time_value = formatDistanceStrict(new Date(date), new Date(add_time), {
    roundingMethod: 'ceil',
  });
  state.interval = setInterval(() => {
    let date = new Date();
    let add_time = new Date(props.itemData.time);
    state.time_value = formatDistanceStrict(
      new Date(date),
      new Date(add_time),
      {
        roundingMethod: 'ceil',
      }
    );
  }, 1000);
});

onBeforeUnmount(() => {
  clearInterval(state.interval);
});

function removeItem(item_id: number | string) {
  emit('removeItemFromList', item_id);
}
</script>

<template>
  <div :class="$style['item-content']">
    <div v-if="props.itemData" :class="$style['value-content']">
      <span v-if="props.isExact" :class="$style['check-icon']"
        ><Icon show-icon="check"
      /></span>
      <span>
        {{ props.itemData.value }}<br />
        <span :class="$style['item-id']"
          ><span v-if="props.isExact" :class="$style['match-text']"
            >Exact match,</span
          >
          #{{ props.itemData.id }}</span
        >
      </span>
    </div>
    <div :class="$style['content-btn']">
      <span :class="$style['time-value']">
        {{ state.time_value }}
      </span>
      <span :class="$style['trash-icon']" @click="removeItem(props.itemData.id)"
        ><Icon show-icon="trash"
      /></span>
    </div>
  </div>
</template>

<style lang="scss" module>
@use 'sass/color';
.item-content {
  background: color.$bg-gray;
  box-shadow: 0px 0px 40px #0000000d;
  border-radius: 6px;
  width: 800px;
  height: 70px;
  display: flex;
  align-items: center;
  padding: 0 15px;
  border-bottom: 1px solid #e9ecef;
  margin-bottom: 7px;

  .content-btn {
    width: 50%;
    text-align: right;
    .trash-icon {
      position: relative;
      top: 6px;
      display: inline-block;
      margin-left: 30px;
      color: color.$red-color;
      display: none;
      cursor: pointer;
    }
  }

  &:first-child {
    margin-top: 14px;
  }

  &:hover {
    background: color.$bg-white;

    .content-btn .trash-icon {
      display: inline-block;
    }
  }
}
.value-content {
  width: 50%;
  span {
    color: color.$text-gray;
    display: inline-block;

    &:first-child {
      margin-right: 25px;

      &.match-text {
        margin-right: 0;
      }
    }

    &.item-id {
      font-size: 12px;
    }
  }
}
</style>
