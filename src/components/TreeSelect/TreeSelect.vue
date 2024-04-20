<template>
  <div class="relative inline-block text-start">
    <div>
      <button
        @click="toggleDropdown"
        type="button"
        class="inline-flex justify-center w-full rounded-md border border-gray-300 px-4 py-2 bg-white text-sm font-medium text-gray-700 hover:bg-gray-50 focus:outline-none focus:border-blue-300 focus:ring focus:ring-blue-200 active:bg-gray-200 active:text-gray-800 transition ease-in-out duration-150"
      >
        <!-- Selected Option -->
        {{ selectedOption || "Select an option" }} {{ selectedOptions }}
        <!-- Dropdown arrow -->
        <svg
          :class="{ 'transform rotate-180': isOpen }"
          class="ml-2 -mr-0.5 h-4 w-4 transition-transform"
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 20 20"
          fill="currentColor"
          aria-hidden="true"
        >
          <path
            fill-rule="evenodd"
            d="M10.707 14.293a1 1 0 0 1-1.414 0l-4-4a1 1 0 1 1 1.414-1.414L10 11.586l3.293-3.293a1 1 0 1 1 1.414 1.414l-4 4z"
            clip-rule="evenodd"
          />
        </svg>
      </button>
    </div>
    <!-- Dropdown Items -->
    <div
      v-if="isOpen"
      class="origin-top-right absolute right-0 w-full rounded-b shadow bg-white ring-1 ring-black ring-opacity-5 focus:outline-none"
    >
      <div
        class="py-1"
        role="menu"
        aria-orientation="vertical"
        aria-labelledby="options-menu"
      >
        <TreeSelectOption
          v-for="option in options ?? []"
          :key="option.value"
          :option="option"
          :default-expand-level="defaultExpandLevel"
          v-model:selected-options="selectedOptions"
        />
      </div>
    </div>
  </div>
</template>

<script>
import TreeSelectOption from "./TreeSelectOption.vue";
export default {
  name: "TreeSelect",
  components: {
    TreeSelectOption,
  },

  props: {
    placeHolder: {
      type: String,
      default: "TreeSelect",
    },

    defaultExpandLevel: {
      type: Number,
      default: 0,
    },

    options: {
      type: Array,
      default: () => [
        {
          value: "1",
          label: "Parent 1",
          children: [
            {
              value: "1-1",
              label: "Child 1-1",
            },
            {
              value: "1-2",
              label: "Child 1-2",
              children: [
                {
                  value: "1-2-1",
                  label: "Grandchild 1-2-1",
                },
                {
                  value: "1-2-2",
                  label: "Grandchild 1-2-2",
                },
              ],
            },
          ],
        },
        {
          value: "2",
          label: "Parent 2",
          children: [
            {
              value: "2-1",
              label: "Child 2-1",
            },
            {
              value: "2-2",
              label: "Child 2-2",
              children: [
                {
                  value: "2-2-1",
                  label: "Grandchild 2-2-1",
                },
                {
                  value: "2-2-2",
                  label: "Grandchild 2-2-2",
                },
              ],
            },
          ],
        },
        {
          value: "3",
          label: "Parent 3",
        },
      ],
    },
  },

  data() {
    return {
      isOpen: true,
      selectedOption: null,
      selectedOptions: [],
    };
  },

  methods: {
    toggleDropdown() {
      this.isOpen = !this.isOpen;
    },
    selectOption(option) {
      this.selectedOption = option;
      this.isOpen = false;
    },
  },
};
</script>
