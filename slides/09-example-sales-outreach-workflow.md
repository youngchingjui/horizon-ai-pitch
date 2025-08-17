# Example AI Workflow: Targeted Sales Outreach

<small class="opacity-80">Deep customer research + personalized multi-touch emails. Inspired by approaches seen in platforms like Relevance AI.</small>

<div class="grid grid-cols-2 gap-6 mt-4">
  <!-- Left: Workflow steps -->
  <div class="space-y-3">
    <div class="rounded-lg border bg-white/80 p-4">
      <div class="text-xs font-semibold tracking-wide text-gray-600">Step 1</div>
      <div class="font-semibold">Trigger & Target List</div>
      <div class="text-sm opacity-80 mt-1">Pull ICP-matched accounts/contacts from CRM (e.g., industry, size, region). Optionally seed with recent intent signals (site visits, event scans).</div>
    </div>
    <div class="rounded-lg border bg-white/80 p-4">
      <div class="text-xs font-semibold tracking-wide text-gray-600">Step 2</div>
      <div class="font-semibold">Deep Company/Persona Research</div>
      <div class="text-sm opacity-80 mt-1">For each account: crawl/about pages, news, careers, product docs; enrich with third-party data. Create a concise company brief + 3 pain hypotheses per persona.</div>
    </div>
    <div class="rounded-lg border bg-white/80 p-4">
      <div class="text-xs font-semibold tracking-wide text-gray-600">Step 3</div>
      <div class="font-semibold">Value Mapping</div>
      <div class="text-sm opacity-80 mt-1">Map pains → product capabilities → outcome statements. Add compliant proof points and relevant case links.</div>
    </div>
    <div class="rounded-lg border bg-white/80 p-4">
      <div class="text-xs font-semibold tracking-wide text-gray-600">Step 4</div>
      <div class="font-semibold">Personalized Drafts (3-touch)</div>
      <div class="text-sm opacity-80 mt-1">Generate T1: opener + 1 value and ask; T2: insight or resource; T3: soft bump. Adapt tone by persona and region (localization optional).</div>
    </div>
    <div class="rounded-lg border bg-white/80 p-4">
      <div class="text-xs font-semibold tracking-wide text-gray-600">Step 5</div>
      <div class="font-semibold">AE Review & Send</div>
      <div class="text-sm opacity-80 mt-1">Human-in-the-loop approval in a queue UI; quick edits; then handoff to Outreach/HubSpot/Marketo sequences with tracking params.</div>
    </div>
  </div>

  <!-- Right: System design & guardrails -->
  <div class="space-y-3">
    <div class="rounded-lg border bg-white/80 p-4">
      <div class="font-semibold">System Blocks (simplified)</div>
      <ul class="list-disc list-inside text-sm opacity-90 mt-2">
        <li>Inputs: CRM records, enrichment APIs, website/news scraping, case studies KB.</li>
        <li>Retrieval: vector search over briefs/cases (e.g., via Relevance AI or in-house embeddings).</li>
        <li>Generation: structured prompts to draft briefs and emails with style guides.</li>
        <li>Orchestration: workflow runner queues jobs, retries, escalates for review.</li>
        <li>Delivery: connect to email sequence tool; write-backs to CRM activities.</li>
      </ul>
    </div>

    <div class="rounded-lg border bg-white/80 p-4">
      <div class="font-semibold">Guardrails</div>
      <ul class="list-disc list-inside text-sm opacity-90 mt-2">
        <li>Hallucination control: retrieval-augmented prompts; cite sources in drafts.</li>
        <li>Compliance: approved claims library; region-specific legal footer/opt-out.</li>
        <li>Privacy: store only business-contact data; respect do-not-contact flags.</li>
        <li>Quality: automatic checks for length, tone, spam risk, personalization depth.</li>
      </ul>
    </div>

    <div class="rounded-lg border bg-white/80 p-4">
      <div class="font-semibold">Success Metrics</div>
      <ul class="list-disc list-inside text-sm opacity-90 mt-2">
        <li>Research time saved per account/contact.</li>
        <li>% drafts approved without major edits.</li>
        <li>Open/Reply/Meeting rates vs. baseline sequences.</li>
        <li>Attribution: SQLs and revenue influenced.</li>
      </ul>
    </div>
  </div>
</div>

<div class="mt-4 text-xs opacity-70">
  Notes: This is an example workflow; tools are interchangeable. Start with a small persona + region + 50 accounts, measure, then scale.
</div>

