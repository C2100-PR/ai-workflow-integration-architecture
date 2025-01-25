# Workflow Configuration Guide

## Overview

This document details how to configure workflows across different platforms and systems in the integrated architecture.

## Universal Workflow Definition

Workflows are defined in YAML format for consistency and portability:

```yaml
workflow:
  name: "cross-platform-workflow"
  description: "Workflow that spans multiple platforms"
  triggers:
    - type: "event"
      source: "warp.app"
      event: "command.executed"
    - type: "schedule"
      cron: "0 9 * * *"
  
  steps:
    - name: "warp-terminal"
      platform: "warp.app"
      action: "execute_command"
      params:
        command: "git pull origin main"
    
    - name: "dr-lucy"
      platform: "medical"
      action: "update_patient_record"
      params:
        protocol_id: "PROTO-001"
        update_type: "medication"
    
    - name: "agency"
      platform: "agency"
      action: "update_project"
      params:
        project_id: "PROJ-001"
        status: "in_progress"
    
    - name: "publishing"
      platform: "anthology"
      action: "schedule_publication"
      params:
        content_id: "CONT-001"
        publish_date: "2025-02-01"
```

## Platform-Specific Configurations

### 1. Warp.app Configuration

```yaml
warp:
  commands:
    - name: "ai-assist"
      shortcut: "cmd+shift+a"
      description: "AI-powered command assistance"
      
    - name: "workflow-record"
      shortcut: "cmd+shift+r"
      description: "Record terminal workflow"
      
  integrations:
    - name: "git"
      permissions: ["read", "write"]
    - name: "filesystem"
      permissions: ["read"]
```

### 2. Dr. Lucy Configuration

```yaml
medical:
  protocols:
    - id: "PROTO-001"
      name: "Standard Patient Update"
      validation:
        - field: "medication"
          rules: ["required", "valid_medication"]
        - field: "dosage"
          rules: ["required", "within_range"]
          
  automation:
    - trigger: "new_patient_data"
      workflow: "update_records"
    - trigger: "lab_results"
      workflow: "notify_physician"
```

### 3. Agency Configuration

```yaml
agency:
  projects:
    - type: "client_workflow"
      stages:
        - name: "initiation"
          required_fields: ["client_id", "project_scope"]
        - name: "execution"
          required_fields: ["timeline", "resources"]
        
  automation:
    - trigger: "milestone_complete"
      action: "notify_stakeholders"
    - trigger: "deadline_approaching"
      action: "escalate_priority"
```

### 4. Publishing Configuration

```yaml
publishing:
  workflows:
    - type: "content_production"
      stages:
        - name: "draft"
          reviewers: ["editor", "subject_expert"]
        - name: "final"
          approvers: ["senior_editor"]
          
  distribution:
    - channel: "web"
      format: ["html", "amp"]
    - channel: "mobile"
      format: ["app", "responsive"]
```

## Model Function Access

Configure which models have access to specific functions:

```yaml
model_permissions:
  technical_model:
    allowed_functions:
      - "code_generation"
      - "system_optimization"
    platforms:
      - "warp.app"
      
  medical_model:
    allowed_functions:
      - "diagnosis_support"
      - "treatment_planning"
    platforms:
      - "dr_lucy"
      
  agency_model:
    allowed_functions:
      - "project_planning"
      - "resource_allocation"
    platforms:
      - "agency"
      
  content_model:
    allowed_functions:
      - "content_generation"
      - "editing_support"
    platforms:
      - "publishing"
```

## Integration Settings

Define how different platforms communicate:

```yaml
integration:
  event_bus:
    type: "rabbitmq"
    exchanges:
      - name: "workflow_events"
        type: "topic"
        
  api_gateway:
    rate_limit: 1000
    timeout: 30
    
  security:
    authentication:
      type: "oauth2"
      providers:
        - name: "google"
        - name: "github"
    
    encryption:
      type: "aes-256"
      key_rotation: "30d"
```