<script lang="ts" setup>
import {
  computed,
  defineEmits,
  defineProps,
  onBeforeMount,
  reactive,
  watch,
} from 'vue';
import Item from './Item.vue';

interface IProps {
  searchQuery?: string;
  addNewItem?: boolean;
}

interface IState {
  list: IList[];
  active_filter: string;
}

interface IList {
  id: number;
  value?: string;
  time: string;
}

const props = defineProps<IProps>();

const state: IState = reactive({
  list: [
    { id: 1, value: 'John Smith', time: '2022-03-10T19:49:49.663Z' },
    { id: 2, value: 'Aria Blaze', time: '2022-03-10T19:53:49.663Z' },
    { id: 3, value: 'Rias Gremory', time: '2022-03-10T19:56:49.663Z' },
  ],
  active_filter: 'value',
});

const emit = defineEmits(['changeItemCheckStatus', 'updateExactStatus']);

onBeforeMount(() => {
  let store_list = localStorage.getItem('list');
  if (store_list) {
    state.list = JSON.parse(store_list);
  }
  let store_filter = localStorage.getItem('active_filter');
  if (store_filter) {
    state.active_filter = store_filter;
  }
});

// const propsAddItem = ref(props.addNewItem);

watch(
  () => props.addNewItem,
  (newVal) => {
    if (newVal === true) {
      let item_value: string | undefined = props.searchQuery;
      let item_obj = {
        id: 1,
        value: item_value ? item_value : '',
        time: new Date().toISOString(),
      };
      state.list.unshift(item_obj);
      emit('changeItemCheckStatus');
      for (let i = 0; i < state.list.length; i++) {
        let element = state.list[i];
        element.id = i + 1;
      }
      localStorage.setItem('list', JSON.stringify(state.list));
    }
  }
);

const listData = computed(() => {
  let sort_list = state.list.filter((item) =>
    item.value?.includes(props.searchQuery ? props.searchQuery : '')
  );
  localStorage.setItem('active_filter', state.active_filter);
  if (state.active_filter === 'value') {
    return sort_list.sort(function (a, b) {
      if (a.value && b.value) {
        return a.value.localeCompare(b.value);
      } else {
        return 0;
      }
    });
  } else {
    return sort_list.sort(function (a, b) {
      let time_a: any = new Date(b.time);
      let time_b: any = new Date(a.time);
      return time_a - time_b;
    });
  }
});

function isExactFound(item_value: string | undefined) {
  let check = props.searchQuery === item_value;
  emit('updateExactStatus', check);
  return check;
}
function deleteItem(params: number | string) {
  let index = state.list.findIndex((item) => item.id === params);
  if (index > -1) {
    state.list.splice(index, 1);
    localStorage.setItem('list', JSON.stringify(state.list));
  }
}
</script>

<template>
  <div :class="$style['list-content']">
    <div>
      <div v-if="!listData.length" :class="$style['placeholder-item']">
        No item found!
      </div>
      <Item
        v-for="(item, i) in listData"
        :key="i"
        :item-data="item"
        :is-exact="isExactFound(item.value)"
        @remove-item-from-list="deleteItem" />
    </div>
    <div :class="$style['buttons']">
      <button
        :class="[state.active_filter === 'value' ? $style['active'] : '']"
        @click="state.active_filter = 'value'">
        <span>Sort by</span> Value
        <span :class="$style['active-dot']" />
      </button>
      <button
        :class="[state.active_filter === 'time' ? $style['active'] : '']"
        @click="state.active_filter = 'time'">
        <span>Sort by</span> Date
        <span :class="$style['active-dot']" />
      </button>
    </div>
  </div>
</template>

<style lang="scss" scoped module>
@use 'sass/color';
.list-content {
  position: relative;
  .buttons {
    position: absolute;
    top: 15px;
    right: -230px;
    display: grid;

    button {
      width: 200px;
      height: 40px;
      background: color.$bg-gray;
      border-radius: 6px;
      font: normal normal normal 13px/18px Open Sans;
      letter-spacing: 0px;
      border: 0;
      text-align: left;
      padding-left: 10px;
      margin-bottom: 10px;
      cursor: pointer;

      &.active {
        background: color.$bg-white;

        span.active-dot {
          width: 5px;
          height: 5px;
          /* UI Properties */
          background: color.$btn-bg;
          position: absolute;
          right: 19px;
          margin-top: 7px;
          border-radius: 50%;
        }
      }

      span {
        color: color.$text-gray;
      }
    }
  }

  .placeholder-item {
    width: 800px;
    height: 70px;
    display: flex;
    align-items: center;
  }
}
</style>
