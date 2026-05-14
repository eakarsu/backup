# Audit Note — backup

## Bucket: DETECTOR_FALSE_POSITIVE

The original audit (batch_09.md) flagged `backup` as Skeleton with "0 routes, 0 AI". This was a detector blind spot — `backup` is a folder containing a previously-archived copy of the `document_management` project, which has substantial backend, AI services, and frontend.

## LLM Integration Found

A whole-project scan for `openrouter|openai|anthropic|claude|chat/completions` against `*.js *.ts *.tsx *.jsx *.py` (excluding `node_modules`, `.next`, `.git`, `dist`, `build`) returned hits in:

- `document_management/ai-services/app/config.py`
- `document_management/ai-services/app/services/openrouter_client.py`
- `document_management/ai-services/app/services/classification_service.py`
- `document_management/backend/scripts/generate-headers-openrouter.js`
- `document_management/backend/src/routes/aiWorkflow.ts`
- `document_management/backend/src/routes/setupRoutes.ts`
- `document_management/backend/src/services/OpenRouterService.ts`
- `document_management/backend/src/services/AICollaborativeService.ts`
- `document_management/backend/src/services/WorkflowAIService.ts`
- `document_management/backend/src/services/AISupplementService.ts`
- `document_management/backend/src/services/ai-document/ai-generator.service.ts`
- `document_management/backend/src/middleware/ai-document/validation.middleware.ts`
- `document_management/backend/src/controllers/ai-document/generator.controller.ts`
- `document_management/frontend/src/hooks/content-analyzer/useContentAnalysis.ts`
- `document_management/frontend/src/components/ai/AIContentAnalyzer.tsx`
- `document_management/frontend/src/types/document-workflow-tasks.ts`
- (Identical mirror under `document/document_management/...`)

Total source files (.js .ts .tsx .jsx .py): 1,327.

## Conclusion

The "no LLM integration anywhere in the backend" finding is a FALSE POSITIVE. This directory holds a backup of a real document-management product that already has a Node/TypeScript backend integrating OpenRouter plus a Python `ai-services` microservice using OpenRouter directly.

## Audit Section (batch_09.md)

The audit entry for `backup` reads:
> Domain: Unknown. Stack & Maturity: 0 routes, 0 AI. Verdict: Skeleton.

No actionable recommendations were captured because the auditor did not look inside `document/` or `document_management/`. There are no genuinely missing recommendations to apply at the `backup/` level — work should target the underlying `document_management` project, not this archive directory.

## Action Taken

NO CODE CHANGES. This is an archive folder. Recommendations:

1. Treat the live project at `/Users/erolakarsu/projects/document_management` (or wherever the canonical copy lives) as the source of truth.
2. Consider deleting the duplicated `backup/document/document_management/` (a backup-of-a-backup) to reduce disk usage.
3. If the `backup/` directory is intended as a long-term snapshot, move it outside the `projects/` tree so future audit passes do not re-scan it.

## Apply pass — implemented

Nothing was modified. Applying audit recommendations to an archive directory would diverge it from the canonical copy. Any apply work should target `/Users/erolakarsu/projects/document_management/`.

## Backlog (prioritized)

1. [HOUSEKEEPING] Move `backup/` outside `projects/` to keep audit detectors from rescanning it.
2. [HOUSEKEEPING] Delete the nested `backup/document/document_management/` (backup-of-a-backup) if confirmed redundant.

## Files touched in this pass

- `/Users/erolakarsu/projects/backup/_AUDIT_NOTE.md` (this file).

No source files were modified. Syntax: N/A.
