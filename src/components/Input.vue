<template>
  <div
    :class="[
      'custom-input-wrapper',
      `size-${size}`,
      variant,
      { 'has-button': hasButton },
    ]"
  >
    <i v-if="iconBefore" :class="['input-icon', 'icon-before', iconBefore]"></i>
    <div v-if="prefix" class="input-affix prefix">{{ prefix }}</div>
    <input
      :value="modelValue"
      @input="handleInput"
      @blur="handleBlur"
      v-bind="$attrs"
      :class="[
        'custom-input',
        `size-${size}`,
        variant,
        { 'has-icon-before': iconBefore, 'has-icon-after': iconAfter },
      ]"
      ref="inputRef"
    />
    <div v-if="suffix" class="input-affix suffix">{{ suffix }}</div>
    <i v-if="iconAfter" :class="['input-icon', 'icon-after', iconAfter]"></i>
    <CustomButton
      v-if="hasButton"
      :label="buttonLabel"
      :variant="buttonVariant"
      @click="$emit('button-click')"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent, computed, PropType, ref, Ref } from "vue";
import CustomButton, { ButtonVariant } from "./Button.vue";

export type InputSize = "xs" | "sm" | "md" | "lg";
export type InputVariant = "outline" | "unstyled" | "flushed" | "filled";

export default defineComponent({
  name: "CustomInput",
  components: {
    CustomButton,
  },
  props: {
    modelValue: {
      type: [String, Number],
      default: "",
    },
    iconBefore: {
      type: String,
      default: "",
    },
    iconAfter: {
      type: String,
      default: "",
    },
    prefix: {
      type: String,
      default: "",
    },
    suffix: {
      type: String,
      default: "",
    },
    buttonLabel: {
      type: String,
      default: "",
    },
    buttonVariant: {
      type: String as PropType<ButtonVariant>,
      default: "solid",
    },
    lazy: {
      type: Boolean,
      default: false,
    },
    size: {
      type: String as PropType<InputSize>,
      default: "md",
    },
    variant: {
      type: String as PropType<InputVariant>,
      default: "outline",
    },
    controlled: {
      type: Boolean,
      default: false,
    },
  },
  emits: ["update:modelValue", "button-click"],
  setup(props, { emit }) {
    const hasButton = computed(() => !!props.buttonLabel);
    const inputRef: Ref<HTMLInputElement | null> = ref(null);

    const handleInput = (event: Event) => {
      const target = event.target as HTMLInputElement;
      if (!props.lazy) {
        emit("update:modelValue", target.value);
      }
    };

    const handleBlur = (event: Event) => {
      const target = event.target as HTMLInputElement;
      if (props.lazy) {
        emit("update:modelValue", target.value);
      }
    };

    return {
      hasButton,
      inputRef,
      handleInput,
      handleBlur,
    };
  },
});
</script>

<style scoped>
.custom-input-wrapper {
  display: flex;
  align-items: stretch;
  position: relative;
  width: 100%;
  margin-bottom: 10px;
}

.custom-input {
  flex-grow: 1;
  padding: 10px 12px;
  border: 1px solid var(--border-color);
  background-color: var(--input-background);
  color: var(--text-color);
  font-size: 14px;
  line-height: 1.5;
  box-sizing: border-box;
  transition: border-color var(--transition-duration),
    background-color var(--transition-duration);
}

.custom-input:focus {
  outline: none;
  border-color: var(--primary-color);
}

.input-icon {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  font-size: 16px;
  color: var(--text-color-secondary);
  z-index: 1;
}

.icon-before {
  left: 12px;
}

.icon-after {
  right: 12px;
}

.custom-input.has-icon-before {
  padding-left: 36px;
}

.custom-input.has-icon-after {
  padding-right: 36px;
}

.input-affix {
  padding: 0 12px;
  color: var(--text-color-secondary);
  display: flex;
  align-items: center;
  border: 1px solid var(--border-color);
}

.prefix {
  border-right: none;
}

.suffix {
  border-left: none;
}

.has-button .custom-button {
  margin-left: -1px;
}

.has-button {
  display: flex;
  align-items: stretch;
}

.has-button .custom-input {
  flex: 1;
}

/* Input sizes */
.size-xs .custom-input {
  height: 24px;
  font-size: 12px;
  padding: 2px 8px;
}

.size-sm .custom-input {
  height: 32px;
  font-size: 14px;
  padding: 4px 10px;
}

.size-md .custom-input {
  height: 40px;
  font-size: 16px;
  padding: 6px 12px;
}

.size-lg .custom-input {
  height: 48px;
  font-size: 18px;
  padding: 8px 16px;
}

/* Input variants */
.outline .custom-input {
  border: 1px solid var(--border-color);
  background-color: transparent;
}

.unstyled .custom-input {
  border: none;
  background: none;
  padding-left: 0;
  padding-right: 0;
}

.flushed .custom-input {
  border: none;
  border-bottom: 2px solid var(--border-color);
  border-radius: 0;
  background-color: transparent;
  padding-left: 0;
  padding-right: 0;
}

.flushed .custom-input:focus {
  border-bottom-color: var(--primary-color);
}

.filled .custom-input {
  border: none;
  background-color: var(--filled-background-color-light);
  color: var(--filled-text-color-light);
  border-radius: 4px;
}

.filled .custom-input:hover {
  background-color: var(--filled-hover-background-color-light);
}

.dark-mode .filled .custom-input {
  background-color: var(--filled-background-color-dark);
  color: var(--filled-text-color-dark);
}

.dark-mode .filled .custom-input:hover {
  background-color: var(--filled-hover-background-color-dark);
}

.filled .custom-input:focus {
  color: var(--text-color);
  outline: 2px solid var(--primary-color);
  outline-offset: -2px;
  background-color: transparent;
}

.size-xs .icon-before {
  left: 8px;
}
.size-xs .icon-after {
  right: 8px;
}
.size-xs .custom-input.has-icon-before {
  padding-left: 28px;
}
.size-xs .custom-input.has-icon-after {
  padding-right: 28px;
}

.size-sm .icon-before {
  left: 10px;
}
.size-sm .icon-after {
  right: 10px;
}
.size-sm .custom-input.has-icon-before {
  padding-left: 32px;
}
.size-sm .custom-input.has-icon-after {
  padding-right: 32px;
}

.size-md .icon-before {
  left: 12px;
}
.size-md .icon-after {
  right: 12px;
}
.size-md .custom-input.has-icon-before {
  padding-left: 36px;
}
.size-md .custom-input.has-icon-after {
  padding-right: 36px;
}

.size-lg .icon-before {
  left: 16px;
}
.size-lg .icon-after {
  right: 16px;
}
.size-lg .custom-input.has-icon-before {
  padding-left: 44px;
}
.size-lg .custom-input.has-icon-after {
  padding-right: 44px;
}

.flushed .custom-input.has-icon-before {
  padding-left: 24px;
}

.flushed .icon-before {
  left: 0;
}

.filled .custom-input.has-icon-before {
  padding-left: 36px;
}

.filled .icon-before {
  left: 12px;
}

.flushed .custom-input.has-icon-after {
  padding-right: 24px;
}

.flushed .icon-after {
  right: 0;
}

.filled .custom-input.has-icon-after {
  padding-right: 36px;
}

.filled .icon-after {
  right: 12px;
}
</style>
