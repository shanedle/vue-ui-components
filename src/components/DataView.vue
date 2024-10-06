<template>
  <div class="data-view">
    <div class="controls">
      <div class="search-wrapper">
        <CustomInput
          v-model="searchQuery"
          placeholder="Search..."
          iconBefore="fas fa-search"
          @update:modelValue="handleSearch"
          size="md"
        />
      </div>
      <div class="buttons-wrapper">
        <CustomButton
          :label="viewMode === 'grid' ? 'Table View' : 'Grid View'"
          :iconBefore="viewMode === 'grid' ? 'fas fa-table' : 'fas fa-th'"
          @click="toggleViewMode"
          size="md"
        />
        <CustomButton label="Select All" @click="toggleSelectAll" size="md" />
        <div class="sort-wrapper">
          <select v-model="sortOption" @change="handleSort">
            <option value="nameAsc">Name (A-Z)</option>
            <option value="nameDesc">Name (Z-A)</option>
            <option value="custom">Custom</option>
          </select>
        </div>
      </div>
    </div>

    <div v-if="viewMode === 'grid'" class="data-container grid">
      <div
        v-for="item in sortedAndFilteredData"
        :key="item.id"
        :class="['data-item', { selected: selectedItems.includes(item.id) }]"
      >
        <img :src="item.image" :alt="item.name" class="item-image" />
        <div class="item-details">
          <h3>{{ item.name }}</h3>
          <p>{{ item.description }}</p>
        </div>
        <div class="item-actions">
          <CustomButton
            label="Edit"
            variant="solid"
            iconBefore="fas fa-edit"
            @click="editItem(item)"
            size="sm"
          />
          <CustomButton
            label="Delete"
            variant="outline"
            iconBefore="fas fa-trash"
            @click="deleteItem(item)"
            size="sm"
          />
        </div>
      </div>
    </div>

    <table v-else class="data-container table">
      <thead>
        <tr>
          <th>Image</th>
          <th>Name</th>
          <th>Description</th>
          <th class="actions-column">Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="item in sortedAndFilteredData"
          :key="item.id"
          :class="{ selected: selectedItems.includes(item.id) }"
        >
          <td>
            <img :src="item.image" :alt="item.name" class="item-image-small" />
          </td>
          <td>{{ item.name }}</td>
          <td>{{ item.description }}</td>
          <td class="actions-column">
            <div class="action-buttons">
              <CustomButton
                label="Edit"
                variant="solid"
                iconBefore="fas fa-edit"
                @click="editItem(item)"
                size="sm"
              />
              <CustomButton
                label="Delete"
                variant="outline"
                iconBefore="fas fa-trash"
                @click="deleteItem(item)"
                size="sm"
              />
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, computed } from "vue";
import CustomButton from "./Button.vue";
import CustomInput from "./Input.vue";

interface DataItem {
  id: number;
  name: string;
  description: string;
  image: string;
}

export default defineComponent({
  name: "DataView",
  components: {
    CustomButton,
    CustomInput,
  },
  props: {
    data: {
      type: Array as () => DataItem[],
      required: true,
    },
  },
  emits: ["edit", "delete"],
  setup(props, { emit }) {
    const viewMode = ref<"grid" | "table">("grid");
    const searchQuery = ref("");
    const sortOption = ref("nameAsc");
    const selectedItems = ref<number[]>([]);

    const toggleViewMode = () => {
      viewMode.value = viewMode.value === "grid" ? "table" : "grid";
    };

    const handleSearch = (query: string) => {
      searchQuery.value = query;
    };

    const handleSort = () => {
      if (sortOption.value === "custom") {
        console.log("Custom sorting selected");
      }
    };

    const toggleSelectAll = () => {
      if (selectedItems.value.length === props.data.length) {
        selectedItems.value = [];
      } else {
        selectedItems.value = props.data.map((item) => item.id);
      }
    };

    const editItem = (item: DataItem) => {
      emit("edit", item);
    };

    const deleteItem = (item: DataItem) => {
      emit("delete", item);
    };

    const sortedAndFilteredData = computed(() => {
      let result = [...props.data];

      if (searchQuery.value.trim()) {
        const query = searchQuery.value.toLowerCase().trim();
        result = result.filter(
          (item) =>
            item.name.toLowerCase().includes(query) ||
            item.description.toLowerCase().includes(query)
        );
      }

      result.sort((a, b) => {
        if (sortOption.value === "nameAsc") {
          return a.name.localeCompare(b.name);
        } else if (sortOption.value === "nameDesc") {
          return b.name.localeCompare(a.name);
        }
        return 0;
      });

      return result;
    });

    return {
      viewMode,
      searchQuery,
      sortOption,
      selectedItems,
      toggleViewMode,
      handleSearch,
      handleSort,
      toggleSelectAll,
      editItem,
      deleteItem,
      sortedAndFilteredData,
    };
  },
});
</script>

<style scoped>
.controls {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
  align-items: center;
  padding: 10px;
}

.search-wrapper {
  flex-grow: 1;
  min-width: 200px;
  height: 40px;
}

.buttons-wrapper {
  display: flex;
  gap: 10px;
  align-items: stretch;
}

.buttons-wrapper > * {
  flex-shrink: 0;
}

.sort-wrapper {
  position: relative;
  height: 40px;
  display: inline-block;
}

.sort-wrapper select {
  height: 100%;
  padding: 0 30px 0 10px;
  font-size: 14px;
  border: 1px solid var(--border-color);
  background-color: var(--background-color);
  color: var(--text-color);
  cursor: pointer;
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
  border-radius: 4px;
  width: 100%;
}

.sort-wrapper::after {
  content: "\25BC";
  position: absolute;
  top: 50%;
  right: 10px;
  transform: translateY(-50%);
  pointer-events: none;
  color: var(--text-color);
}

.data-container.grid {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.data-item {
  border: 1px solid var(--border-color);
  padding: 16px;
  width: calc(33.33% - 14px);
  box-sizing: border-box;
  transition: background-color var(--transition-duration),
    border-color var(--transition-duration);
}

.item-image {
  width: 100%;
  height: 200px;
  object-fit: cover;
  margin-bottom: 12px;
}

.item-details p {
  color: var(--text-color-secondary);
}

.selected {
  background-color: var(--selected-background);
}

.data-container.table {
  width: 100%;
  border-collapse: separate;
  border-spacing: 0;
}

.data-container.table th,
.data-container.table td {
  padding: 12px;
  text-align: left;
  border-bottom: 1px solid var(--border-color);
  vertical-align: middle;
}

.data-container.table .actions-column {
  width: 1%;
  white-space: nowrap;
}

.data-container.table th.actions-column {
  text-align: center;
}

.data-container.table .action-buttons {
  display: flex;
  justify-content: flex-end;
  gap: 5px;
}

.item-image-small {
  width: 50px;
  height: 50px;
  object-fit: cover;
}

@media (max-width: 768px) {
  .controls {
    flex-direction: column;
    align-items: stretch;
  }

  .search-wrapper,
  .buttons-wrapper {
    width: 100%;
  }

  .buttons-wrapper {
    flex-wrap: wrap;
    gap: 5px;
  }

  .buttons-wrapper > * {
    flex: 1 1 calc(50% - 5px);
    min-width: 120px;
  }

  .data-item {
    width: calc(50% - 10px);
  }

  .data-container.table {
    display: block;
    overflow-x: auto;
    white-space: nowrap;
  }

  .data-container.table .actions-column {
    position: relative;
    right: 0;
  }
}

@media (max-width: 480px) {
  .buttons-wrapper > * {
    flex: 1 1 100%;
  }

  .data-item {
    width: 100%;
  }
}
</style>
