---
name: Prompt Engineer
version: 1.0.0
category: ai
purpose: Optimizes and tests AI prompts for LLM best results, token efficiency
tags:
  - prompt
  - LLM
  - AI
  - optimization
  - tokens
  - nlp
  - gpt
  - llama
  - claude
requires:
  - openclaw CLI
  - access to LLM API (openai, anthropic, ollama, etc.)
outputs:
  - optimized prompts
  - prompt analysis
  - token budgets
  - test results
---

# Prompt Engineer

Specialized skill for optimizing, testing, and refining AI prompts to achieve maximum effectiveness with minimum token usage.

## Quick Reference

| Command | Description |
|---------|-------------|
| `prompt:analyze <prompt>` | Analyze prompt structure, clarity, and potential issues |
| `prompt:optimize <prompt>` | Optimize prompt for clarity and efficiency |
| `prompt:tokenize <prompt>` | Count tokens and estimate cost |
| `prompt:test <prompt> --model gpt-4` | Test prompt against specified model |
| `prompt:compare <prompt1> <prompt2>` | Compare two prompts side-by-side |
| `prompt:expand <seed>` | Generate enhanced prompt from seed idea |
| `prompt:shrink <prompt>` | Reduce token count while preserving intent |
| `prompt:role <role> <task>` | Create prompt with specific role assignment |

## Core Concepts

### Token Budgeting
- **System prompts**: Reserve 500-800 tokens for instructions
- **Context window**: Know your model's limit (GPT-4: 128k, Claude: 200k, Llama: 8k-128k)
- **Output buffer**: Leave 20% of context for response
- **Rule of thumb**: Input tokens ≈ 3.5x cost of output tokens

### Prompt Anatomy
```
[Role] + [Context] + [Task] + [Constraints] + [Format] + [Examples]
```

## Use Cases

### 1. Code Review Prompt
```markdown
Analyze this code for security vulnerabilities:

```python
def process_payment(user_id, amount):
    db.execute(f"UPDATE accounts SET balance = balance - {amount} WHERE id = {user_id}")
```

Identify: SQL injection, race conditions, input validation issues
Format: JSON with severity levels
```

### 2. Documentation Generator
```markdown
Generate API docs for this function:

```typescript
function getUserById(id: string, includeOrders?: boolean): Promise<User>
```

Include: parameters, return type, errors, example usage
Style: TypeDoc format, concise
```

### 3. Data Extraction Prompt
```markdown
Extract structured data from this invoice:

[INVOICE CONTENT]

Output JSON schema:
{
  "vendor": "string",
  "date": "ISO8601",
  "total": "number",
  "items": [{"description": "string", "quantity": "number", "unit_price": "number"}]
}
```

### 4. SQL Query Optimizer
```markdown
Optimize this PostgreSQL query for performance:

SELECT * FROM orders o
JOIN customers c ON o.customer_id = c.id
JOIN products p ON o.product_id = p.id
WHERE c.country = 'US'
ORDER BY o.created_at DESC
LIMIT 100

Explain: indexes needed, query plan improvements, any denormalization
```

## Commands Reference

### prompt:analyze

Analyzes prompt for structural issues, ambiguity, and optimization opportunities.

```bash
# Analyze a prompt
prompt:analyze "Write a story about a robot"

# Analyze with context
prompt:analyze "Create a Python function to parse JSON" --context "embedded systems, low memory"
```

**Output includes:**
- Clarity score (1-10)
- Ambiguity warnings
- Missing context indicators
- Suggested improvements

### prompt:optimize

Refines prompts for better LLM performance.

```bash
# Basic optimization
prompt:optimize "Make a summary of this text"

# Optimized output
# "Provide a concise 3-bullet summary of the following text, capturing key points and action items:"
```

### prompt:tokenize

Accurate token counting for major providers.

```bash
# Count tokens for GPT-4
prompt:tokenize "Your prompt here" --provider openai --model gpt-4

# Count for Claude
prompt:tokenize "Your prompt here" --provider anthropic --model claude-3-opus

# Estimate cost
prompt:tokenize "Your prompt here" --cost --provider openai --model gpt-4
```

### prompt:test

Execute prompt against live model and capture results.

```bash
# Test with GPT-4
prompt:test "Your optimized prompt" --model gpt-4 --temperature 0.7

# Test with multiple models
prompt:test "Your prompt" --models gpt-4 claude-3-opus llama-3-70b

# Include evaluation criteria
prompt:test "Your prompt" --model gpt-4 --eval "accuracy, coherence, conciseness"
```

### prompt:compare

Side-by-side comparison of prompt variations.

```bash
# Compare two approaches
prompt:compare "Explain quantum physics" "Explain quantum physics to a 5-year-old using analogies"

# With scoring
prompt:compare "version A" "version B" --metrics clarity token_count effectiveness
```

### prompt:expand

Generate comprehensive prompt from minimal seed.

```bash
# Expand a seed idea
prompt:expand "write product descriptions"

# Expanded output includes:
# - Role definition
# - Target audience
# - Tone guidelines
# - Format requirements
# - Constraints
# - Examples
```

### prompt:shrink

Reduce token count while preserving core intent.

```bash
# Aggressive shrinking
prompt:shrink "Your long prompt here" --target 50%

# Moderate shrinking
prompt:shrink "Your prompt" --target 75%
```

### prompt:role

Create structured role-based prompts.

```bash
# Developer role
prompt:role developer "fix this bug" --context "Python, Django, API"

# Analyst role  
prompt:role analyst "analyze sales data" --context "Q4 2024, European market"

# Writer role
prompt:role copywriter "write landing page" --context "SaaS, B2B, enterprise"
```

## Troubleshooting

### Problem: Model ignores constraints

**Symptoms:** Response doesn't follow format, includes prohibited content

**Solutions:**
1. Add explicit "Do NOT" statements
2. Use XML tags to separate instructions from content
3. Place constraints at end of prompt
4. Add "If unable to comply, respond with: [fallback]"

### Problem: Inconsistent output format

**Symptoms:** JSON malformed, varying structures

**Solutions:**
1. Provide 3+ examples in correct format
2. Use XML examples: ```json ... ```
3. Add "Output MUST match this exact structure"
4. Include validation step in prompt

### Problem: Poor reasoning on complex tasks

**Symptoms:** Logical errors, missed edge cases

**Solutions:**
1. Add "Think step by step" or "Chain of thought"
2. Break into numbered steps
3. Ask for "edge cases and how to handle them"
4. Add "Verify your answer by [method]"

### Problem: Token limit exceeded

**Symptoms:** Truncated responses, errors from API

**Solutions:**
1. Use prompt:shrink to reduce
2. Move system instructions to API parameters
3. Use shorter example format
4. Implement streaming for long outputs

### Problem: Model too verbose

**Symptoms:** Unnecessarily long responses

**Solutions:**
1. Add "Respond in maximum X words"
2. Use "Concise" or "Brief" keyword
3. Request bullet points explicitly
4. Set temperature to 0.3-0.5

### Problem: Role confusion

**Symptoms:** Model doesn't adopt persona correctly

**Solutions:**
1. Strengthen role: "You ARE a [role], not just helping"
2. Add background: "With 10 years experience in..."
3. Add constraints: "Never break character"
4. Add example dialogue in character

## Prompt Templates

### Template: Structured Extraction
```
Extract information from the following [CONTENT_TYPE]:

[CONTENT]

Output schema:
[SCHEMA]

Rules:
1. [Constraint 1]
2. [Constraint 2]
3. Return null for missing fields

Format: Valid JSON only, no additional text
```

### Template: Chain of Thought
```
Task: [TASK]

Instructions:
1. [Step 1]
2. [Step 2]
3. [Step 3]

Verify: [Verification method]
```

### Template: Few-Shot Classification
```
Classify the sentiment of these reviews:

Input: "Amazing product, love it!" → [POSITIVE]
Input: "Terrible, want refund" → [NEGATIVE]
Input: "[YOUR INPUT]"

Categories: POSITIVE, NEGATIVE, NEUTRAL
Respond with category only.
```

### Template: System + User Split
```
System: You are a [ROLE]. Guidelines: [GUIDELINES]

User: [TASK]

Remember: [REMINDER]
```

## Best Practices

1. **Be specific, not vague**: "Write 3 paragraphs" > "Write a good summary"
2. **Use delimiters**: Separate instructions from content with ``` or ###
3. **Order matters**: Constraints at end have more weight
4. **Examples work**: 3 examples > lengthy descriptions
5. **Test iteratively**: Prompt optimization is empirical
6. **Track versions**: Store prompt variants with results
7. **Consider tokens**: Every word costs money and context
8. **Role clarity**: "You ARE X" > "You are helpful"
9. **Format output**: "Respond as JSON" > "Give me the data"
10. **Edge cases**: Explicitly handle edge cases in prompt

## Provider-Specific Notes

### OpenAI (GPT-4, GPT-3.5)
- Use `gpt-4-turbo` for 128k context
- `temperature` 0.7 for creative, 0.2 for factual
- `max_tokens` to prevent runaway output

### Anthropic (Claude)
- Supports very long context (200k)
- Claude follows instructions well naturally
- Use "human:" and "assistant:" for few-shot

### Ollama (Local)
- Limited by local hardware
- Smaller models need simpler prompts
- System prompt goes in Modelfile

## Metrics to Track

| Metric | Target | Measurement |
|--------|--------|-------------|
| Token efficiency | <50% of context | prompt:tokenize |
| Success rate | >90% valid output | prompt:test --eval |
| Consistency | >95% format match | Multiple runs |
| Latency | <5s for 1k tokens | API response time |

## Exit Criteria

Skill complete when:
- [ ] Prompt achieves desired output format
- [ ] Token usage within budget
- [ ] Consistent results across 5+ test runs
- [ ] No ambiguity errors in analysis
- [ ] Cost within estimated bounds
```