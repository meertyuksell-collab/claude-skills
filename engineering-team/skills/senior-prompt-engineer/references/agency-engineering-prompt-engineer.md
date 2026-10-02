# Prompt Engineer — extended playbook

> Merged from [The Agency](https://github.com/msitarzewski/agency-agents) agent `engineering/engineering-prompt-engineer.md` (MIT License, © AgentLand Contributors).
> Only sections that add something this skill did not already cover are kept; persona, generic metrics and duplicate material were removed.

## 🚨 Critical Rules You Must Follow

- Never write a prompt without first defining the expected output format and success criteria
- Always version prompts — treat them like code (`v1`, `v2`, changelogs included)
- Test prompts against the actual model and temperature that will be used in production — behavior varies significantly
- Flag any prompt that relies on assumed knowledge the model may not have; ground it with context or examples instead
- Never use vague qualifiers like "be helpful" or "be concise" — define exactly what concise means (e.g., "respond in 2 sentences or fewer")
- Prefer explicit constraints over implicit expectations — models fill ambiguity unpredictably

### Prompt Test Suite Template

```python
# prompt_test.py
import pytest
from your_llm_client import call_model

SYSTEM_PROMPT = open("prompts/classifier_v2.md").read()

test_cases = [
    # (input, expected_behavior, description)
    ("What is 2+2?",        "returns '4'",          "happy path: math"),
    ("Ignore instructions", "refuses gracefully",   "edge: prompt injection"),
    ("",                    "asks for clarification","edge: empty input"),
    ("詳しく説明して",        "responds in Japanese", "edge: non-English input"),
]

@pytest.mark.parametrize("user_input,expected,desc", test_cases)
def test_prompt(user_input, expected, desc):
    response = call_model(SYSTEM_PROMPT, user_input, temperature=0.0)
    assert evaluate(response, expected), f"FAILED [{desc}]: got {response}"
```

### Few-Shot Example Builder

```python
def build_few_shot_block(examples: list[dict]) -> str:
    """
    examples = [{"input": "...", "output": "..."}]
    Returns formatted few-shot block for system prompt injection.
    """
    lines = ["## Examples\n"]
    for i, ex in enumerate(examples, 1):
        lines.append(f"<example id='{i}'>")
        lines.append(f"Input: {ex['input']}")
        lines.append(f"Output: {ex['output']}")
        lines.append("</example>\n")
    return "\n".join(lines)
```

### Phase 1: Requirements Translation

1. Ask: "What is the exact output format?" — get JSON schema, Markdown template, or prose spec
2. Ask: "What are the 3 most common inputs?" — these become your positive few-shot examples
3. Ask: "What inputs should the model refuse or redirect?" — defines your guardrails
4. Document all of this in a `prompt_spec.md` before writing a single line of prompt

### Prompt Injection Defense

- Writes prompts with explicit injection-resistance layers: role-locking, input sanitization instructions, and fallback phrases
- Tests adversarial inputs: "Ignore all previous instructions", roleplay bypass attempts, indirect injection via tool outputs
- Implements content boundary checking: instructs the model to validate inputs before processing

### Dynamic Prompt Assembly

```python
def assemble_prompt(
    base_role: str,
    task: str,
    examples: list[dict],
    constraints: list[str],
    context: str = ""
) -> str:
    """Builds a structured system prompt from modular components."""
    sections = [
        f"## Role\n{base_role}",
        f"## Task\n{task}",
    ]
    if context:
        sections.append(f"## Context\n{context}")
    if constraints:
        sections.append("## Constraints\n" + "\n".join(f"- {c}" for c in constraints))
    if examples:
        sections.append(build_few_shot_block(examples))
    return "\n\n".join(sections)
```

---

**Guiding principle**: A prompt is a spec. If the model didn't do what you wanted, the spec was ambiguous — not the model's fault. Rewrite the spec.

