# System Architecture

This document describes the public high-level architecture of the **Autonomous AI Video Production System** developed by [TankDev](https://tankdev.tech).

The system coordinates research, content generation, AI providers, media production, operational controls, and publishing through a centralized production pipeline.

> This document describes architectural boundaries and component responsibilities without exposing production source code, credentials, internal prompts, proprietary orchestration logic, or security-sensitive configuration.

## Architecture Goals

The system was designed around several operational requirements:

- Coordinate multiple AI and media services through a single production workflow
- Separate short-form and long-form production logic
- Keep external AI providers configurable
- Control variable API consumption and production costs
- Validate production requirements before publishing
- Separate generation, control, and publishing responsibilities
- Support scheduled production without requiring a local workstation
- Isolate project-specific credentials and runtime configuration

## High-Level Architecture

<pre>
┌─────────────────────────────────────┐
│        Production Configuration     │
│                                     │
│ Topic · Audience · Language         │
│ Format · Models · Budget · Schedule │
└──────────────────┬──────────────────┘
                   │
                   ▼
┌─────────────────────────────────────┐
│       Scheduler / Orchestrator      │
│                                     │
│ Production State · Job Coordination │
└──────────────────┬──────────────────┘
                   │
                   ▼
┌─────────────────────────────────────┐
│          Research Layer             │
│                                     │
│ Research · Sources · Context        │
└──────────────────┬──────────────────┘
                   │
                   ▼
┌─────────────────────────────────────┐
│       Content Generation Layer      │
│                                     │
│ Script · Structure · Metadata       │
└──────────────────┬──────────────────┘
                   │
          ┌────────┴────────┐
          │                 │
          ▼                 ▼
┌───────────────────┐  ┌───────────────────┐
│  Voice Providers  │  │ Visual Providers  │
│                   │  │                   │
│ Narration         │  │ Images / Assets   │
│ Timing            │  │ Scene Visuals     │
└─────────┬─────────┘  └─────────┬─────────┘
          │                      │
          └──────────┬───────────┘
                     │
                     ▼
┌─────────────────────────────────────┐
│        Media Assembly Layer         │
│                                     │
│ Video · Audio · Subtitles · Timing  │
└──────────────────┬──────────────────┘
                   │
                   ▼
┌─────────────────────────────────────┐
│         Production Controls         │
│                                     │
│ Quality · Budget · Format · Limits  │
└──────────────────┬──────────────────┘
                   │
             ┌─────┴─────┐
             │           │
           PASS          FAIL
             │           │
             ▼           ▼
┌───────────────────┐  Stop / Review
│ Publishing Layer  │
│                   │
│ YouTube API       │
│ Export Workflows  │
└───────────────────┘
</pre>

## Component Responsibilities

### 1. Production Configuration

Production begins with a configurable set of operational parameters.

Configuration can define:

- Topic scope
- Target audience
- Language
- Video format
- Output resolution
- AI providers and models
- Publishing frequency
- API spending limits
- Retry limits
- Concurrent production limits

This separates production policy from individual generation operations.

The pipeline therefore operates according to explicit configuration rather than embedding every production decision directly into individual generation steps.

## 2. Scheduler and Orchestration Layer

The orchestration layer coordinates the lifecycle of a production job.

Its architectural responsibilities include:

- Starting scheduled production
- Tracking production state
- Coordinating pipeline stages
- Managing dependencies between stages
- Applying retry boundaries
- Enforcing configured concurrency
- Routing successful outputs to subsequent stages
- Preventing failed jobs from automatically continuing

The internal orchestration implementation is proprietary and is not distributed through this repository.

## 3. Research Layer

Research is treated as a dedicated production stage rather than being merged directly into script generation.

At a high level, this layer is responsible for:

- Topic research
- Source collection
- Information preparation
- Context construction for downstream generation

This provides the content-generation layer with research context gathered during the current production workflow.

<pre>
Topic
  │
  ▼
Research
  │
  ▼
Sources
  │
  ▼
Prepared Context
  │
  ▼
Content Generation
</pre>

## 4. Content Generation Layer

The content layer transforms prepared research into production-oriented content.

Responsibilities can include:

- Script generation
- Narrative structure
- Scene planning
- Metadata preparation
- Format-specific content decisions

Short-form and long-form content are treated as separate production paths.

<pre>
                 Prepared Research
                        │
                ┌───────┴───────┐
                │               │
                ▼               ▼
          Long-Form Flow   Short-Form Flow
                │               │
                ▼               ▼
        Extended Narrative  Compressed Narrative
                │               │
                └───────┬───────┘
                        │
                        ▼
                 Media Production
</pre>

This prevents short-form content from being modeled simply as a shortened version of a long-form template.

## 5. Provider Layer

External AI capabilities are separated according to their production responsibility.

Conceptually, the system can interact with independent providers for:

- Research and text generation
- Voice generation
- Visual generation
- Additional media generation
- Publishing integrations

<pre>
                 Orchestration Layer
                         │
        ┌────────────────┼────────────────┐
        │                │                │
        ▼                ▼                ▼
 Research / Text       Voice           Visuals
    Provider          Provider          Provider
        │                │                │
        └────────────────┼────────────────┘
                         │
                         ▼
                  Production Pipeline
</pre>

Provider separation reduces coupling between the overall workflow and any single external AI service.

Provider-specific credentials, prompts, request logic, fallback rules, and internal integration details are intentionally excluded from this public documentation.

## 6. Voice Production

Voice generation produces narration required by the media pipeline.

Narration duration can provide timing information for downstream scene construction.

This creates a production relationship between generated narration and scene timing rather than requiring every scene to operate on a fixed duration.

**Input:** Approved narration text  
**Process:** Voice generation  
**Output:** Narration media and timing information

## 7. Visual Production

The visual layer produces or prepares visual assets according to the scene and narrative context.

Visual production remains logically separate from script and voice generation.

This allows visual-provider configuration to change without redefining the complete content-production workflow.

**Input:** Scene context and visual direction  
**Process:** Visual generation or preparation  
**Output:** Scene visual assets

## 8. Media Assembly Layer

The assembly layer combines production assets into the final video timeline.

Its responsibilities include coordination of:

- Narration
- Visual assets
- Scene timing
- Transitions
- Subtitles
- Output format

<pre>
Narration ───────┐
                 │
Visuals ─────────┼──► Media Assembly ───► Video
                 │
Subtitles ───────┤
                 │
Timing ──────────┘
</pre>

Short-form and long-form outputs can use different assembly requirements according to their production configuration.

## 9. Production Control Layer

Automated generation does not automatically imply automated publishing.

A dedicated control boundary exists between media production and publishing.

The system can evaluate configured requirements such as:

- Video duration
- Output format
- Production configuration
- API budget state
- Required service availability
- Workflow completion state

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
Publish   Stop
Path      / Review
</pre>

This boundary prevents critical production failures from automatically propagating into the publishing stage.

## 10. API Budget Architecture

External AI services introduce variable operating costs.

Budget control is therefore treated as part of the production architecture rather than as an external accounting process.

The system supports four control levels:

<pre>
Per-Video Budget
       │
       ▼
Daily Budget
       │
       ▼
Weekly Budget
       │
       ▼
Monthly Budget
</pre>

Production can be constrained according to configured limits before additional API consumption is allowed.

This architecture makes cost boundaries explicit within automated production.

## 11. Publishing Layer

Publishing is separated from generation and assembly.

### YouTube

Supported publishing operations can use YouTube API integration for configured workflows.

Publishing configuration can include:

- Schedule
- Privacy state
- Subtitle handling
- Translation configuration
- API readiness checks

### Other Platforms

Vertical outputs and associated publishing assets can be prepared for platforms such as Instagram Reels and TikTok.

Where direct publishing integration is not configured, final upload remains a manual operation.

This distinction prevents the architecture from representing prepared output as an automated platform integration.

## 12. Runtime Environment

The production system can operate on a dedicated VPS.

<pre>
┌─────────────────────────────────────┐
│             VPS Runtime             │
│                                     │
│  Scheduler                          │
│  Orchestrator                       │
│  Production Configuration           │
│  Provider Integrations              │
│  Media Processing                   │
│  Production State                   │
│  Generated Assets                   │
└─────────────────────────────────────┘
</pre>

This allows scheduled production to continue without requiring a local development workstation to remain online.

Project-specific credentials, environment configuration, infrastructure details, and operational secrets are intentionally not published.

## Production State

A production job progresses through defined stages rather than being treated as a single AI request.

A simplified public representation is:

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
  ├── FAIL ──► STOPPED / REVIEW
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

The exact internal state model and implementation are proprietary.

## Architectural Principles

### Orchestration Over Script Chaining

The production process is modeled as coordinated stages with explicit responsibilities rather than as an uncontrolled sequence of AI calls.

### Provider Separation

External services are treated as replaceable production dependencies rather than defining the architecture of the entire system.

### Research Before Generation

Research and source processing occur before script generation.

### Format-Specific Workflows

Short-form and long-form production maintain separate production logic.

### Cost-Aware Production

API budgets form part of the operational architecture.

### Explicit Control Gates

Generated content must pass configured production boundaries before automated publishing.

### Runtime Isolation

Production can operate independently on dedicated infrastructure.

## Technology Boundaries

| Layer | Responsibility |
| --- | --- |
| Configuration | Production policy and operational settings |
| Scheduler | Production timing |
| Orchestrator | Pipeline coordination and production state |
| Research | Information and source preparation |
| Content | Script, narrative, scenes, and metadata |
| Providers | AI and media generation services |
| Assembly | Video, audio, subtitles, and timing |
| Controls | Quality, format, budget, and workflow validation |
| Publishing | YouTube integration and platform-ready exports |
| Runtime | VPS-based continuous operation |

## Public vs. Proprietary Architecture

This repository documents enough of the architecture for technical evaluation while protecting production implementation details.

**Publicly documented**

- System boundaries
- Production stages
- Provider separation
- Short-form and long-form architecture
- Budget-control model
- Production control gates
- Publishing boundaries
- Runtime model

**Not publicly distributed**

- Production source code
- API keys and credentials
- Internal prompts
- Provider-specific request implementation
- Proprietary orchestration logic
- Internal fallback strategies
- Infrastructure secrets
- Channel credentials
- Security-sensitive configuration

## Related Documentation

- [Repository Overview](../README.md)
- [TankDev Case Study](https://tankdev.tech/tr/case-studies/ai-video-automation)
- [System Overview](https://tankdev.tech/tr/systems/ai-video-automation)

## About TankDev

[TankDev](https://tankdev.tech) develops custom software systems around real operational requirements, business rules, data flows, and system boundaries.

**Software Engineering · Applied AI · Process Automation**
