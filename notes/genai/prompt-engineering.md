# Prompt Engineering

## What is Prompt Engineering?

The practice of designing inputs (prompts) to get desired outputs from LLMs.

## Anatomy of a Good Prompt

```
[Role]        → Who the AI should act as
[Context]     → Background information
[Task]        → What you want it to do
[Format]      → How you want the output
[Constraints] → Rules or limitations
```

**Example:**
```
You are a senior React developer.
I have a component that re-renders on every keystroke.
Write a memoized version using useCallback and React.memo.
Use TypeScript. Keep it under 30 lines.
```

## Prompting Techniques

### Zero-Shot

No examples. Just the task.

```
Classify this text as positive, negative, or neutral:
"The product is okay but the shipping was slow."
```

### Few-Shot

Provide examples before the task.

```
Classify the sentiment:

Text: "I love this!" → Positive
Text: "Terrible experience" → Negative
Text: "It's fine I guess" → Neutral

Text: "The product is okay but the shipping was slow."
→
```

### Chain-of-Thought (CoT)

Ask the model to think step by step.

```
Solve this step by step:
If a shirt costs $25 and is 20% off, then has an additional 10% 
discount at checkout, what is the final price?
```

### Role Prompting

Assign a specific role to guide tone and expertise.

```
Act as a senior backend engineer reviewing this code for security 
vulnerabilities. Be specific about each issue found.
```

### Structured Output

Explicitly define the format.

```
List 3 React hooks and their use cases.
Format:
- Hook name: description
```

## Best Practices

### Be Specific

```
❌ "Write a function"
✅ "Write a TypeScript function that takes an array of users and 
    returns only active users sorted by name"
```

### Provide Context

```
❌ "How do I fix this error?"
✅ "I'm using React 18 with TypeScript. When I call setState inside 
    useEffect without a dependency array, I get: Warning: useEffect 
    cleanup function must return..."
```

### Break Down Complex Tasks

```
❌ "Build me a full authentication system"
✅ Step 1: "Design the database schema for user auth with email/password"
Step 2: "Write the registration endpoint with validation"
Step 3: "Write the login endpoint with JWT"
```

### Use Delimiters

Separate instructions from content.

```
Summarize the following article in 3 bullet points:

---
[article text here]
---
```

## Common Patterns

### For Code Generation

```
Write a React custom hook called useLocalStorage that:
- Accepts a key and initial value
- Persists to localStorage
- Handles JSON serialization
- Includes TypeScript generics
- Include usage example
```

### For Code Review

```
Review this code for:
1. Security issues
2. Performance problems
3. Best practices violations

Explain each issue with the line number and suggested fix.
```

### For Explanations

```
Explain [concept] like I'm a developer who knows [related concept] 
but has never used [target concept]. Use a practical example.
```

## Anti-Patterns

| Anti-Pattern | Why it's bad |
|---|---|
| Too vague | Model guesses what you want |
| Too long | Model may lose focus |
| Contradictory instructions | Unpredictable output |
| No examples for complex tasks | Model may misunderstand format |
| Assuming knowledge | Model doesn't know your codebase |

## Quick Reference

| Technique | When to use |
|---|---|
| Zero-shot | Simple, clear tasks |
| Few-shot | Specific format or style needed |
| Chain-of-thought | Math, logic, complex reasoning |
| Role prompting | Need expertise in specific domain |
| Structured output | JSON, tables, specific formats |
