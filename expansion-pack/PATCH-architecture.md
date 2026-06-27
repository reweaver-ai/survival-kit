# PATCH: Architecture Additions

**Add to**: `SKILL-architecture.md`

---

## Addition 1: Third-Party Dependency Boundaries

**Insert before**: the "Design Patterns" section

---

## Third-Party Dependency Boundaries

Never scatter direct calls to a third-party library across your codebase. Wrap external dependencies behind internal interfaces so that swapping, upgrading, or mocking them requires changing one file — not fifty.

### The Problem

```typescript
// ❌ FORBIDDEN — axios called directly in 30+ files
// services/UserService.ts
import axios from 'axios';
const user = await axios.get('/api/users/1');

// services/ProjectService.ts
import axios from 'axios';
const project = await axios.get('/api/projects/1');

// When you switch to fetch or ky, you edit every file
```

### The Solution

```typescript
// ✅ REQUIRED — Wrap behind an internal interface
// src/http/IHttpClient.ts
export interface IHttpClient {
  get<T>(url: string, options?: RequestOptions): Promise<T>;
  post<T>(url: string, body: unknown, options?: RequestOptions): Promise<T>;
  put<T>(url: string, body: unknown, options?: RequestOptions): Promise<T>;
  delete<T>(url: string, options?: RequestOptions): Promise<T>;
}

// src/http/AxiosHttpClient.ts
import axios from 'axios'; // axios imported in ONE file
export class AxiosHttpClient implements IHttpClient {
  async get<T>(url: string, options?: RequestOptions): Promise<T> {
    const response = await axios.get(url, options);
    return response.data;
  }
  // ...
}

// services/UserService.ts — depends on interface, not axios
export class UserService {
  constructor(private http: IHttpClient) {}
  async getUser(id: string): Promise<User> {
    return this.http.get<User>(`/api/users/${id}`);
  }
}
```

### Which Dependencies to Wrap

| Wrap Behind Interface | Use Directly |
|----------------------|-------------|
| HTTP clients (axios, fetch, ky) | Language builtins (Array, Map, Set) |
| Databases and ORMs | Utility libraries (lodash, date-fns) |
| Logging frameworks | Type-only dependencies (interfaces, types) |
| Message queues and event buses | Build-time-only tools (eslint, prettier) |
| Authentication providers | |
| File storage (S3, local FS) | |
| External SaaS APIs | |

**Rule of thumb**: If you'd need to change it during a migration, it should be behind an interface.

---

## Addition 2: Git & Commit Hygiene

**Insert before**: the "Code Organization Checklist" section

---

## Git & Commit Hygiene

### Atomic Commits

Each commit should represent one logical change. AI agents will happily make a single massive commit mixing refactoring, features, and bug fixes. Don't allow this.

```bash
# ❌ FORBIDDEN — One commit that does everything
git commit -m "Updated stuff"
git commit -m "Fixed things and added feature and refactored"

# ✅ REQUIRED — Atomic, descriptive commits
git commit -m "refactor: extract PriceCalculator from OrderService"
git commit -m "feat: add percentage discount to PriceCalculator"
git commit -m "fix: handle negative prices in PriceCalculator"
```

### Commit Message Format

Use conventional commits (`type: description`):

| Type | Usage |
|------|-------|
| `feat` | New feature or capability |
| `fix` | Bug fix |
| `refactor` | Code change that neither fixes nor adds |
| `test` | Adding or updating tests |
| `docs` | Documentation only |
| `chore` | Build, CI, dependency changes |

### Rules

- **Never mix refactoring with feature changes** in the same commit
- **Never commit broken code** — every commit should build and pass tests
- **Never commit generated files** — build artifacts, compiled outputs, node_modules
- **Keep commits reviewable** — if a diff is >400 lines, break it up
