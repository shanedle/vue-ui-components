<template>
  <div :class="['app', theme]">
    <header>
      <div class="container header-content">
        <h1>Vue UI Components</h1>
        <button
          @click="toggleTheme"
          class="theme-toggle"
          :title="
            theme === 'light' ? 'Switch to Dark Mode' : 'Switch to Light Mode'
          "
        >
          <i :class="theme === 'light' ? 'fas fa-moon' : 'fas fa-sun'"></i>
        </button>
      </div>
    </header>

    <main class="container">
      <section class="component-section">
        <h2>Buttons</h2>
        <div class="component">
          <div class="button-group">
            <h3>Button Variants</h3>
            <CustomButton label="Solid" />
            <CustomButton label="Outline" variant="outline" />
            <CustomButton label="Ghost" variant="ghost" />
            <CustomButton label="Link" variant="link" />
          </div>

          <div class="button-group">
            <h3>Buttons with Icons</h3>
            <CustomButton label="Email" iconBefore="fas fa-envelope" />
            <CustomButton
              label="Call us"
              variant="outline"
              iconAfter="fas fa-phone"
            />
          </div>

          <div class="button-group">
            <h3>Disabled Button</h3>
            <CustomButton label="Disabled" :disabled="true" />
          </div>

          <div class="button-group icon-only-group">
            <h3>Icon-only Buttons</h3>
            <CustomButton icon="fas fa-heart" title="Like" />
            <CustomButton icon="fas fa-share" variant="outline" title="Share" />
            <CustomButton icon="fas fa-trash" variant="ghost" title="Delete" />
          </div>

          <div class="button-group">
            <h3>Button Sizes</h3>
            <CustomButton label="Extra Small" size="xs" />
            <CustomButton label="Small" size="sm" />
            <CustomButton label="Medium" size="md" />
            <CustomButton label="Large" size="lg" />
          </div>

          <div class="button-group">
            <h3>Clickable Buttons</h3>
            <CustomButton label="Solid" @click="handleButtonClick" />
            <CustomButton
              label="Outline"
              variant="outline"
              @click="handleButtonClick"
            />
            <CustomButton
              label="Ghost"
              variant="ghost"
              @click="handleButtonClick"
            />
            <CustomButton
              label="Link"
              href="https://vuejs.org"
              target="_blank"
              variant="link"
            />
          </div>
        </div>
      </section>

      <section class="component-section">
        <h2>Input</h2>
        <div class="component">
          <div class="input-group">
            <h3>Input Sizes</h3>
            <CustomInput placeholder="Extra Small" size="xs" />
            <CustomInput placeholder="Small" size="sm" />
            <CustomInput placeholder="Medium" size="md" />
            <CustomInput placeholder="Large" size="lg" />
          </div>

          <div class="input-group">
            <h3>Input Variants</h3>
            <CustomInput placeholder="Outline" variant="outline" />
            <CustomInput placeholder="Filled" variant="filled" />
            <CustomInput placeholder="Flushed" variant="flushed" />
            <CustomInput placeholder="Unstyled" variant="unstyled" />
          </div>

          <div class="input-group">
            <h3>Input with Left and Right Icons</h3>
            <CustomInput
              placeholder="Enter text..."
              iconBefore="fas fa-user"
              iconAfter="fas fa-check"
            />
          </div>

          <div class="input-group">
            <h3>Controlled Input</h3>
            <p>Value: {{ controlledInputValue }}</p>
            <CustomInput
              v-model="controlledInputValue"
              placeholder="Controlled Input"
              :controlled="true"
            />
          </div>

          <div class="input-group">
            <h3>Input with Button</h3>
            <CustomInput
              v-model="inputWithButton"
              placeholder="Search..."
              buttonLabel="Search"
              iconBefore="fas fa-search"
              @button-click="handleSearch"
              :controlled="true"
            />
          </div>
        </div>
      </section>

      <section class="component-section">
        <h2>Data View</h2>
        <div class="component">
          <DataView
            :data="dummyData"
            @edit="handleEdit"
            @delete="handleDelete"
          />
        </div>
      </section>
    </main>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import CustomButton from "./components/Button.vue";
import CustomInput from "./components/Input.vue";
import DataView from "./components/DataView.vue";

export default defineComponent({
  name: "App",
  components: {
    CustomButton,
    CustomInput,
    DataView,
  },
  setup() {
    const controlledInputValue = ref("");
    const inputWithButton = ref("");
    const theme = ref("light");

    const dummyData = ref([
      {
        id: 1,
        name: "Item 1",
        description: "This is the first item",
        image: "https://picsum.photos/200/300?random=1",
      },
      {
        id: 2,
        name: "Item 2",
        description: "This is the second item",
        image: "https://picsum.photos/200/300?random=2",
      },
      {
        id: 3,
        name: "Item 3",
        description: "This is the third item",
        image: "https://picsum.photos/200/300?random=3",
      },
    ]);

    const toggleTheme = () => {
      theme.value = theme.value === "light" ? "dark" : "light";
      document.body.classList.toggle("dark", theme.value === "dark");
    };

    const handleButtonClick = () => {
      alert("Button clicked!");
    };

    const handleSearch = () => {
      alert(`Searching for: ${inputWithButton.value}`);
    };

    const handleEdit = (item: any) => {
      alert(`Editing item: ${item.name}`);
    };

    const handleDelete = (item: any) => {
      alert(`Deleting item: ${item.name}`);
    };

    return {
      controlledInputValue,
      inputWithButton,
      dummyData,
      theme,
      toggleTheme,
      handleButtonClick,
      handleSearch,
      handleEdit,
      handleDelete,
    };
  },
});
</script>

<style>
:root {
  --primary-color: #000000;
  --primary-hover-color: #333333;
  --background-color: #ffffff;
  --text-color: #333333;
  --text-color-secondary: #666666;
  --border-color: #e0e0e0;
  --selected-background: #e6e6e6;
  --input-background: #ffffff;
  --button-text-color: #ffffff;
  --primary-button-text-color: #ffffff;
  --filled-background-color-light: #f2f2f2;
  --filled-hover-background-color-light: #e6e6e6;
  --filled-text-color-light: #333333;
  --border-radius: 4px;
  --box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  --transition-duration: 0.3s;
}

.dark {
  --primary-color: #ffffff;
  --primary-hover-color: #cccccc;
  --background-color: #333333;
  --text-color: #ffffff;
  --text-color-secondary: #cccccc;
  --border-color: #555555;
  --selected-background: #555555;
  --input-background: #444444;
  --primary-button-text-color: #000000;
  --filled-background-color-dark: #333333;
  --filled-hover-background-color-dark: #404040;
  --filled-text-color-dark: #ffffff;
}

body {
  margin: 0;
  padding: 0;
  font-family: Arial, sans-serif;
  font-size: 14px;
  line-height: 1.5;
}

.app {
  background-color: var(--background-color);
  color: var(--text-color);
  min-height: 100vh;
  transition: background-color var(--transition-duration),
    color var(--transition-duration);
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

header {
  padding: 0.5rem;
  border-bottom: 1px solid var(--border-color);
}

.header-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h1,
h2,
h3 {
  margin: 0;
  color: var(--text-color);
}

h1 {
  font-size: 20px;
}

h2 {
  font-size: 18px;
  border-bottom: 1px solid var(--border-color);
  padding-bottom: 0.5rem;
  margin-bottom: 1rem;
}

h3 {
  font-size: 16px;
  margin-top: 1rem;
  margin-bottom: 0.5rem;
}

.theme-toggle {
  background: none;
  border: none;
  font-size: 1.2rem;
  cursor: pointer;
  color: var(--text-color);
  padding: 5px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: color var(--transition-duration);
}

.theme-toggle:hover {
  color: var(--primary-color);
  background-color: rgba(0, 0, 0, 0.1);
}

main {
  padding: 2rem;
}

.component-section {
  margin-bottom: 2rem;
}

.component-section:first-child {
  margin-top: 2rem;
}

.component {
  border: 1px solid var(--border-color);
  padding: 1.5rem;
  margin-top: 1rem;
}

.button-group {
  margin-bottom: 1rem;
}

.button-group h3 {
  margin-bottom: 0.5rem;
}

.input-group {
  margin-bottom: 1rem;
}

.input-group h3 {
  margin-bottom: 0.5rem;
}

@media (max-width: 768px) {
  .container {
    padding: 0 15px;
  }

  main {
    padding: 1.5rem;
  }

  .component-section:first-child {
    margin-top: 1.5rem;
  }
}

@media (max-width: 480px) {
  h1 {
    font-size: 18px;
  }

  h2 {
    font-size: 16px;
  }

  h3 {
    font-size: 14px;
  }

  .component {
    padding: 1rem;
  }

  .component-section:first-child {
    margin-top: 1rem;
  }
}
</style>
