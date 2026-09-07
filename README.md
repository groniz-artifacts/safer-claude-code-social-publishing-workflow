# A safer Claude Code workflow for social publishing

A safer publishing workflow starts with one campaign source. Turn it into channel-specific drafts, require explicit human review, send only the approved versions through Groniz, and verify every result. Claude Code handles the repetitive work while the operator decides what goes live.

## The five-stage workflow

### 1. Start with one source file

Install the Groniz Skill in Claude Code:

```bash
npx skills add groniz/groniz-cli
```

This gives Claude Code a consistent way to work with Groniz. The [full Claude Code setup guide](https://groniz.com/blog/how-to-automate-social-media-posting-with-claude-code/) covers the rest of the configuration. Keep the campaign brief in your repository and use it as the source of truth.

A compact source file can look like this:

```yaml
campaign: July product update
goal: Bring existing users to the release notes
audience: Developers already using the API
message: Batch scheduling is now available
link: https://example.com/releases/batch-scheduling
channels:
  - LinkedIn
  - X
constraints:
  - No performance claims
  - Use US English
schedule: 2026-07-30T16:00:00Z
```

Keep the facts, links, audience, constraints, requested channels, and timing in this file. If anything changes, update the source before generating another draft.

### 2. Generate channel-specific drafts

Channel limits and required settings vary. Before Claude Code writes any payloads, have it inspect the live Groniz integration for each requested channel. Generate a separate draft for each channel instead of forcing the same copy into every format.

Use this reusable instruction:

```text
Read the campaign source file and use the installed Groniz Skill.
Inspect the live integration, capabilities, and required settings for each
requested channel before constructing anything. Create channel-specific
drafts only; do not schedule or publish. Preserve every factual claim and
constraint from the source. Return, for each channel: final copy, link and
media choices, required delivery settings, and any unresolved assumption or
unsupported request.
```

Finish this stage with a reviewable draft for every requested channel, or a clear explanation of what is missing.

### 3. Require explicit human approval

Keep approval in your own operating process. Generated copy, a clean validation result, or a requested schedule does not count as permission to publish.

Review each channel version against this checklist:

- Every claim matches the source file.
- The link and destination are correct.
- Tone and call to action fit the intended audience.
- Copy, media, mentions, and tags are correct for that channel.
- Required channel settings and schedule use the intended values and timezone.
- No unresolved assumption remains.
- The reviewer explicitly approves the exact final copy and delivery settings.

Record approval somewhere your team already controls, such as a pull request, issue, or campaign record. The decision must be unambiguous and tied to the exact draft.

Any substantive edit after approval sends that channel draft back through review.

### 4. Deliver only the approved versions

Give Claude Code the approved copy and settings, identify the intended channels, and explicitly authorize scheduling or publishing. Groniz Connectors runs from your own agent, the Console, or the public API. It handles OAuth, per-platform formatting, scheduling, and delivery across 32+ networks.

Keep the delivery request narrow. Name the approved draft, channel, account, and schedule. Before it acts, ask Claude Code to show any setting that differs from the approved record. A connected account does not mean every channel accepts the same fields or features.

### 5. Verify and record the outcome

A successful request still needs verification. Check each channel independently and record:

- Whether the item was scheduled, published, or rejected.
- The channel and account used.
- The effective publication time and timezone.
- Any returned post, job, or delivery identifier.
- The public URL when one is available.
- The error and next action for any failure.

Compare the delivered post with the approved copy. If platform formatting changed something material, record the difference and decide whether to correct it.

## Handle live-integration failures safely

When a live integration fails, preserve the approved state and avoid accidental duplicates:

- If authentication or a required setting is missing, stop work for that channel and report the exact missing input.
- If a requested option is unsupported, revise the channel plan and return it for approval.
- If some channels succeed and others fail, keep the successes recorded and retry only the failed channels.
- If the result is ambiguous, inspect the live delivery state before retrying.
- If copy or delivery settings must change, create a new draft and repeat approval.

The source stays intact, approved drafts remain identifiable, and you can reconcile delivery attempts one channel at a time.

[Set up Groniz for Claude Code](https://groniz.com/agents/claude-code).
