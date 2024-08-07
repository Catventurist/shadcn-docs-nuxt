<template>
  <UiCard
    class="[&:not(:first-child)]:mt-5 [&:not(:last-child)]:mb-5 relative overflow-hidden"
    :class="[inGroup && 'rounded-t-none border-none mb-0']"
  >
    <div v-if="!inGroup && filename" class="p-3 border-b flex text-sm font-mono">
      <Icon v-if="icon" :name="icon" class="self-center mr-1.5" />
      {{ filename }}
      <CodeCopy :code="code" class="ml-auto mr-1" />
    </div>

    <span v-if="!filename" class="absolute top-3 right-3">
      <CodeCopy :code="code" />
    </span>
    <div class="pr-9 bg-muted/30">
      <UiScrollArea>
        <div
          class="py-3 text-sm overflow-x-auto"
          :class="[`highlight-${language}`, !filename && 'inline-copy', !language && 'pl-3']"
        >
          <slot />
        </div>
        <ScrollBar orientation="horizontal" />
      </UiScrollArea>
    </div>
  </UiCard>
</template>

<script setup lang="ts">
import type { BuiltinLanguage } from 'shiki';
import ScrollBar from '../ui/scroll-area/ScrollBar.vue';

const props = defineProps({
  code: {
    type: String,
    default: '',
  },
  language: {
    type: String as PropType<BuiltinLanguage>,
    default: null,
  },
  filename: {
    type: String,
    default: null,
  },
  inGroup: {
    type: Boolean,
    default: false,
  },
  highlights: {
    type: Array as () => number[],
    default: () => [],
  },
});

const iconMap = new Map(Object.entries(useConfig().value.main.codeIcon));
const icon = iconMap.get(props.filename?.toLowerCase()) || iconMap.get(props.language);
</script>

<style>
.fade-enter-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.shiki .line.highlight {
  background-color: hsl(var(--muted) / 0.8);
}

.shiki .line {
  padding-left: 0.75rem;
  padding-right: 0.75rem;
}
</style>
