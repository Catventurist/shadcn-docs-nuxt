<template>
  <template v-if="item.links">
    <UiCollapsible v-model:open="isOpen">
      <UiCollapsibleTrigger class="w-full text-left p-2">
        <div class="w-full flex gap-2">
          <Icon v-if="item?.icon" :name="item.icon" class="mt-1" />
          {{ item.title }}
          <Icon :name="isOpen ? 'lucide:chevrons-down-up' : 'lucide:chevrons-up-down'" size="12"
            class="ml-auto self-center" />
        </div>
      </UiCollapsibleTrigger>
      <UiCollapsibleContent>
        <ul class="pl-2">
          <li v-for="link in item.links" :key="link.title">
            <NuxtLink :to="link.to" :target="link.target"
              class="px-3 py-2 mb-1 hover:bg-muted rounded-md w-full block gap-2 transition-all">
              <div class="font-semibold gap-2 flex justify-between">
                {{ link.title }}
                <Icon v-if="link.target === '_blank'" name="lucide:external-link" class="text-muted-foreground "
                  size="13" />
                <Icon v-if="link.icon" :name="link.icon" />
              </div>
              <div class="text-muted-foreground text-sm">
                {{ link.description }}
              </div>
            </NuxtLink>
          </li>
        </ul>
      </UiCollapsibleContent>
    </UiCollapsible>
  </template>
  <NuxtLink v-else :to="item.to" :target="item.target" class="w-full flex p-2">
    <div class="font-semibold gap-2">
      <Icon v-if="item?.icon" :name="item.icon" />
      {{ item.title }}
    </div>
    <Icon name="lucide:arrow-up-right" class="ml-1 text-muted-foreground" size="12" />
  </NuxtLink>
</template>

<script setup lang="ts">
const props = defineProps<{
  item: any;
  index: number;
}>();

const collapsed = useCollapsedMap();
const isOpen = ref(collapsed.value.get(`mobile-header-nav${props.index}`) || false);
watch(isOpen, (v) => {
  collapsed.value.set(`mobile-header-nav${props.index}`, v);
});
</script>
