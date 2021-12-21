<template>
  <LeftSidebar
    v-model:current-filter="currentFilter"
    v-model:is-sorted="isSorted"
    v-model:items-length="itemsLength"
    v-model:is-disabled="isDisabled"
  />

  <div class="list">
    <BaseCheckboxIndeterminate
      v-model="checkedItems"
      :source-array="sortedFilteredItems"
      :is-disabled="isDisabled"
      label-empty="Пусто"
      label-disabled="Не доступно"
      label-true="Отменить всё"
      label-false="Выбрать всё"
    />

    <label
      v-for="(item, index) in sortedFilteredItems"
      :key="`items-${index}`">
      <input
        :value="item"
        type="checkbox"
        v-model="checkedItems"
      >
      <span>
        id: {{ item.id }}, amount: {{ item.amount }}
      </span>
      <button @click.stop="item.amount += 1">
        +1
      </button>
      <button @click.stop="item.amount -= 1">
        -1
      </button>
    </label>
  </div>

  <div class="list">
    <label v-for="(item, index) in checkedItems" :key="`checked-items-${index}`">
      id: {{ item.id }}, amount: {{ item.amount }}
    </label>
  </div>
</template>

<script>
import LeftSidebar from '@/components/LeftSidebar.vue';
import BaseCheckboxIndeterminate from '@/components/BaseCheckboxIndeterminate.vue';

const compareBy = (key) => (a, b) => a[key] - b[key];
const createItem = (_, i) => ({ id: i + 1, amount: ((Math.random() * 2001) | 0) - 1000 });

export default {
  components: {
    LeftSidebar,
    BaseCheckboxIndeterminate,
  },

  data() {
    return {
      itemsLength: 0,
      items: [],
      currentFilter: '',
      isSorted: false,
      checkedItems: [],
      isDisabled: false,
      isInversion: false,
    };
  },

  computed: {
    filteredItems() {
      if (this.currentFilter === 'less') return this.items.filter((e) => e.amount < 0);
      if (this.currentFilter === 'more') return this.items.filter((e) => e.amount >= 0);

      return this.items;
    },

    sortedFilteredItems() {
      if (!this.isSorted) return this.filteredItems;

      const copyFilteredSheet = [...this.filteredItems];
      copyFilteredSheet.sort(compareBy('amount'));

      return copyFilteredSheet;
    },
  },

  watch: {
    itemsLength(length) {
      this.items = Array.from({ length }, createItem);
      this.checkedItems = [];
    },
  },
};
</script>

<style lang="scss">
html, body, #app {
  padding: 0;
  margin: 0;
  width: 100%;
  height: 100%;
}

#app {
  display: flex;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  font-size: 16px;

  & * {
    font-family: inherit;
    font-size: inherit;
    font-weight: inherit;
    box-sizing: border-box;
  }
}

.list {
  border: 1px solid lightgray;
  flex-grow: 1;
  flex-shrink: 1;
  overflow: hidden auto;
  width: 0;

  & hr {
    visibility: hidden;
  }

  & > label {
    width: 100%;
    padding: 10px;
    display: inline-block;
    cursor: pointer;
    user-select: none;

    & > input {
      vertical-align: -1px;
      margin: 0;
    }

    & > span {
      margin: 0 1ex;
    }

    &:hover {
      background: rgba(0,0,0,0.03);
    }
  }
}
</style>
