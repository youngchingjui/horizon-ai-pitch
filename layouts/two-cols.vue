<template>
  <div class="slidev-layout two-cols">
    <div v-if="$slots.header" class="mb-6">
      <slot name="header" />
    </div>
    <div class="grid grid-cols-2 gap-8 items-start text-sm">
      <div>
        <slot />
      </div>
      <div>
        <slot name="right" />
      </div>
    </div>
  </div>
</template>
