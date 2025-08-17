<template>
  <div class="slidev-layout deliverables-grid">
    <div v-if="$slots.header" class="mb-4">
      <slot name="header" />
    </div>
    <div class="grid-scroll">
      <!-- 4 columns on desktop, responsive down to 3/2 on smaller viewports -->
      <div class="grid grid-cols-4 gap-4">
        <slot />
      </div>
    </div>
  </div>
</template>

<style>
.deliverables-grid {
  display: flex;
  flex-direction: column;
  height: 100%;
}

/* Ensure the content never gets cut off - it scrolls within the slide if needed */
.deliverables-grid .grid-scroll {
  flex: 1 1 auto;
  min-height: 0; /* allow child to shrink and enable overflow */
  overflow-y: auto;
}

/* Make any direct children of the grid slot look like uniform cards */
.deliverables-grid .grid > * {
  background: #ffffff;
  border: 1px solid rgba(15, 23, 42, 0.08);
  border-radius: 10px;
  padding: 12px 14px;
  line-height: 1.3;
}

/* If authors pass a list, make list items behave like cards too */
.deliverables-grid .grid ul,
.deliverables-grid .grid ol {
  list-style: none;
  padding: 0;
  margin: 0;
  display: contents; /* flatten list so li become direct grid children */
}
.deliverables-grid .grid li {
  background: #ffffff;
  border: 1px solid rgba(15, 23, 42, 0.08);
  border-radius: 10px;
  padding: 12px 14px;
}

@media (max-width: 1024px) {
  .deliverables-grid .grid {
    grid-template-columns: repeat(3, minmax(0, 1fr));
  }
}
@media (max-width: 768px) {
  .deliverables-grid .grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }
}
</style>

