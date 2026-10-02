# Production Workflow

This document describes the public operational workflow of the **Autonomous AI Video Production System** developed by [TankDev](https://tankdev.tech).

The workflow coordinates research, content generation, media production, operational controls, and publishing as a structured production lifecycle.

> Proprietary source code, internal prompts, credentials, provider-specific implementation details, and security-sensitive configuration are intentionally excluded.

## Workflow Overview

<pre>
Topic
  │
  ▼
Research
  │
  ▼
Source Processing
  │
  ▼
Script Generation
  │
  ▼
Voice & Visual Production
  │
  ▼
Media Assembly
  │
  ▼
Production Controls
  │
  ├── FAIL ──► Stop / Review
  │
  ▼
 PASS
  │
  ▼
Metadata & Publishing
  │
  ▼
Completed Production
</pre>

## Stage 1 — Production Initialization

A production job begins with configuration rather than an isolated AI request.

The production environment can define parameters such as:

- Topic scope
- Target audience
- Language
- Video format
- AI providers and models
- Output configuration
- Publishing schedule
- API budget limits
- Retry limits
- Concurrent production limits

The scheduler and orchestration layer use these parameters to initialize the production lifecycle.

**Input:** Production configuration  
**Process:** Job initialization  
**Output:** Configured production job  
**Responsibility:** Scheduler / Orchestrator

## Stage 2 — Research

The selected topic enters a dedicated research stage.

Research occurs before script generation so that downstream content can operate on information collected for the current production job.

The research stage can gather information and sources relevant to the selected topic.

**Input:** Topic and production context  
**Process:** Topic research  
**Output:** Research material and sources  
**Responsibility:** Research Layer

## Stage 3 — Source Processing

Collected research is prepared for downstream generation.

The objective is to transform raw research results into usable production context while maintaining the relationship between the generated content and the information gathered during the research stage.

<pre>
Research Results
       │
       ▼
Source Processing
       │
       ▼
Prepared Context
</pre>

**Input:** Research results and sources  
**Process:** Context preparation  
**Output:** Generation-ready context  
**Responsibility:** Research / Content Layer

## Stage 4 — Script Generation

Prepared research context is transformed into a production-oriented script.

The generation path depends on the configured video format.

### Long-Form

Long-form production can use:

- Extended narrative structure
- Broader research context
- Longer scene continuity
- Landscape-oriented output

### Short-Form

Short-form production can use:

- Direct opening
- Compressed narrative
- Shorter scene structure
- Vertical-oriented output

<pre>
Prepared Context
      │
 ┌────┴────┐
 │         │
 ▼         ▼
Long      Short
Form      Form
 │         │
 └────┬────┘
      │
      ▼
Production Script
</pre>

**Input:** Prepared research context  
**Process:** Format-specific script generation  
**Output:** Production script  
**Responsibility:** Content Generation Layer

## Stage 5 — Voice Production

The approved narration is sent to the configured voice provider.

The resulting narration provides both audio and timing information for downstream media production.

Measured narration duration can therefore influence scene construction rather than requiring every scene to use a fixed predefined duration.

**Input:** Narration text  
**Process:** Voice generation  
**Output:** Narration audio and timing information  
**Responsibility:** Voice Provider / Production Layer

## Stage 6 — Visual Production

Visual requirements are prepared according to the script and scene context.

The configured visual provider can then generate or prepare the assets required for each scene.

Voice and visual production remain separate responsibilities.

<pre>
Production Script
      │
 ┌────┴────┐
 │         │
 ▼         ▼
Voice    Visuals
 │         │
 └────┬────┘
      │
      ▼
Media Assets
</pre>

**Input:** Script and scene context  
**Process:** Visual generation or preparation  
**Output:** Scene visual assets  
**Responsibility:** Visual Provider / Production Layer

## Stage 7 — Media Assembly

Generated media assets are combined into the video timeline.

The assembly stage coordinates:

- Narration
- Visual assets
- Scene timing
- Transitions
- Subtitles
- Output format

<pre>
Narration ─────┐
               │
Visuals ───────┼──► Assembly ───► Video
               │
Subtitles ─────┤
               │
Timing ────────┘
</pre>

The output of this stage is a generated video candidate.

It is not automatically considered ready for publishing.

**Input:** Production media assets  
**Process:** Timeline and media assembly  
**Output:** Generated video candidate  
**Responsibility:** Media Assembly Layer

## Stage 8 — Production Controls

The generated output passes through an explicit control boundary before publishing.

Configured checks can evaluate conditions such as:

- Duration
- Output format
- Workflow completion
- Production configuration
- Required service state
- API budget availability

<pre>
Generated Video
      │
      ▼
Production Controls
      │
 ┌────┴────┐
 │         │
PASS      FAIL
 │         │
 ▼         ▼
Continue  Stop
          Review
          / Retry
</pre>

A failed critical check prevents the job from automatically continuing to publishing.

**Input:** Generated production output  
**Process:** Production validation  
**Output:** Approved output or stopped job  
**Responsibility:** Control Layer

## Budget Evaluation

API budget controls operate as part of the production lifecycle.

The public model contains four control levels:

<pre>
Per Video
    │
    ▼
Daily
    │
    ▼
Weekly
    │
    ▼
Monthly
</pre>

When a configured budget boundary prevents additional API consumption, the production job does not continue as if resources were unlimited.

This allows automated production to operate within explicit cost constraints.

## Retry Boundary

Retry behavior is controlled rather than unlimited.

Conceptually:

<pre>
Operation
   │
   ▼
Success? ── YES ──► Continue
   │
   NO
   │
   ▼
Retry Allowed?
   │
 ┌─┴─┐
 │   │
YES  NO
 │   │
 ▼   ▼
Retry Stop
</pre>

The exact retry implementation and provider-specific fallback strategies are proprietary.

## Stage 9 — Publishing Preparation

After production controls pass, publishing assets are prepared.

These can include:

- Final video
- Title
- Description
- Thumbnail
- Subtitle data
- Translation assets
- Publishing configuration

The resulting package contains the assets required for the configured distribution workflow.

**Input:** Approved video and metadata  
**Process:** Publishing preparation  
**Output:** Platform-ready publishing package  
**Responsibility:** Publishing Layer

## Stage 10 — Distribution

### YouTube

Configured workflows can use YouTube API integration for supported publishing operations.

Depending on configuration, this can include:

- Video upload
- Publishing schedule
- Privacy state
- Subtitle handling
- Translation configuration
- API readiness checks

### Instagram Reels and TikTok

Vertical outputs and associated publishing materials can be prepared for these platforms.

Where direct platform publishing is not configured, final upload remains manual.

The workflow therefore distinguishes between:

**automated publishing** and **platform-ready output**.

## Production Completion

A production job is considered complete after its configured distribution workflow has been successfully processed.

A simplified lifecycle is:

<pre>
QUEUED
  │
  ▼
RESEARCHING
  │
  ▼
GENERATING
  │
  ▼
PRODUCING MEDIA
  │
  ▼
ASSEMBLING
  │
  ▼
VALIDATING
  │
  ├── FAIL ──► REVIEW / STOP
  │
  ▼
READY
  │
  ▼
PUBLISHING
  │
  ▼
COMPLETED
</pre>

The exact internal state model is not part of this public repository.

## Workflow Responsibilities

| Stage | Primary Component | Output |
| --- | --- | --- |
| Initialization | Scheduler / Orchestrator | Configured production job |
| Research | Research Layer | Research material |
| Source Processing | Research / Content Layer | Prepared context |
| Script | Content Generation | Production script |
| Voice | Voice Provider | Narration and timing |
| Visuals | Visual Provider | Scene assets |
| Assembly | Media Pipeline | Generated video |
| Controls | Control Layer | Approved or stopped output |
| Publishing Preparation | Publishing Layer | Platform-ready package |
| Distribution | Platform Integration | Published or export-ready content |

## Workflow Principles

### Research Before Generation

The production lifecycle gathers current research context before script generation.

### Separate Production Responsibilities

Research, writing, voice, visuals, assembly, controls, and publishing remain defined stages rather than one opaque AI operation.

### Format-Specific Flows

Short-form and long-form production use separate production logic.

### Controlled Retries

Failed operations do not create unlimited retry loops.

### Budget-Aware Execution

Variable API consumption is constrained by configured production budgets.

### Validation Before Publishing

Generated media must pass the configured control boundary before automated publishing.

### Publishing Is a Separate Stage

A successfully generated video is not automatically equivalent to a successfully published video.

## Public Documentation Boundary

### Included

- Production lifecycle
- Stage responsibilities
- Short-form and long-form separation
- Control gates
- Budget model
- Retry concept
- Publishing boundaries

### Excluded

- Production source code
- Internal prompts
- API credentials
- Channel credentials
- Provider-specific requests
- Proprietary orchestration logic
- Internal fallback strategies
- Infrastructure secrets
- Security-sensitive configuration

## Related Documentation

- [Repository Overview](../README.md)
- [System Architecture](./architecture.md)
- [TankDev Case Study](https://tankdev.tech/tr/case-studies/ai-video-automation)
- [System Overview](https://tankdev.tech/tr/systems/ai-video-automation)

## About TankDev

[TankDev](https://tankdev.tech) develops custom software systems around real operational requirements, business rules, data flows, and system boundaries.

**Software Engineering · Applied AI · Process Automation**
