<template>
  <label class="">
    <input
      :disabled="isLocalDisabled || isDisabled"
      :indeterminate="isIndeterminate"
      :checked="isChecked"
      @change="onToggle"
      class=""
      type="checkbox"
    >
    <span v-if="label">
      {{ label }}
    </span>
    <slot v-else />
  </label>
</template>

<script>
export default {
  name: 'BaseCheckboxIndeterminate',

  props: [
    'modelValue',
    'sourceArray',
    'isDisabled',
    'labelEmpty',
    'labelDisabled',
    'labelTrue',
    'labelFalse',
  ],

  computed: {
    reducer() {
      const reducer = new Set(this.sourceArray);

      this.modelValue.forEach((e) => {
        reducer.delete(e);
      });

      return reducer;
    },

    isLocalDisabled() {
      return !this.sourceArray.length;
    },

    isEmpty() {
      return !this.sourceArray.length || !this.modelValue.length;
    },

    isIndeterminate() {
      if (this.isEmpty) return false;

      return this.modelValue.length && this.reducer.size !== 0;
    },

    isChecked() {
      if (this.isEmpty) return false;

      return this.modelValue.length > 0;
    },

    label() {
      if (this.isLocalDisabled) return this.labelEmpty;

      if (this.isDisabled) return this.labelDisabled;

      if (this.isChecked || this.isIndeterminate) return this.labelTrue;

      return this.labelFalse;
    },
  },

  methods: {
    onToggle() {
      if (this.isChecked || this.isIndeterminate) {
        this.$emit('update:modelValue', []);
      } else {
        this.$emit('update:modelValue', [...this.sourceArray]);
      }
    },
  },
};
</script>

<style lang="scss">

</style>
