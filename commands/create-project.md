---
name: create-project
description: Create a new Inliner project namespace
---

# Create Project

Use this during onboarding when users do not yet have a namespace.

1. Collect `project` (namespace), `displayName`, optional `description`, optional `isDefault`.
2. Call `create_project`.
3. Return created namespace and suggested next step: generate a test image.
