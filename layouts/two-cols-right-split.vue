<template>
  <div class="slidev-layout two-cols-right-split">
    <div v-if="$slots.header" class="mb-6">
      <slot name="header" />
    </div>
    <div class="grid grid-cols-2 gap-8 items-stretch text-sm">
      <div>
        <slot />
      </div>
      <div class="flex flex-col gap-8 h-full">
        <div class="flex-1">
          <slot name="right-top" />
        </div>
        <div class="flex-1">
          <slot name="right-bottom" />
        </div>
      </div>
    </div>
  </div>
</template>
