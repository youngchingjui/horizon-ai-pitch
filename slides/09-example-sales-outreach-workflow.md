# Spotlight Case Study: Targeted Sales Outreach

<small class="opacity-80">AI‑assisted research → value mapping → 3‑touch drafts with human approval. Designed to reduce prep time and lift reply rates.</small>

<div class="grid grid-cols-2 gap-6 mt-4">
  <!-- Left: Workflow steps (compressed) -->
  <div class="space-y-3">
    <div class="rounded-lg border bg-white/80 p-4">
      <div class="text-[11px] font-semibold tracking-wide text-gray-600">Step 1</div>
      <div class="font-semibold">Trigger & Target List</div>
      <div class="text-xs opacity-80 mt-1">Pull ICP‑matched contacts from CRM; optionally filter by recent intent (site visits, events).</div>
    </div>
    <div class="rounded-lg border bg-white/80 p-4">
      <div class="text-[11px] font-semibold tracking-wide text-gray-600">Step 2</div>
      <div class="font-semibold">Research + Value Mapping</div>
      <div class="text-xs opacity-80 mt-1">Crawl site/news + enrich; generate brief with 2–3 pain hypotheses per persona → map to compliant value statements + proof points.</div>
    </div>
    <div class="rounded-lg border bg-white/80 p-4">
      <div class="text-[11px] font-semibold tracking-wide text-gray-600">Step 3</div>
      <div class="font-semibold">Personalized Drafts (3‑touch)</div>
      <div class="text-xs opacity-80 mt-1">T1: opener + 1 value + ask • T2: insight/resource • T3: soft bump. Localize tone by region/persona.</div>
    </div>
    <div class="rounded-lg border bg-white/80 p-4">
      <div class="text-[11px] font-semibold tracking-wide text-gray-600">Step 4</div>
      <div class="font-semibold">AE Review & Send</div>
      <div class="text-xs opacity-80 mt-1">Human‑in‑the‑loop queue for quick edits/approval → push to Outreach/HubSpot sequences with tracking + CRM write‑back.</div>
    </div>
  </div>

  <!-- Right: How AI helps, Why now, Risks & mitigations -->
  <div class="space-y-3">
    <div class="rounded-lg border bg-white/85 p-4">
      <div class="font-semibold">How AI helps (key levers)</div>
      <ul class="list-disc list-inside text-xs opacity-90 mt-2">
        <li>Faster research: summarize company signals into a brief with citations.</li>
        <li>Better fit: map pains → capabilities → outcomes consistently.</li>
        <li>Personalization at scale: on‑brand drafts that AEs can approve in seconds.</li>
        <li>Operational telemetry: automatic length/tone/spam checks before send.</li>
      </ul>
    </div>

    <div class="rounded-lg border bg-white/85 p-4">
      <div class="font-semibold">Why now</div>
      <ul class="list-disc list-inside text-xs opacity-90 mt-2">
        <li>Model quality: current LLMs handle controlled personalization reliably when grounded with retrieval.</li>
        <li>Data readiness: CRM + web/news + case KB are sufficient to start small.</li>
        <li>Tooling: mature email sequence platforms + APIs for tight integration.</li>
      </ul>
    </div>

    <div class="rounded-lg border bg-white/85 p-4">
      <div class="font-semibold">Backlash & mitigations</div>
      <ul class="list-disc list-inside text-xs opacity-90 mt-2">
        <li>"It’ll sound generic" → Retrieval‑augmented prompts + citation checks; require AE approval.</li>
        <li>Compliance concerns → Approved claims library; region‑specific legal footers.</li>
        <li>Data/privacy worries → Only business‑contact data; honor do‑not‑contact flags.</li>
        <li>Quality drift over time → Automatic lint checks + periodic sample reviews.</li>
      </ul>
    </div>
  </div>
</div>

<div class="mt-3 text-[11px] opacity-75">
  Pilot scope: 1 persona × 1 region × 50 accounts for 2 weeks → compare reply/meeting rates vs. baseline. If lift ≥ +20% or prep time ↓ by 50%+, replicate pattern across 10 use cases (2 per function).
</div>

