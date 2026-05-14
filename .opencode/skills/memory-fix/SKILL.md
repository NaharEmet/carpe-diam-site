---
name: memory-fix
description: "Run the memory testing pipeline, analyze bug reports, fix extraction code, and loop until all bugs are resolved. Triggered by phrases like 'fix memory', 'memory bug', 'memory test', 'extraction bug', or 'run memory pipeline'. Slash command: /memory-fix"
user-invokable: true
args:
  - name: mode
    description: "Loop mode: full (test→fix→repeat), analyze-only (just read reports)"
    required: false
  - name: iterations
    description: "Max fix iterations before stopping (default: 5)"
    required: false
---

Self-improving fix loop for Anantha's memory extraction pipeline. Runs tests, reads bug reports, fixes root causes, and re-runs until all bugs are resolved.

## Prerequisites

Before running this skill, ensure:
- Ecto repos are running (`mix ecto.setup` or similar)
- You have an org_id with test messages in the database
- LLM client is configured (check `lib/anantha_os/llm/client.ex`)

## Workflow

### Step 1: Run the pipeline
```
mix memory.test --scenarios 50 --top 20
```
Or for a single pass without looping:
```
mix memory.test_loop --once
```

### Step 2: Read bug reports
Bug reports are saved to `memory_testing/patterns/` as YAML files grouped by severity. Also available in memory via `Anantha.MemoryTesting.PatternStore.all()`. Each bug has:
- `severity`: critical, high, medium, low
- `delta.missing_fields`: what wasn't extracted
- `delta.wrong_fields`: what was extracted wrong (with expected vs actual)
- `root_cause_hypothesis`: best-guess at what went wrong
- `source_scenario`, `test_type`, `pattern_tags`

### Step 3: Prioritize fixes
Fix in this order: **critical → high → medium**. Group bugs by their root cause to fix multiple with one change.

### Step 4: Map bug to fix target

| Bug pattern                            | Likely fix file                                                                                                                |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| Missing/wrong cash amount              | `lib/anantha_os/memory/extraction_worker.ex` (`extract_amount_from_content`) or `lib/anantha_os/memory/extractor.ex` (system prompt) |
| Missing/wrong item/quantity            | `extraction_worker.ex` (`extract_item_and_quantity_from_content`) or `extractor.ex` prompt                                           |
| Wrong direction (inflow/outbound)      | `extraction_worker.ex` (`extract_direction_from_content`)                                                                          |
| Missing actor/party                    | `extraction_worker.ex` (`extract_actor_name_from_content`)                                                                         |
| Wrong promise_type / revision handling | `lib/anantha_os/memory/pipeline.ex` (`handle_promise`)                                                                             |
| Classification wrong (memory_type)      | `lib/anantha_os/memory/classifier.ex` or `prompts/memory/classify.message.prompt`                                                  |
| LLM JSON parse failure                  | `lib/anantha_os/llm/json_extractor.ex` or `extractor.ex` (`parse_extraction_response`)                                               |
| False positive bug reports             | `lib/anantha_os/memory_testing/evaluator.ex`, `normalizer.ex`, or `bugger.ex`                                                       |

### Step 5: Fix the code
Apply the fix in the identified source file. The rule: fix the root cause, not the symptom. If the LLM prompt is ambiguous, clarify it. If the parser is brittle, harden it. If the fallback regex misses cases, extend it.

### Step 6: Re-run and verify
```
mix memory.test --scenarios 50 --top 20
```
Check:
- Did the specific bugs you fixed go away?
- Any new bugs introduced?
- Passing rate improved?

### Step 7: Loop or done
- If bugs remain → **invoke this skill again** (`/memory-fix`)
- If zero bugs → done
- If only known-acceptable ambiguous failures remain → done
- If no improvement after 2 consecutive iterations → escalate (likely a deeper issue needs human review)

## Important Notes

- The pipeline's extraction stage currently uses simulation. To test real LLM extraction, wire `Runner.simulate_extraction/1` to call `Anantha.Memory.Pipeline.process_message/2` or `Anantha.Memory.Extractor.extract/2`
- Start with high severity bugs — they usually indicate clear extraction failures
- When fixing prompts, test with both the YAML scenarios and real conversations from your org
- Keep track of what you fixed — if a bug pattern recurs, consider adding it to the ScenarioCompiler

## Eval Queries

Use these queries to verify the pipeline after fixes:

### Eval: Basic extraction accuracy
```
Evaluate whether the memory extraction pipeline correctly extracts cash flows, inventory movements, and sentiments from test messages. Check that field presence, types, and values match the expected output in the test scenarios.
```

### Eval: Regression check
```
Run the full memory.test suite and compare the pass rate against the previous run. Verify that no previously passing scenarios regressed after the latest fix.
```

### Eval: Edge case coverage
```
Identify any extraction test scenarios that are missing from the suite. Check for coverage gaps in: non-standard amounts, multiple items in one message, ambiguous direction indicators, and actor name variations.
```

### Eval: Fix effectiveness
```
Given the list of bugs fixed in this iteration, verify that each specific bug pattern no longer appears in the output of `mix memory.test --scenarios 50 --top 20`. Confirm the root cause was addressed, not just a single symptom.
```
