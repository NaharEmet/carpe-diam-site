<!-- fullWidth: false tocVisible: false tableWrap: true -->
---
name: anantha-mcp-monitor
description: Health monitoring for AnanthaOS using MCP tools
---

# AnanthaOS MCP Monitoring Skill

Monitor AnanthaOS health using Model Context Protocol tools.

## When to Use

- Daily health checks
- Pre/post deployment verification
- Production monitoring
- Performance tracking

## Health Check Procedure

### 1. System Logs

```json
{"name": "get_logs", "arguments": {"limit": 10}}
```

### 2. Workflow Status

```json
{"name": "list_workflows", "arguments": {"org_id": "test_org", "status": "active"}}
```

### 3. Contact Availability

```json
{"name": "list_contacts", "arguments": {"org_id": "test_org"}}
```

### 4. Message Flow Test

```json
{"name": "send_message", "arguments": {"org_id": "test_org", "actor_id": "test_actor", "content": "Health check"}}
```

## Health Indicators

| Status | Logs | Workflows | Contacts | Messages |
|--------|------|-----------|----------|----------|
| 🟢 Healthy | 0 errors | > 0 active | > 0 | Working |
| 🟡 Warning | 5+ warnings | 0 active | Slow queries | > 5s latency |
| 🔴 Critical | Any errors | All down | 0 found | Failed |

## Quick Check

```
Run health check on AnanthaOS:
1. Check last 10 logs for errors
2. List active workflows for org "test_org"
3. List contacts
4. Test message flow
5. Report health status
```

## Health Report

```markdown
# Health Report
**Status**: [🟢/🟡/🔴]
**Score**: [X/100]

## Components:
- Logs: [X errors, Y warnings]
- Workflows: [X active/total]
- Contacts: [X available]
- Messages: [pass/fail, Xs latency]

## Recommendations:
[Actions if needed]
```

## Related Skills

- `anantha-mcp-test` - Workflow testing
- `anantha-mcp-debug` - Issue investigation
