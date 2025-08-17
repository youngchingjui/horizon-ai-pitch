<script setup lang="ts">
import { onMounted, ref, onBeforeUnmount } from "vue";
import rough from "roughjs/bundled/rough.esm.js";

const props = withDefaults(
  defineProps<{
    peaks?: number;
    lineColor?: string;
    baselineColor?: string;
    enhancedColor?: string;
    background?: string;
    seed?: number; // make drawing deterministic across renders
    showTitle?: boolean; // optionally render internal title
  }>(),
  {
    peaks: 12,
    lineColor: "#2563eb", // blue-600
    baselineColor: "#6b7280", // gray-500
    enhancedColor: "#16a34a", // green-600
    background: "transparent",
    seed: 42,
    showTitle: true,
  }
);

const canvasRef = ref<HTMLCanvasElement | null>(null);
let cleanup: (() => void) | null = null;

// simple seeded PRNG (LCG)
function makeRng(seed: number) {
  let s = Math.imul(48271, (seed % 2147483647)) % 2147483647;
  return function rand() {
    s = (s * 48271) % 2147483647;
    return (s - 1) / 2147483646;
  };
}

function draw() {
  const canvas = canvasRef.value;
  if (!canvas) return;

  const dpr = window.devicePixelRatio || 1;
  const parent = canvas.parentElement;
  if (!parent) return;

  const width = Math.min(parent.clientWidth, 1000);
  const height = Math.max(Math.round(width * 0.45), 260);

  // size canvas
  canvas.width = Math.round(width * dpr);
  canvas.height = Math.round(height * dpr);
  canvas.style.width = width + "px";
  canvas.style.height = height + "px";

  const ctx = canvas.getContext("2d")!;
  ctx.setTransform(1, 0, 0, 1, 0, 0); // reset transform in case of redraws
  ctx.scale(dpr, dpr);

  // background
  ctx.fillStyle = props.background;
  ctx.fillRect(0, 0, width, height);

  const rc = rough.canvas(canvas);

  // padding
  const pad = 32;
  const left = pad;
  const right = width - pad;
  const top = pad;
  const bottom = height - pad;

  // baselines
  const midY = top + (bottom - top) * 0.55;
  const plusY = top + (bottom - top) * 0.3;

  rc.line(left, midY, right, midY, {
    stroke: props.baselineColor,
    strokeWidth: 2,
    roughness: 0.6,
    bowing: 0.3,
  });

  rc.line(left, plusY, right, plusY, {
    stroke: props.enhancedColor,
    strokeWidth: 2,
    roughness: 0.6,
    bowing: 0.3,
    dash: [8, 6],
  });

  // labels
  ctx.font =
    "14px ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Helvetica Neue, Arial";
  ctx.fillStyle = props.baselineColor;
  ctx.textBaseline = "middle";
  ctx.fillText("Human without AI", left + 6, midY - 12);

  ctx.fillStyle = props.enhancedColor;
  ctx.fillText("Human + AI (improved average)", left + 6, plusY - 12);

  // seeded randomness for reproducible jagged line
  const rand = makeRng(props.seed || 42);

  // generate jagged points representing AI experiences
  const n = Math.max(8, props.peaks);
  const pts: [number, number][] = [];
  const cycles = Math.max(4, Math.floor(n / 2)); // more cycles -> more highs/lows
  for (let i = 0; i <= n; i++) {
    const t = i / n;
    const x = left + (right - left) * t;
    // higher-frequency sin wave + seeded randomness for a jaggier line
    const base = Math.sin(t * Math.PI * cycles);
    const noise = (rand() - 0.5) * 0.8; // moderate noise for readability
    const v = base * 0.95 + noise;
    const y = midY - v * (bottom - top) * 0.45;
    pts.push([x, y]);
  }

  // draw jagged polyline
  for (let i = 0; i < pts.length - 1; i++) {
    const [x1, y1] = pts[i];
    const [x2, y2] = pts[i + 1];
    rc.line(x1, y1, x2, y2, {
      stroke: props.lineColor,
      strokeWidth: 2.5,
      roughness: 1.1,
      bowing: 1.3,
    });
  }

  // helper: detect local peaks and troughs
  const locals = pts.map((p, idx) => ({ idx, x: p[0], y: p[1], type: "flat" as "peak" | "trough" | "flat" }));
  for (let i = 1; i < pts.length - 1; i++) {
    const yPrev = pts[i - 1][1];
    const yCurr = pts[i][1];
    const yNext = pts[i + 1][1];
    // remember: smaller y is visually higher (peak)
    if (yCurr < yPrev && yCurr < yNext) locals[i].type = "peak";
    if (yCurr > yPrev && yCurr > yNext) locals[i].type = "trough";
  }

  const peaksOnly = locals.filter((l) => l.type === "peak");
  const troughsOnly = locals.filter((l) => l.type === "trough");

  // find the highest peak (smallest y)
  const bestPeak = peaksOnly.sort((a, b) => a.y - b.y)[0];
  // find a couple deepest troughs (largest y)
  const worstTroughs = troughsOnly.sort((a, b) => b.y - a.y).slice(0, 2);

  // draw small markers on alternating points for visual texture
  pts.forEach(([x, y], idx) => {
    if (idx % 2 === 0) {
      rc.circle(x, y, 6, {
        fill: props.lineColor,
        fillStyle: "solid",
        stroke: "transparent",
      });
    }
  });

  // utility to draw a rough circle highlight and a callout text
  function drawCallout(
    x: number,
    y: number,
    text: string,
    options: { side?: "left" | "right"; color?: string } = {}
  ) {
    const side = options.side ?? (x < (left + right) / 2 ? "right" : "left");
    const color = options.color ?? "#111827";

    // circle highlight
    rc.circle(x, y, 28, {
      stroke: color,
      strokeWidth: 2,
      roughness: 1.2,
      fill: "transparent",
    });

    // connector line
    const dx = side === "right" ? 60 : -60;
    const anchorX = x + dx;
    const anchorY = y + (y < midY ? -24 : 24);
    rc.line(x + (dx > 0 ? 14 : -14), y, anchorX, anchorY, {
      stroke: color,
      strokeWidth: 1.8,
      roughness: 1,
    });

    // text
    ctx.fillStyle = color;
    ctx.font =
      "13px ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Helvetica Neue, Arial";
    ctx.textBaseline = "top";
    const tx = side === "right" ? anchorX + 8 : anchorX - 8;
    const maxWidth = Math.min(260, right - left - 40);
    const words = text.split(" ");
    let line = "";
    let ty = anchorY - 10;
    for (let i = 0; i < words.length; i++) {
      const test = line + (line ? " " : "") + words[i];
      const w = ctx.measureText(test).width;
      if (w > maxWidth && line) {
        ctx.fillText(line, tx + (side === "right" ? 0 : -ctx.measureText(line).width), ty);
        line = words[i];
        ty += 16;
      } else {
        line = test;
      }
    }
    if (line) {
      ctx.fillText(line, tx + (side === "right" ? 0 : -ctx.measureText(line).width), ty);
    }
  }

  // annotations: positive at a high peak
  if (bestPeak) {
    drawCallout(bestPeak.x, bestPeak.y, "Wow, that saved me a lot of time.", {
      side: "right",
      color: "#065f46", // deep green text
    });
  }

  // annotations: negatives at deep troughs
  if (worstTroughs[0]) {
    drawCallout(
      worstTroughs[0].x,
      worstTroughs[0].y,
      "I could have done this faster myself.",
      { side: "left", color: "#7f1d1d" }
    );
  }
  if (worstTroughs[1]) {
    drawCallout(
      worstTroughs[1].x,
      worstTroughs[1].y,
      "It didn't understand what I was trying to do.",
      { side: "right", color: "#7f1d1d" }
    );
  }

  if (props.showTitle) {
    // title
    ctx.fillStyle = "#111827";
    ctx.font =
      "600 18px ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Helvetica Neue, Arial";
    ctx.fillText(
      "Jagged Intelligence: Today's AI user experience",
      left,
      top - 6 + 8
    );
  }

  // Removed the small key (highs/lows) per request
}

onMounted(() => {
  draw();
  const ro = new ResizeObserver(() => draw());
  if (canvasRef.value?.parentElement) ro.observe(canvasRef.value.parentElement);
  cleanup = () => ro.disconnect();
});

onBeforeUnmount(() => {
  cleanup?.();
});
</script>

<template>
  <div style="width: 100%">
    <canvas ref="canvasRef" />
  </div>
</template>

