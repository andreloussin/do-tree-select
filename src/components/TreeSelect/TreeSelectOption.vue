<template>
  <div>
    <span
      v-if="hasChildren"
      class="inline-flex align-center transition ease-in-out duration-15"
    >
      <svg
        :class="showChildren ? '' : 'transform -rotate-90'"
        class="w-4 transition-transform"
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 20 20"
        fill="currentColor"
        aria-hidden="true"
        @click="toggleChildren"
      >
        <path
          fill-rule="evenodd"
          d="M10.707 14.293a1 1 0 0 1-1.414 0l-4-4a1 1 0 1 1 1.414-1.414L10 11.586l3.293-3.293a1 1 0 1 1 1.414 1.414l-4 4z"
          clip-rule="evenodd"
        />
      </svg>
    </span>
    <span v-else class="mx-1"></span>
    <span class="mr-2">
      <input
        type="checkbox"
        :checked="isSelected"
        @change="toggleSelection"
        :indeterminate="isIndeterminate"
      />
    </span>
    <span @click="toggleSelection">{{ option.label }}</span>
    <template v-if="showChildren">
      <TreeSelectOption
        v-for="child in option.children ?? []"
        class="ml-2"
        :key="child.value"
        :option="child"
        :default-expand-level="defaultExpandLevel - 1"
        v-model:selected-options="mutableSelectedOptions"
      />
    </template>
  </div>
</template>

<script>
import TreeSelectOption from "./TreeSelectOption.vue";
import isEmpty from "lodash/isEmpty";
import isArray from "lodash/isArray";
import remove from "lodash/remove";

export default {
  components: {
    TreeSelectOption,
  },
  props: {
    option: {
      type: Object,
      required: true,
    },

    selectedOptions: {
      type: Array,
      default: () => [],
    },

    defaultExpandLevel: {
      type: Number,
      default: 0,
    },
  },

  emits: ["update:selectedOptions"],

  data() {
    return {
      mutableSelectedOptions: this.selectedOptions,
      selectedNode: null,
      showChildren: this.defaultExpandLevel > 0,
    };
  },
  computed: {
    nodes() {
      return this.treeData || [];
    },

    isSelected() {
      return this.isSelectedOption(this.option);
    },

    isIndeterminate() {
      return this.isIndeterminateOption(this.option);
    },

    hasChildren() {
      return this.hasOptionChildren(this.option);
    },
  },

  watch: {
    mutableSelectedOptions: {
      handler(newValue) {
        this.$emit("update:selectedOptions", newValue);
      },
      deep: true,
    },
    selectedOptions: {
      handler(newValue) {
        this.mutableSelectedOptions = newValue;
      },
      deep: true,
    },
  },

  methods: {
    toggleChildren() {
      this.showChildren = !this.showChildren;
    },

    toggleSelection() {
      if (this.isSelected) {
        this.unSelectOption(this.option);
      } else {
        this.selectOption(this.option);
      }
    },

    hasOptionChildren(option) {
      return isArray(option.children) && !isEmpty(option.children);
    },

    selectOption(option) {
      if (this.hasOptionChildren(option)) {
        option.children.forEach((child) => this.selectOption(child));
        return;
      }

      this.mutableSelectedOptions.push(option);
    },

    unSelectOption(option) {
      if (this.hasOptionChildren(option)) {
        option.children.forEach((child) => this.unSelectOption(child));
        return;
      }
      remove(
        this.mutableSelectedOptions,
        (option) => option.value === this.option.value
      );
    },

    isSelectedOption(option) {
      if (this.hasOptionChildren(option)) {
        return option.children.every((child) => this.isSelectedOption(child));
      }

      return this.selectedOptions.some((child) => child.value === option.value);
    },

    isIndeterminateOption(option) {
      if (this.hasOptionChildren(option)) {
        return (
          !this.isSelected &&
          (option.children.some((child) => this.isSelectedOption(child)) ||
            option.children.some((child) => this.isIndeterminateOption(child)))
        );
      }

      return false;
    },
  },
};
</script>
