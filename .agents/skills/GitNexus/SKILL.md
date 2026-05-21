```markdown
# GitNexus Development Patterns

> Auto-generated skill from repository analysis

## Overview
This skill teaches the core development patterns and conventions used in the GitNexus TypeScript codebase. You'll learn how to structure files, write and organize code, follow commit message standards, and execute common workflows—especially around error handling and refactoring in the worker pool logic. This guide is designed to help maintain consistency and clarity for contributors.

## Coding Conventions

### File Naming
- Use **PascalCase** for file names.
  - Example: `ParsingProcessor.ts`, `WorkerPool.ts`

### Import Style
- Use **relative imports** for internal modules.
  - Example:
    ```typescript
    import { WorkerPool } from './workers/WorkerPool';
    ```

### Export Style
- Use **named exports**.
  - Example:
    ```typescript
    export function parseRepository() { ... }
    export const WORKER_POOL_SIZE = 4;
    ```

### Commit Messages
- Use **Conventional Commits** with prefixes such as `chore` and `fix`.
  - Example:
    ```
    fix: handle worker crash on malformed input
    chore: refactor error handling in worker pool
    ```

## Workflows

### Worker Logic Hardening and Refactor
**Trigger:** When someone introduces or modifies error handling in worker pool logic, then follows up with a cleanup/refactor to align with existing patterns and reduce code verbosity.  
**Command:** `/refactor-worker-error-handling`

1. **Implement or modify error/crash handling logic**  
   Update files such as `gitnexus/src/core/ingestion/parsing-processor.ts` and `gitnexus/src/core/ingestion/workers/worker-pool.ts` to improve error detection and handling.
   ```typescript
   // Example: Adding error handling in WorkerPool
   worker.on('error', (err) => {
     logger.error(`Worker crashed: ${err.message}`);
     // Additional recovery logic...
   });
   ```

2. **Update or add callbacks, logging, and progress tracking**  
   Ensure that all error cases are logged and that callbacks reflect error states.
   ```typescript
   function onWorkerError(error: Error) {
     progressTracker.fail();
     logger.warn('Worker failed during ingestion', error);
   }
   ```

3. **Refactor for brevity and consistency**  
   In a subsequent commit, clean up the new logic: collapse redundant branches, reduce comment verbosity, and align documentation with sibling code.
   ```typescript
   // Before
   if (error) {
     // handle error
   } else {
     // handle success
   }

   // After (refactored)
   if (error) return handleError(error);
   handleSuccess();
   ```

## Testing Patterns

- Test files follow the `*.test.*` naming pattern.
  - Example: `WorkerPool.test.ts`
- The testing framework is **unknown** (not detected), but tests are likely colocated with implementation files.
- To write a test:
  ```typescript
  // WorkerPool.test.ts
  import { WorkerPool } from './WorkerPool';

  describe('WorkerPool', () => {
    it('should handle worker errors gracefully', () => {
      // test logic here
    });
  });
  ```

## Commands

| Command                        | Purpose                                                      |
|--------------------------------|--------------------------------------------------------------|
| /refactor-worker-error-handling| Refactor and harden worker pool error handling and logic      |
```
