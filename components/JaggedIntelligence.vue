<script setup lang="ts">
import { onMounted, ref, onBeforeUnmount } from 'vue'
import rough from 'roughjs/bundled/rough.esm.js'

const props = withDefaults(defineProps<{
  peaks?: number
  lineColor?: string
  baselineColor?: string
  enhancedColor?: string
  background?: string
}>(), {
  peaks: 8,
  lineColor: '#2563eb', // blue-600
  baselineColor: '#6b7280', // gray-500
  enhancedColor: '#16a34a', // green-600
  background: 'transparent',
})

const canvasRef = ref<HTMLCanvasElement | null>(null)
let cleanup: (() => void) | null = null

function draw() {
  const canvas = canvasRef.value
  if (!canvas) return

  const dpr = window.devicePixelRatio || 1
  const parent = canvas.parentElement
  if (!parent) return

  const width = Math.min(parent.clientWidth, 1000)
  const height = Math.max(Math.round(width * 0.45), 260)

  // size canvas
  canvas.width = Math.round(width * dpr)
  canvas.height = Math.round(height * dpr)
  canvas.style.width = width + 'px'
  canvas.style.height = height + 'px'

  const ctx = canvas.getContext('2d')!
  ctx.scale(dpr, dpr)

  // background
  ctx.fillStyle = props.background
  ctx.fillRect(0, 0, width, height)

  const rc = rough.canvas(canvas)

  // padding
  const pad = 32
  const left = pad
  const right = width - pad
  const top = pad
  const bottom = height - pad

  // baselines
  const midY = top + (bottom - top) * 0.55
  const plusY = top + (bottom - top) * 0.30

  rc.line(left, midY, right, midY, {
    stroke: props.baselineColor,
    strokeWidth: 2,
    roughness: 0.6,
    bowing: 0.3,
  })

  rc.line(left, plusY, right, plusY, {
    stroke: props.enhancedColor,
    strokeWidth: 2,
    roughness: 0.6,
    bowing: 0.3,
    dash: [8,6],
  })

  // labels
  ctx.font = '14px ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Helvetica Neue, Arial'
  ctx.fillStyle = props.baselineColor
  ctx.textBaseline = 'middle'
  ctx.fillText('Human without AI', left + 6, midY - 12)

  ctx.fillStyle = props.enhancedColor
  ctx.fillText('Human + AI (improved average)', left + 6, plusY - 12)

  // generate jagged points representing AI experiences
  const n = Math.max(4, props.peaks)
  const pts: [number, number][] = []
  for (let i = 0; i <= n; i++) {
    const t = i / n
    const x = left + (right - left) * t
    // sin wave base + randomness
    const base = Math.sin(t * Math.PI * 3) // a few ups/downs
    const noise = (Math.random() - 0.5) * 0.6
    const v = base * 0.9 + noise
    const y = midY - v * (bottom - top) * 0.45
    pts.push([x, y])
  }

  // draw jagged polyline
  for (let i = 0; i < pts.length - 1; i++) {
    const [x1, y1] = pts[i]
    const [x2, y2] = pts[i + 1]
    rc.line(x1, y1, x2, y2, {
      stroke: props.lineColor,
      strokeWidth: 2.5,
      roughness: 1.2,
      bowing: 1.5,
    })
  }

  // draw markers on peaks/troughs (every other point)
  pts.forEach(([x, y], idx) => {
    if (idx % 2 === 0) {
      rc.circle(x, y, 6, {
        fill: props.lineColor,
        fillStyle: 'solid',
        stroke: 'transparent',
      })
    }
  })

  // title
  ctx.fillStyle = '#111827'
  ctx.font = '600 18px ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Helvetica Neue, Arial'
  ctx.fillText('Jagged Intelligence: Today\'s AI user experience', left, top - 6 + 8)

  // annotations
  ctx.font = '13px ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Helvetica Neue, Arial'
  ctx.fillStyle = '#374151'
  ctx.fillText('Highs: great experiences', left + 8, top + 24)
  ctx.fillText('Lows: disappointing experiences', left + 8, top + 44)
}

onMounted(() => {
  draw()
  const ro = new ResizeObserver(() => draw())
  if (canvasRef.value?.parentElement)
    ro.observe(canvasRef.value.parentElement)
  cleanup = () => ro.disconnect()
})

onBeforeUnmount(() => {
  cleanup?.()
})
</script>

<template>
  <div style="width: 100%;">
    <canvas ref="canvasRef"/>
  </div>
</template>

