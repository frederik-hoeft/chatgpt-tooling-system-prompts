# ChatGPT Tooling System Prompts

This repository documents the exact tooling available to ChatGPT agents by capturing the underlying internal system prompts and summarizing them in a human-friendly reference. Use it to understand what tools an agent can call, what arguments they accept, and the constraints baked into the prompts.

## Contents

- Consult [chatgpt-tooling-api-reference.md](chatgpt-tooling-api-reference.md) for an overview of supported capabilities, subcommands, parameters, and intended use cases.
- Read the corresponding file in [raw-system-prompts](raw-system-prompts) for exact agent instructions, constraints, and examples on how it should use each tool.

## Use Cases

- Deterministic tool selection: crafting explicit instructions (e.g., "call `web.run` to fetch live data") and verifying the agent has a matching tool.
- Capability and limitation audits: checking which operations are allowed (read-only Gmail/Calendar, no internet in `python_user_visible`, etc.) before designing flows.
- Prompt optimization: tailoring prompts to align with tool constraints (parameter shapes, required fields, rate limits) to reduce retries and failures.
- Safety and compliance reviews: inspecting raw prompts to confirm what the agent can or cannot do for a given integration.
- Developer onboarding: giving engineers a quick map of available tools so they can design integrations and tests faster.
- Bypassing soft-restrictions: some tools may have soft constraints that disincentivize the agent from using them in certain ways; explicitly asking it to use a specific tool with specific parameters can help bypass these limitations when necessary.

## Scope and limitations

- This repo is informational only; it does not contain the primary system prompt or content policies that govern ChatGPT's overall behavior.
- The documented tools and prompts reflect what is available to ChatGPT agents at the time of capture; capabilities may change over time.
- The information provided may not be up-to-date with the latest internal changes and were retrieved by simply asking ChatGPT for the "raw documentation" of each tool, so there may be nuances or constraints induced by the broader system prompt that are not fully captured here.