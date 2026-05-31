# Security Policy

## Reporting Security Vulnerabilities

If you discover a security vulnerability in ADK documentation or related resources, please report it responsibly by emailing security@example.com instead of using the public issue tracker.

**Please include:**
- A description of the vulnerability
- Steps to reproduce (if applicable)
- Potential impact
- Suggested fix (if available)

We take all security reports seriously and will respond promptly.

## Security Considerations for ADK Users

### Event Schema & Custom Session Databases

When upgrading to ADK 2.0, ensure your database schemas accommodate new Event fields (`node_info` and `output`). If using strict JSON schema validation, update downstream validators to accept the 2.0 Event format to prevent validation failures.

### Error Handling & Exception Management

- **Do not** catch `BaseException` unless explicitly re-raising, as this traps `NodeInterruptedError` and breaks Human-in-the-Loop (HITL) functionality
- Allow standard exceptions to propagate so the framework can apply configured retry policies
- **Do not** manually append events to sessions; use the framework's event emission mechanisms for proper state management

### Callback Execution

When overriding execution logic, use standardized `BeforeAgentCallback` and `AfterAgentCallback` interfaces rather than legacy method overrides. Direct method overrides bypass the Workflow Graph engine and may compromise security and determinism.

### Session Data Management

- Do not bypass the framework to directly manipulate session events
- Use `yield` statements to emit events from within nodes/agents for proper persistence and routing
- Ensure all reader applications are updated to handle the 2.0 Event format before writing 2.0 sessions to shared databases

## Dependency Security

Review and keep dependencies up to date. For ADK 2.0, ensure:
- **Python 3.10** or later is installed
- `pip` packages are regularly updated

## Reporting Other Security Issues

For security issues in the ADK Python project itself, visit:
https://github.com/google/adk-python/issues

---

**Last Updated:** May 2026
