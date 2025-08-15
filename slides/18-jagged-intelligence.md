# Jagged Intelligence — Human x AI

<small>The lived experience of AI is jagged: moments of brilliance and moments of disappointment. Our goal is to raise and stabilize outcomes above the human-only baseline.</small>

<div style="margin-top: 16px; background: white; padding: 20px; border-radius: 12px; box-shadow: 0 8px 24px rgba(0,0,0,0.12);">
  <svg viewBox="0 0 800 420" width="100%" height="auto" role="img" aria-label="Infographic showing jagged AI experiences, human baseline, and improved Human+AI trajectory">
    <defs>
      <linearGradient id="grad-ai" x1="0" x2="0" y1="0" y2="1">
        <stop offset="0%" stop-color="#3b82f6" stop-opacity="0.15"/>
        <stop offset="100%" stop-color="#3b82f6" stop-opacity="0"/>
      </linearGradient>
    </defs>

    <!-- Padding -->
    <g transform="translate(60,20)">
      <!-- Axes -->
      <line x1="0" y1="360" x2="720" y2="360" stroke="#9CA3AF" stroke-width="1"/>
      <line x1="0" y1="0" x2="0" y2="360" stroke="#9CA3AF" stroke-width="1"/>

      <!-- Axis labels -->
      <text x="360" y="400" text-anchor="middle" font-size="14" fill="#374151">Interactions / Tasks</text>
      <text x="-28" y="180" text-anchor="middle" font-size="14" fill="#374151" transform="rotate(-90 -28,180)">Capability / Outcome</text>

      <!-- Human baseline (flat) -->
      <line x1="0" y1="220" x2="720" y2="220" stroke="#6B7280" stroke-dasharray="6 6" stroke-width="2"/>
      <text x="12" y="210" font-size="12" fill="#6B7280">Human without AI (baseline)</text>

      <!-- Jagged AI experiences -->
      <!-- Points chosen to show peaks and troughs around the baseline -->
      <polyline points="0,260 60,120 120,280 180,160 240,320 300,140 360,260 420,120 480,300 540,180 600,260 660,140 720,280"
                fill="url(#grad-ai)" stroke="#3B82F6" stroke-width="3"/>
      <text x="520" y="110" font-size="12" fill="#1F2937" background="white">Jagged AI experience (today)</text>

      <!-- Human + AI (raised and smoothed) -->
      <path d="M0,230 C120,200 180,200 240,190 C300,180 360,180 420,170 C480,165 600,155 720,150" stroke="#10B981" stroke-width="3" fill="none"/>
      <text x="480" y="160" font-size="12" fill="#065F46">Human + AI (raised, more consistent)</text>

      <!-- Markers on select peaks/troughs -->
      <g fill="#3B82F6" stroke="white" stroke-width="2">
        <circle cx="60" cy="120" r="5"/>
        <circle cx="120" cy="280" r="5"/>
        <circle cx="300" cy="140" r="5"/>
        <circle cx="480" cy="300" r="5"/>
        <circle cx="660" cy="140" r="5"/>
      </g>

      <!-- Legend -->
      <g transform="translate(440,8)">
        <rect x="0" y="0" width="250" height="60" rx="8" ry="8" fill="#F9FAFB" stroke="#E5E7EB"/>
        <line x1="14" y1="18" x2="46" y2="18" stroke="#6B7280" stroke-width="3" stroke-dasharray="6 6"/>
        <text x="60" y="22" font-size="12" fill="#374151">Human without AI</text>
        <line x1="14" y1="38" x2="46" y2="38" stroke="#10B981" stroke-width="3"/>
        <text x="60" y="42" font-size="12" fill="#065F46">Human + AI</text>
      </g>

      <!-- Callouts -->
      <g>
        <text x="70" y="110" font-size="12" fill="#1F2937">Great outcome</text>
        <text x="110" y="300" font-size="12" fill="#1F2937">Disappointing</text>
      </g>
    </g>
  </svg>
</div>

Notes:
- The blue jagged line represents volatile outcomes from current AI usage (peaks and troughs).
- The dashed gray line is the human-only baseline.
- The green curve shows our goal: raise and stabilize outcomes with training, tooling, and guardrails.

