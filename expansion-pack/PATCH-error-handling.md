# PATCH: Error Handling Additions

**Add to**: `SKILL-error-handling.md`

---

## Addition: Structured Logging Beyond Errors

**Insert before**: the "Custom Error Types" section

---

## Structured Logging Beyond Errors

The three-point error pattern (log, surface, throw) covers error cases. For non-error logging, follow these standards:

### Log Levels

| Level | When to Use | Example |
|-------|------------|---------|
| `error` | Operation failed, requires attention | `❌ Failed to parse order: missing customer id` |
| `warn` | Unexpected but recoverable situation | `⚠️ Retry attempt 2/3 for API call` |
| `info` | Significant business events | `✅ Order processed: 3 items, total $108.00` |
| `debug` | Diagnostic detail for development | `Cart parsed: { items: 3, currency: 'USD' }` |

### Structured Format

```typescript
// ❌ FORBIDDEN — Unstructured, unsearchable logs
console.log('User did thing');
console.log('Processing: ' + JSON.stringify(bigObject));

// ✅ REQUIRED — Structured with context
function log(level: 'info' | 'warn' | 'error' | 'debug', message: string, context?: Record<string, unknown>) {
  const entry = {
    timestamp: new Date().toISOString(),
    level,
    message,
    ...context,
  };
  console[level](JSON.stringify(entry));
}

// Usage
log('info', 'Order processed', { itemCount: 3, total: 108.0, durationMs: 1230 });
log('warn', 'API retry', { attempt: 2, maxAttempts: 3, endpoint: '/api/sync' });
```

### Correlation IDs

For operations that span multiple functions or services, use correlation IDs to trace the full chain:

```typescript
// ✅ REQUIRED — Correlation ID threads through the operation
async function handleRequest(req: Request): Promise<Response> {
  const correlationId = req.headers['x-correlation-id'] || crypto.randomUUID();

  log('info', 'Request received', { correlationId, method: req.method, path: req.path });

  try {
    const result = await processRequest(req, correlationId);
    log('info', 'Request completed', { correlationId, status: 200 });
    return result;
  } catch (error) {
    log('error', 'Request failed', {
      correlationId,
      error: error instanceof Error ? error.message : String(error),
    });
    throw error;
  }
}
```

### What NEVER to Log

```typescript
// ❌ FORBIDDEN — Sensitive data in logs
log('info', 'User authenticated', { token: jwtToken });          // Leaks token
log('debug', 'Config loaded', { config: JSON.stringify(config) }); // May include secrets
log('info', 'Payment processed', { cardNumber: '4111...' });      // PCI violation

// ✅ REQUIRED — Redact sensitive fields
log('info', 'User authenticated', { userId: user.id, role: user.role });
log('debug', 'Config loaded', { endpoint: config.apiUrl, region: config.region });
log('info', 'Payment processed', { amount: 99.99, currency: 'USD', last4: '1234' });
```
