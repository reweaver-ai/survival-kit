# PATCH: Security Additions

**Add to**: `SKILL-security.md`

---

## Addition: Environment & Configuration Management

**Insert before**: the "API Security" section

---

## Environment & Configuration Management

### No Environment Logic in Business Code

Environment-specific behavior must be isolated in configuration objects, never scattered through application logic.

**FORBIDDEN:**
```typescript
// ❌ Environment checks scattered in business code
function processPayment(amount: number) {
  if (process.env.NODE_ENV === 'production') {
    return stripe.charge(amount);
  } else {
    return { success: true, mock: true }; // Fake data in non-prod!
  }
}

// ❌ Conditional behavior hidden in utilities
function getApiUrl() {
  return process.env.NODE_ENV === 'staging'
    ? 'https://staging.api.com'
    : 'https://api.com';
}
```

**REQUIRED:**
```typescript
// ✅ Configuration object — validated once at startup
interface AppConfig {
  apiUrl: string;
  stripeKey: string;
  logLevel: 'debug' | 'info' | 'warn' | 'error';
  features: {
    enableNewCheckout: boolean;
  };
}

function loadConfig(): AppConfig {
  const required = ['API_URL', 'STRIPE_KEY'];
  for (const key of required) {
    if (!process.env[key]) {
      throw new Error(`Missing required environment variable: ${key}`);
    }
  }
  return {
    apiUrl: process.env.API_URL!,
    stripeKey: process.env.STRIPE_KEY!,
    logLevel: (process.env.LOG_LEVEL as AppConfig['logLevel']) ?? 'info',
    features: {
      enableNewCheckout: process.env.ENABLE_NEW_CHECKOUT === 'true',
    },
  };
}

// ✅ Validated at startup — app crashes immediately if misconfigured
const config = loadConfig();
```

### Configuration Rules

- **Validate ALL required config at startup** — fail fast if a variable is missing
- **Never use `process.env` directly in business logic** — access through a typed config object
- **No environment-specific branching** in application code — use config flags instead
- **No default values for required secrets** — throw if missing, never substitute
- **Type your config** — use an interface, not `Record<string, string>`
