---
name: identity-init
description: Initialize or update your identity.json profile for personality-aware learning
---

# /identity-init

Initialize or update your identity profile for the continuous learning system.

## What This Does

Creates `~/.claude/homunculus/identity.json` with your preferences:
- **Technical level**: How detailed should explanations be?
- **Verbosity**: How much context do you want in responses?
- **Code comments**: Should generated code include comments?
- **Domain expertise**: What areas are you most familiar with?

## Usage

```
/identity-init
```

You'll be asked a few questions to determine your preferences.

## Identity Schema

```json
{
  "version": "2.0",
  "technicalLevel": "technical|semi-technical|non-technical|chaotic",
  "preferredStyle": {
    "verbosity": "minimal|moderate|detailed",
    "codeComments": true,
    "explanations": true
  },
  "domains": ["typescript", "react", "cloudflare"],
  "createdAt": "2025-01-25T00:00:00Z",
  "updatedAt": "2025-01-25T00:00:00Z"
}
```

## Technical Levels

| Level | Description |
|-------|-------------|
| **technical** | Experienced developer. Brief explanations, focus on code. |
| **semi-technical** | Familiar with coding. Moderate explanations, some context. |
| **non-technical** | Learning to code. Detailed explanations, step-by-step guidance. |
| **chaotic** | Experienced but prefers creative/experimental approaches. |

## How It Affects Learning

The observer agent uses your identity to:
1. Adjust instinct verbosity (more detailed for non-technical)
2. Weight domain-specific patterns higher
3. Include/exclude code comments in instinct actions
4. Tailor explanations to your expertise level

## Implementation

When this command runs:

1. Check if `~/.claude/homunculus/identity.json` exists
2. If not, ask user questions to determine preferences
3. Create/update the identity file
4. Show confirmation with detected settings

### Questions to Ask

1. **Technical level**: "How would you describe your coding experience?"
   - "Expert developer (technical)"
   - "Intermediate developer (semi-technical)"
   - "Learning to code (non-technical)"
   - "I like to experiment (chaotic)"

2. **Verbosity**: "How detailed should explanations be?"
   - "Brief and to the point (minimal)"
   - "Some context when helpful (moderate)"
   - "Full explanations please (detailed)"

3. **Domains**: "What technologies do you work with most? (select multiple)"
   - Analyze current session's file types
   - Suggest based on observed patterns
   - Allow custom input

### Example Output

```
Created ~/.claude/homunculus/identity.json

Your Profile:
- Technical Level: technical
- Verbosity: moderate
- Code Comments: enabled
- Domains: typescript, react, cloudflare

The observer will now personalize instincts to your style.
Run /identity-init again anytime to update.
```

## Related

- `/instinct-status` - Shows your identity info
- `/evolve` - Uses identity to personalize evolved skills
