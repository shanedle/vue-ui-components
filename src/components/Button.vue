<template>
  <component
    :is="tag"
    :class="[
      'custom-button',
      `variant-${variant}`,
      `size-${size}`,
      { 'icon-only': icon && !label },
      { 'button-disabled': disabled },
    ]"
    @click="handleClick"
    v-bind="$attrs"
    :disabled="disabled"
  >
    <i v-if="iconBefore" :class="iconBefore"></i>
    <i v-if="icon && !label" :class="icon"></i>
    <span v-if="label">{{ label }}</span>
    <i v-if="iconAfter" :class="iconAfter"></i>
  </component>
</template>

<script lang="ts">
import { computed, defineComponent, PropType } from "vue";

export type ButtonSize = "xs" | "sm" | "md" | "lg";
export type ButtonVariant = "solid" | "outline" | "ghost" | "link";

export default defineComponent({
  name: "CustomButton",
  props: {
    label: {
      type: String,
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
    icon: {
      type: String,
      default: "",
    },
    variant: {
      type: String as PropType<ButtonVariant>,
      default: "solid",
      validator: (value: ButtonVariant) =>
        ["solid", "outline", "ghost", "link"].includes(value),
    },
    size: {
      type: String as PropType<ButtonSize>,
      default: "md",
      validator: (value: ButtonSize) =>
        ["xs", "sm", "md", "lg"].includes(value),
    },
    disabled: {
      type: Boolean,
      default: false,
    },
    href: {
      type: String,
      default: "",
    },
    target: { type: String, default: "_self" },
  },
  emits: ["click"],
  setup(props, { emit }) {
    const tag = computed(() => (props.href ? "a" : "button"));

    const handleClick = (event: MouseEvent) => {
      if (props.href) {
        event.preventDefault();
        window.open(props.href, props.target || "_self");
      }
      emit("click", event);
    };

    return {
      tag,
      handleClick,
    };
  },
});
</script>

<style scoped>
.custom-button {
  display: inline-flex;
  align-items: center;
  text-decoration: none;
  line-height: 1;
  margin: 0 5px;
  font-weight: 600;
  cursor: pointer;
  transition: all var(--transition-duration);
  color: var(--button-text-color);
}

.custom-button i {
  font-size: 1em;
}

.icon-only {
  display: inline-flex;
  align-items: center;
  justify-content: center;
}

.custom-button:not(.icon-only) i {
  margin-right: 0.5em;
}

.custom-button:not(.icon-only) i:last-child {
  margin-right: 0;
  margin-left: 0.5em;
}

.icon-before i {
  margin-right: 0.5em;
}

.icon-after i {
  margin-left: 0.5em;
}

/* Size variations */
.size-xs {
  font-size: 12px;
  padding: 6px 10px;
}

.size-xs.icon-only {
  width: 24px;
  height: 24px;
  padding: 0;
}

.size-sm {
  font-size: 13px;
  padding: 8px 12px;
}

.size-sm.icon-only {
  width: 32px;
  height: 32px;
  padding: 0;
}

.size-md {
  font-size: 14px;
  padding: 10px 16px;
}

.size-md.icon-only {
  width: 40px;
  height: 40px;
  padding: 0;
}

.size-lg {
  font-size: 16px;
  padding: 12px 20px;
}

.size-lg.icon-only {
  width: 48px;
  height: 48px;
  padding: 0;
}

/* Variant styles */
.variant-solid {
  background-color: var(--primary-color);
  color: var(--primary-button-text-color);
  border: none;
}

.variant-solid:hover {
  background-color: var(--primary-hover-color);
}

.variant-outline {
  background-color: transparent;
  color: var(--primary-color);
  border: 1px solid var(--primary-color);
}

.variant-outline:hover {
  background-color: var(--primary-color);
  color: var(--primary-button-text-color);
}

.variant-ghost {
  background-color: transparent;
  color: var(--primary-color);
  border: none;
}

.variant-ghost:hover {
  background-color: rgba(0, 0, 0, 0.1);
}

.variant-link {
  background-color: transparent;
  color: var(--primary-color);
  border: none;
  padding: 0;
  text-decoration: none;
}

.variant-link:hover {
  text-decoration: underline;
}

.button-disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.dark .variant-ghost:hover {
  background-color: rgba(255, 255, 255, 0.1);
}
</style>
