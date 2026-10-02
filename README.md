# Autonomous AI Video Production System

An autonomous AI-powered video production and publishing system developed by [TankDev](https://tankdev.tech).

The system coordinates research, source evaluation, script generation, voice production, visual generation, video assembly, subtitles, quality control, metadata, scheduling, and publishing through a controlled production pipeline.

It is designed as a production system rather than a collection of disconnected AI tools.

## Overview

AI-assisted video production commonly requires multiple independent services for research, writing, voice generation, image generation, editing, subtitles, metadata, and publishing.

Managing these services manually creates fragmented workflows, repeated operations, inconsistent production rules, and uncontrolled API usage.

This system brings those stages into a single configurable production pipeline:

**Research → Sources → Script → Voice → Visuals → Assembly → Control → Publishing**

## System Scope

The implemented production system includes:

- **8** coordinated production stages
- **4** API budget control levels
- **2** independent production flows
- **1** centralized control layer
- Separate short-form and long-form production logic
- Configurable AI providers and models
- Automated YouTube publishing support
- Multilingual subtitle configuration
- Production scheduling
- Pre-production service checks
- VPS-based continuous operation

These figures describe implemented system scope and control boundaries rather than audience, revenue, or content-performance KPIs.

## Production Pipeline

<pre>
Topic
  │
  ▼
Research
  │
  ▼
Source Evaluation
  │
  ▼
Script Generation
  │
  ▼
Voice Production
  │
  ▼
Visual Generation
  │
  ▼
Video Assembly
  │
  ▼
Quality & Budget Control
  │
  ▼
Metadata & Publishing
</pre>

## Eight Production Stages

### 1. Research

The system researches the selected topic before content generation begins.

The objective is to build a working information base instead of relying exclusively on the existing knowledge of a language model.

### 2. Source Processing

Research findings are processed together with their sources.

This allows downstream content generation to operate on information collected during the research stage.

### 3. Script Generation

Research is transformed into a structured script according to configured parameters such as:

- Channel
- Target audience
- Video format
- Narrative style

Short-form and long-form content follow separate production logic.

### 4. Voice Production

The script is converted into scene-level narration.

Measured voice duration becomes a timing reference for downstream scene construction rather than relying entirely on fixed scene durations.

### 5. Visual Generation

Visual direction is generated according to the content and narrative context of each scene.

Visual production can be configured independently from other AI providers used elsewhere in the pipeline.

### 6. Video Assembly

Voice, visuals, transitions, and subtitles are combined into a synchronized timeline.

The resulting output is prepared as a publishable video.

### 7. Production Control

Before publishing, the system evaluates configured production constraints.

These can include:

- Duration rules
- Format rules
- API budget limits
- Production configuration
- Required service availability

Content that fails critical checks is not automatically forwarded to publishing.

### 8. Publishing Preparation

The final stage prepares publishing assets including:

- Title
- Description
- Thumbnail
- Video output
- Subtitle data

Supported YouTube operations can be completed through API integration.

Outputs for Instagram Reels and TikTok can be prepared in the appropriate format for manual publishing.

## Short-Form and Long-Form

Short and long videos are not treated as different durations of the same template.

They use separate production logic.

| Long-Form | Short-Form |
| --- | --- |
| Extended narrative | Direct opening |
| Broader research context | Compressed research context |
| Longer continuity | Short scene structure |
| Landscape-oriented production | Vertical-oriented production |
| Independent scheduling | Independent scheduling |

This allows each format to be generated according to its own narrative and production requirements.

## Centralized Control Layer

Production behavior can be configured from a central operational layer.

Configuration can include:

- Topic scope
- Target audience
- Language
- Publishing frequency
- AI model providers
- Output format and resolution
- API spending limits
- Retry limits
- Concurrent production limits

The objective is not simply automation.

The objective is **controlled automation**.

## API Budget Controls

External AI services introduce variable production costs.

The system therefore supports configurable API spending limits at multiple levels:

<pre>
Per Video
    │
    ▼
Daily Limit
    │
    ▼
Weekly Limit
    │
    ▼
Monthly Limit
</pre>

Production can be constrained before API consumption exceeds configured operational limits.

Retry and concurrent-production behavior can also be controlled as part of the production environment.

## Provider Architecture

Research, text generation, visual generation, voice generation, music, and publishing integrations are treated as separate provider responsibilities.

<pre>
                 Production Pipeline
                         │
       ┌─────────────────┼─────────────────┐
       │                 │                 │
       ▼                 ▼                 ▼
 Research / Text      Visuals           Voice
       │                 │                 │
       └─────────────────┼─────────────────┘
                         │
                         ▼
                  Video Assembly
                         │
                         ▼
                     Publishing
</pre>

This separation allows supported providers and models to be configured without treating the entire production pipeline as a single AI dependency.

## Publishing

### YouTube

Supported video production and publishing operations can be completed through YouTube API integration.

Configuration can include:

- Publishing schedule
- Privacy state
- API readiness checks
- Subtitle upload
- Translation languages

### Instagram Reels and TikTok

The system can prepare vertical video and associated publishing materials.

Final upload remains manual where automatic publishing is not part of the configured platform integration.

## Multilingual Subtitles

Subtitle generation and publishing options are managed as part of the production workflow.

Supported configurations can prepare subtitle and translation assets for multiple languages without requiring the core video-production pipeline to be rebuilt for each output language.

## Production Environment

The system can operate continuously on a dedicated VPS without requiring a local workstation to remain online.

A project-specific environment can contain:

- API credentials
- Provider configuration
- Channel connections
- Production settings
- Media files
- Generated outputs
- Scheduling configuration

Sensitive credentials and infrastructure configuration are not included in this repository.

## Operational Model

<pre>
Production Configuration
          │
          ▼
       Scheduler
          │
          ▼
 Research & Generation
          │
          ▼
     Media Assembly
          │
          ▼
 Production Controls
          │
     ┌────┴────┐
     │         │
   PASS       FAIL
     │         │
     ▼         ▼
 Publishing   Stop
 Preparation  / Review
</pre>

The production pipeline therefore includes an explicit control boundary before automated publishing.

## Engineering Principles

### Research Before Generation

Content production begins with a research stage rather than relying exclusively on model memory.

### Format-Specific Production

Short-form and long-form videos have independent production requirements.

### Provider Separation

AI services are treated as configurable components with defined responsibilities.

### Cost-Aware Automation

API consumption is constrained through configurable production budgets.

### Explicit Quality Gates

Critical production rules are evaluated before publishing.

### Isolated Runtime

The production environment can operate independently on dedicated infrastructure.

## Public vs. Proprietary

This repository is a public technical showcase of a proprietary TankDev production system.

**Publicly documented**

- System architecture
- Production stages
- Workflow boundaries
- Provider model
- Budget-control approach
- Publishing architecture
- Operational principles

**Not publicly distributed**

- Production source code
- API credentials
- Channel credentials
- Infrastructure secrets
- Internal prompts
- Proprietary orchestration logic
- Provider-specific implementation details
- Security-sensitive configuration

## Performance Boundaries

This system automates content-production and publishing operations.

It does **not** guarantee:

- Views
- Subscribers
- Engagement
- Revenue
- Search rankings
- Platform distribution

Content performance depends on factors outside the production system, including topic selection, audience behavior, market conditions, and platform algorithms.

## Case Study

A detailed engineering case study covering the problem, production architecture, operational controls, and implemented system is available on TankDev:

**[Read the Autonomous AI Video Studio Case Study](https://tankdev.tech/tr/case-studies/ai-video-automation)**

## System Overview

A product-oriented overview of the system, production stages, controls, and deployment model is available here:

**[Explore the Autonomous AI Video Studio](https://tankdev.tech/tr/systems/ai-video-automation)**

## About TankDev

[TankDev](https://tankdev.tech) develops custom software systems around real operational requirements, business rules, data flows, and system boundaries.

TankDev works across **custom software, applied artificial intelligence, process automation, web applications, and system integration**.

---

Developed by **[TankDev](https://tankdev.tech)**  
[Website](https://tankdev.tech) · [Case Studies](https://tankdev.tech/tr/case-studies)
