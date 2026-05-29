# Product Requirements Document (PRD)
## NCPL HR Dashboard Migration

### 1. Executive Summary
**Project Overview**
This project outlines the strategic migration of the NCPL HR Birthday Dashboard's core conversational and text-generation engines to Anthropic's Claude API. The dashboard currently automates employee birthday celebration assets (flyers, videos, and reminders) for the HR team. 

**Migration Objective**
To replace the existing AI text and prompt generation engines within the dashboard with the Claude API, optimizing for highly nuanced, human-like corporate communication, advanced context windows, and stable prompt adherence.

**Expected Business Impact**
- Reduction in manual editing time required by HR personnel for generated flyers and emails.
- Increased personalization and emotional resonance in employee communications.
- Higher reliability and consistency in automated generative workflows.

**Key Benefits of Moving to Claude**
- **Nuanced Tone Control:** Claude's alignment allows for highly professional, warm, and corporate-appropriate birthday messaging with minimal prompt engineering.
- **Extended Context:** Ability to feed dense employee history and detailed corporate brand guidelines into the AI for highly accurate and customized output generation.
- **Consistent Formatting:** Strong adherence to rigid output schemas (JSON) for seamless automated dashboard rendering.

### 2. Problem Statement
**Current Limitations in AI Studio**
- Lengthy prompt iteration cycles required to maintain strict and warm corporate tones.
- Occasional hallucinations or formatting deviations in JSON outputs that break automated rendering pipelines in the dashboard.

**Existing Workflow Challenges**
- HR teams occasionally need to manually rewrite AI-generated flyer content because the AI phrasing feels either too generic or overly dramatic.
- Automation triggers (e.g., WhatsApp reminders) sometimes generate with inconsistent length or structure.

**Performance or Maintainability Issues**
- Maintaining complex role-playing prompts for video storyboarding and text generation requires constant tweaking.

### 3. Goals & Objectives
**Functional Goals**
- Fully integrate the Claude API for all text-based generation tasks (Flyer Messages, WhatsApp Reminders, Email Output, Storyboards).
- Seamlessly process multi-variable employee data (Name, Department, Seniority) into the new model.

**Technical Goals**
- Refactor the existing `geminiService.ts` or implement a parallel `claudeService.ts`.
- Ensure a response latency of under 3 seconds for text generation.
- Implement robust error handling and fallback logic for API timeouts.

**Business Goals**
- Achieve a 95% "zero-edit" rate for HR dashboard flyers and reminders.
- Ensure 100% data privacy compliance regarding employee PII sent to the model.

**User Experience Goals**
- Provide a seamless, immediate loading state without disrupting the existing React/Tailwind UI.
- Maintain the exact same 1-click generation flow currently used in the dashboard.

### 4. Scope

**In Scope**
- **Features included in migration:** Text generation for Flyers, WhatsApp messages, and Storyboard prompts.
- **AI Integrations:** Anthropic Claude API (specifically Claude 3.5 Sonnet for speed/quality balance).
- **APIs:** Node.js/Express or direct client-side integration of the Anthropic SDK.
- **Dashboard components:** Updating the generation loading states and error toasts to reflect the new pipeline.
- **Prompt workflows:** Rewriting all system instructions to be optimized for Claude's constitutional AI style.
- **Automation flows:** Synchronizing Claude's output perfectly with the automated email scheduling and download scripts.

**Out of Scope**
- **Features not included:** Video rendering pipelines (canvas rendering), image generation (Veo/Image models stay external since Claude is text-focused).
- **Future enhancements:** Full chatbot integration for HR inquiries inside the dashboard.

### 5. User Personas

**1. Admin**
- *Role:* Manages the overall configuration of the dashboard.
- *Needs:* Ability to update the Claude API key, monitor API usage limits, and configure global prompt templates.

**2. HR Team**
- *Role:* Daily driver of the application.
- *Needs:* A 1-click solution to generate birthday assets. Needs the text output to be professional, fast, and require absolutely zero manual proofreading.

**3. Employees**
- *Role:* The end recipients.
- *Needs:* To receive highly personalized, emotionally resonant, and grammatically perfect birthday flyers and videos that make them feel valued by NCPL.

**4. Super Admin**
- *Role:* High-level IT/System administrator.
- *Needs:* Oversight on data compliance, system uptime, and ability to manage role-based access to the dashboard.

**5. Technical Support Users**
- *Role:* IT troubleshooters.
- *Needs:* Clear audit logs of API requests, distinct error codes for Claude API failures vs. missing employee data, and raw JSON access to generated outputs.

### 6. Functional Requirements

**Authentication & Authorization**
- Must retain the existing Firebase Google Authentication.
- Only authorized HR domains (e.g., `@ncplconsulting.net`) can access the generation suite.

**Dashboard**
- The UI must remain clean, single-page, and intuitive.
- Provide clear visual indicators (spinners) during Claude API processing.

**AI Prompt Engine**
- Centralized prompt management system.
- Ability to inject templates with `{{name}}`, `{{department}}`, `{{seniority}}` variables before sending to Claude.

**Claude API Integration**
- Use the official `@anthropic-ai/sdk`.
- Implement streaming for faster perceived load times on long text generations.
- Securely store API keys on the server-side (Express) or require explicit user input; never expose credentials in the client bundle.

**User Management**
- Synchronize employee lists securely.
- Role-based toggles for who can edit templates vs. who can just click "Generate".

**Analytics & Reporting**
- Log generations per day.
- Track average token usage per employee generation batch to monitor costs.

**Notification System**
- Dashboard toasts indicating success or failure of Claude generation.
- Clear error surfacing (e.g., "Claude API Rate Limited").

**Workflow Automation**
- One button click must sequentially chain all required generations: Flyer Headline -> Flyer Message -> Storyboard -> Reminders.

**Role-Based Access**
- **HR:** Generate and View.
- **Admin:** Generate, View, Edit Prompts, Manage API Keys.

**Data Storage**
- Continue utilizing Firebase/Firestore for persisting employee profiles and the final generated output strings.

**Audit Logs**
- Maintain a secure Firebase collection (`api_logs`) tracking timestamp, user, generated employee ID, and success status for compliance.
