<template>
  <div class="logo-grid" :style="{ '--min': `${minSize}px` }">
    <component
      v-for="(logo, idx) in logos"
      :is="logo.href ? 'a' : 'div'"
      :key="idx"
      class="logo-item"
      :href="logo.href || undefined"
      :target="logo.href ? '_blank' : undefined"
      :rel="logo.href ? 'noopener' : undefined"
      :title="logo.alt || undefined"
    >
      <img :src="logo.src" :alt="logo.alt || ''" loading="lazy" />
    </component>
  </div>
</template>

<script setup lang="ts">
interface LogoItem {
  src: string
  alt?: string
  href?: string
}

const props = withDefaults(defineProps<{
  logos: LogoItem[]
  /** Minimum column width in px before wrapping to next line */
  minSize?: number
}>(), {
  minSize: 80,
})

const { logos, minSize } = toRefs(props)
</script>

<style scoped>
.logo-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(var(--min, 80px), 1fr));
  gap: 16px;
  align-items: center;
}

.logo-item {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 8px;
}

.logo-item img {
  max-width: 100%;
  max-height: 56px;
  height: auto;
  object-fit: contain;
  /* keep them subtle and consistent */
  filter: grayscale(100%);
  opacity: 0.9;
  transition: opacity 0.2s ease, filter 0.2s ease;
}

.logo-item:hover img {
  opacity: 1;
  filter: grayscale(0%);
}

@media (min-width: 640px) {
  .logo-item img { max-height: 60px; }
}

@media (min-width: 1024px) {
  .logo-item img { max-height: 72px; }
}
</style>

