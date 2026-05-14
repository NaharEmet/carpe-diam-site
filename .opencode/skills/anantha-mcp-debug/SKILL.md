<!-- fullWidth: false tocVisible: false tableWrap: true -->
---
name: anantha-mcp-debug
description: System debugging and diagnostics for AnanthaOS using MCP tools
---

# AnanthaOS MCP Debugging Skill

Debug AnanthaOS issues using Model Context Protocol tools for real-time system diagnostics.

## When to Use This Skill

- System behaving unexpectedly
- Workflow execution failing
- Messages not being processed
- Need to investigate production issues
- Performance problems

## Debug Checklist

Always run diagnostics in this order:

1. **System Health** - Check logs for errors
2. **Workflow Status** - Verify workflows are operational
3. **Contact Verification** - Confirm actors exist
4. **Message Flow** - Test message processing
5. **Error Analysis** - Deep dive into issues

## Diagnostic Tools

### 1. System Health Check

Get recent logs to identify issues:

```json
{
  "name": "get_logs",
  "arguments": {
    "limit": 20
  }
}
```

Filter by level:

```json
{
  "name": "get_logs",
  "arguments": {
    "level": "error",
    "limit": 50
  }
}
```

### 2. Workflow Diagnostics

List all workflows:

```json
{
  "name": "list_workflows",
  "arguments": {
    "org_id": "test_org",
    "status": "all"
  }
}
```

Get specific workflow:

```json
{
  "name": "get_workflow",
  "arguments": {
    "workflow_id": "problematic_workflow",
    "org_id": "test_org"
  }
}
```

Check workflow status:

```json
{
  "name": "get_workflow_status",
  "arguments": {
    "workflow_id": "problematic_workflow",
    "org_id": "test_org"
  }
}
```

### 3. Contact/Actor Verification

List contacts:

```json
{
  "name": "list_contacts",
  "arguments": {
    "org_id": "test_org"
  }
}
```

Search for specific contact:

```json
{
  "name": "list_contacts",
  "arguments": {
    "org_id": "test_org",
    "search": "actor_name"
  }
}
```

### 4. Message Flow Analysis

Check conversation history:

```json
{
  "name": "get_conversation_history",
  "arguments": {
    "org_id": "test_org",
    "actor_id": "test_actor",
    "limit": 10
  }
}
```

Test message processing:

```json
{
  "name": "get_system_reply",
  "arguments": {
    "org_id": "test_org",
    "actor_id": "test_actor",
    "content": "Test message for debugging"
  }
}
```

## Common Issues & Solutions

### Issue: Workflow Not Found

**Symptoms**: `get_workflow` returns "Not found"

**Diagnosis**:
1. List workflows to see available ones
2. Check if workflow belongs to correct org
3. Verify workflow status

**Solution**:
- Use correct org_id
- Check workflow is not deleted
- Verify user permissions

### Issue: Message Not Processed

**Symptoms**: Messages not generating responses

**Diagnosis**:
1. Check actor exists: `list_contacts`
2. View conversation history
3. Check Orchestrator logs

```json
{
  "name": "get_logs",
  "arguments": {
    "level": "error",
    "component": "Orchestrator",
    "limit": 20
  }
}
```

**Solution**:
- Verify actor_id is correct
- Check workflow triggers are configured
- Review message routing rules

### Issue: System Unresponsive

**Symptoms**: No replies, timeouts

**Diagnosis**:
1. Get recent error logs
2. Check system health
3. Test with simple message

**Solution**:
- Restart MCP server if needed
- Check database connectivity
- Verify all services running

## Debug Session Template

Use this structured approach:

```
Debug AnanthaOS issue:

**Context**: [Describe the problem]

**Investigation Steps**:
1. Get last 20 system logs
2. List workflows for org "test_org"
3. List contacts for org
4. Get conversation history for affected actor
5. Test message processing
6. Analyze error logs

**Findings**:
[Document what you discovered]

**Root Cause**:
[Identify the underlying issue]

**Fix Applied**:
[Describe the solution]

**Verification**:
[Confirm issue is resolved]
```

## Debug Report Format

Provide findings in this structure:

```markdown
## Debug Report: [Issue Title]

### System Status: [🟢 Healthy / 🟡 Degraded / 🔴 Critical]

### Issues Found:
1. **[Issue Name]**
   - Severity: High/Medium/Low
   - Evidence: [Log entries or data]
   - Impact: [What it affects]
   - Recommended Fix: [Action to take]

### Component Health:
| Component | Status | Notes |
|-----------|--------|-------|
| Workflows | ✅/❌ | [count] active |
| Messages | ✅/❌ | Processing [yes/no] |
| Contacts | ✅/❌ | [count] available |
| Logs | ✅/❌ | [error count] errors |

### Actions Taken:
1. [Step 1]
2. [Step 2]

### Resolution:
[Final outcome]
```

## Advanced Debugging

### Component-Specific Logs

Filter by component:

```json
{
  "name": "get_logs",
  "arguments": {
    "component": "Orchestrator",
    "level": "warning",
    "limit": 30
  }
}
```

### Time-Based Analysis

Check logs for specific period:

```json
{
  "name": "get_logs",
  "arguments": {
    "since": "2024-01-01T00:00:00Z",
    "until": "2024-01-02T00:00:00Z",
    "level": "error"
  }
}
```

### Workflow Trigger Testing

Manually trigger workflow:

```json
{
  "name": "trigger_workflow",
  "arguments": {
    "workflow_id": "test_workflow",
    "org_id": "test_org",
    "actor_id": "test_actor",
    "input_context": {"test": "data"}
  }
}
```

## Best Practices

1. **Start Broad, Then Narrow**
   - Begin with general health check
   - Drill down to specific components
   - Isolate the root cause

2. **Document Everything**
   - Log all diagnostic steps
   - Record tool outputs
   - Note timestamps

3. **Test Fixes Immediately**
   - Verify solution works
   - Check for side effects
   - Confirm no new issues introduced

4. **Prevent Future Issues**
   - Identify patterns
   - Suggest monitoring improvements
   - Recommend preventive measures

## Related Resources

- [OPENCODE_MCP_GUIDE.md](../../../OPENCODE_MCP_GUIDE.md)
- [MCP_TEST_SCENARIOS.md](../../../MCP_TEST_SCENARIOS.md)
- `prompts/mcp_debug.prompt`
