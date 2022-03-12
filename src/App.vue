<script lang="ts" setup>
import List from 'components/List.vue';
import SearchBar from 'components/SearchBar.vue';
import { reactive } from 'vue';

interface IState {
  searchFor: string;
  isExactFound: boolean;
  addNewItem: boolean;
}

const state: IState = reactive({
  searchFor: '',
  isExactFound: false,
  addNewItem: false,
});

function setExactStatus(params: boolean): void {
  state.isExactFound = params;
}

function appendList(): void {
  state.addNewItem = true;
}
</script>

<template>
  <div :class="$style['list-app']">
    <SearchBar
      v-model="state.searchFor"
      :disable-add-btn="state.isExactFound"
      @add-item-to-l-ist="appendList" />
    <List
      :search-query="state.searchFor"
      :add-new-item="state.addNewItem"
      @change-item-check-status="state.addNewItem = false"
      @update-exact-status="setExactStatus" />
  </div>
</template>

<style lang="scss" scoped module>
.list-app {
  display: flex;
  align-items: center;
  flex-direction: column;
  padding-top: 70px;
}
</style>
