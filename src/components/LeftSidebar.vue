<template>
  <div class="list">
    <label><input value="" type="radio" v-model="changeFilter"> Все</label>
    <label><input value="less" type="radio" v-model="changeFilter"> Меньше 0</label>
    <label><input value="more" type="radio" v-model="changeFilter"> Больше или равно 0 </label>
    <hr>

    <label><input type="checkbox" :checked="true" v-model="changeSort"> Сортировать по сумме</label>
    <hr>

    <label><input type="number" v-model="arrayLength"></label>
    <label><input type="checkbox" v-model="isLocalDisabled">Заблокировать</label>
  </div>
</template>
<script>
export default {
  props: ['currentFilter', 'isSorted', 'itemsLength', 'isDisabled'],

  computed: {
    isLocalDisabled: {
      get() { return this.isDisabled; },
      set(value) { this.$emit('update:isDisabled', value); },
    },

    changeFilter: {
      get() { return this.currentFilter; },
      set(value) { this.$emit('update:currentFilter', value); },
    },

    changeSort: {
      get() { return this.isSorted; },
      set(value) { this.$emit('update:isSorted', value); },
    },

    arrayLength: {
      get() {
        return this.itemsLength;
      },

      set(value) {
        this.$emit('update:itemsLength', +value);
      },
    },
  },
};
</script>
<style lang="scss">

</style>
