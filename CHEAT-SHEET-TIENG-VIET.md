# Claude Code - Cheat Sheet (Tiếng Việt) 🚀

## Commands Cơ Bản

```bash
/plan              # Lập kế hoạch trước khi code
/tdd               # Test-Driven Development
/code-review       # Review security & quality
/build-fix         # Fix lỗi build tự động
/e2e               # Generate E2E tests (Playwright)
/refactor-clean    # Dọn dẹp dead code
/learn             # Học patterns từ session
```

## Workflows Phổ Biến

### 1️⃣ Feature Mới (Đầy Đủ)
```bash
/plan → confirm → /tdd → /code-review → /e2e → commit
```
⏱️ 30 phút - 2 giờ

### 2️⃣ Fix Bug Nhanh
```bash
/tdd → /code-review → commit
```
⏱️ 5-15 phút

### 3️⃣ Refactoring
```bash
/plan → /tdd → /refactor-clean → /code-review → commit
```
⏱️ 1-4 giờ

### 4️⃣ Security-Critical
```bash
/plan → /tdd (security tests) → /code-review (strict) → /e2e → commit
```
⏱️ 2-8 giờ

## Khi Nào Dùng Gì?

| Tình huống | Command | Lý do |
|------------|---------|-------|
| Feature mới phức tạp | `/plan` | Cần thiết kế trước |
| Feature đơn giản | `/tdd` | Viết tests & code luôn |
| Fix bug | `/tdd` | Reproduce bug với test |
| Code bẩn | `/refactor-clean` | Dọn dẹp tự động |
| Build fail | `/build-fix` | Fix errors tự động |
| Cần E2E | `/e2e` | Generate Playwright tests |
| Trước commit | `/code-review` | Security & quality check |

## TDD Cycle (Bắt Buộc!)

```
┌───────────────────────────────────┐
│  RED → GREEN → REFACTOR → REPEAT  │
└───────────────────────────────────┘

RED:      Viết test → FAIL
GREEN:    Viết code → PASS
REFACTOR: Cải thiện code
REPEAT:   Feature tiếp theo
```

**❌ KHÔNG BAO GIỜ:** Viết code trước test
**✅ LUÔN LUÔN:** Test trước, code sau

## Coverage Requirements

```
Minimum:  80%
Critical: 100% (auth, payment, financial)

Check: npm run test:coverage
```

## Security Checklist

```
☐ No hardcoded secrets (API keys, passwords)
☐ Input validation (Zod schemas)
☐ Parameterized queries (No SQL injection)
☐ httpOnly cookies (Not localStorage)
☐ CSRF protection
☐ Rate limiting
☐ XSS sanitization (DOMPurify)
☐ RLS enabled (Supabase)
```

## Code Quality Rules

```
☐ Functions ≤ 50 lines
☐ Files ≤ 800 lines
☐ No console.log in production
☐ No TODO/FIXME comments
☐ Immutability (const, không mutate arrays/objects)
☐ Error handling đầy đủ
```

## Agents Quan Trọng

| Agent | Dùng khi | Tools |
|-------|----------|-------|
| `planner` | Cần plan implementation | Read, Grep, Glob |
| `tdd-guide` | Implement với TDD | Read, Edit, Write, Bash |
| `code-reviewer` | Review quality | Read, Grep, Glob |
| `security-reviewer` | Review security | Read, Grep |
| `build-error-resolver` | Fix build errors | Read, Edit, Bash |

## Skills Quan Trọng

| Skill | Kích hoạt khi |
|-------|---------------|
| `tdd-workflow` | Viết feature, fix bug |
| `security-review` | Auth, API, user input |
| `coding-standards` | Viết code mới |
| `backend-patterns` | API, database, caching |
| `frontend-patterns` | React, Next.js |

## Ví Dụ Nhanh

### Ví dụ 1: Feature mới
```bash
# Bạn:
/plan Add user profile page with avatar upload

# Claude: [Tạo plan chi tiết]
# Bạn: proceed

# Claude: /tdd được tự động gọi
# → Viết tests
# → Implement code
# → Coverage check

/code-review  # Final check
git commit -m "feat: add user profile page"
```

### Ví dụ 2: Fix bug
```bash
# Bạn:
/tdd Fix: Shopping cart total calculation wrong

# Claude:
# 1. Viết test reproduce bug (FAIL) ✓
# 2. Fix code
# 3. Test PASS ✓
# 4. Coverage check ✓

/code-review
git commit -m "fix: cart total calculation"
```

### Ví dụ 3: Build errors
```bash
npm run build
# → 10 errors

/build-fix
# Claude tự động:
# → Parse errors
# → Fix file by file
# → Re-run build
# → Success ✓
```

## Commit Checklist

```
Trước khi commit:
☐ /code-review pass (no CRITICAL/HIGH)
☐ npm run build success
☐ npm test pass (coverage ≥80%)
☐ No console.log
☐ No secrets
☐ Clean commit message

Commit format:
feat: add feature
fix: fix bug
refactor: refactor code
test: add tests
docs: update docs
```

## Tips Vàng

### 1. Context Window
```json
// Disable unused MCPs để tiết kiệm context
{
  "disabledMcpServers": ["github", "supabase", ...]
}

Rule: ≤10 MCPs enabled, <80 tools total
```

### 2. Khi nào KHÔNG cần /plan?
```
✗ Fix typo
✗ Thêm 1 function đơn giản
✗ Bug nhỏ, rõ ràng
✗ Yêu cầu đã chi tiết

✓ Feature mới
✓ Thay đổi architecture
✓ Ảnh hưởng >3 files
✓ Nhiều cách implement
```

### 3. Commands Combo
```bash
# Full feature
/plan → /tdd → /code-review → /e2e

# Quick fix
/tdd → /code-review

# Safe refactor
/plan → /tdd → /refactor-clean → /code-review

# Security first
/plan → /tdd → /code-review (strict)
```

### 4. Gọi Agent trực tiếp
```bash
# Thay vì commands
@planner Analyze auth flow
@code-reviewer Review src/auth/
@security-reviewer Check API endpoints

# Multiple agents
@security-reviewer và @code-reviewer review PR #123
```

### 5. Hooks Hữu Ích
```bash
# Cảnh báo console.log
# Auto-format trước commit
# Kiểm tra secrets
# Lưu context trước compact
```

## Keyboard Shortcuts

```bash
↑ / ↓          # Navigate command history
Tab            # Auto-complete
Ctrl+C         # Cancel current operation
/help          # Show help
/clear         # Clear conversation
```

## Troubleshooting

### Build fails
```bash
/build-fix
# Auto-fix errors
```

### Tests failing
```bash
npm test -- --watch
# Debug từng test
```

### Context too large
```bash
# Disable unused MCPs in .claude/config.json
# Hoặc dùng /learn để compact knowledge
```

### Slow responses
```bash
# Dùng model Haiku cho simple tasks
# Giảm số MCPs enabled
# Split large files
```

## Resources

📖 [Hướng dẫn đầy đủ](./HUONG-DAN-SKILLS-TIENG-VIET.md)
🔗 [Shorthand Guide](https://x.com/affaanmustafa/status/2012378465664745795)
🔗 [Longform Guide](https://x.com/affaanmustafa/status/2014040193557471352)
💻 [GitHub Repo](https://github.com/affaan-m/everything-claude-code)

---

## Quick Reference Card

```
┌──────────────────────────────────────────────────┐
│         CLAUDE CODE - QUICK REFERENCE            │
├──────────────────────────────────────────────────┤
│ WORKFLOW MẶC ĐỊNH                                │
│                                                  │
│  New Feature:  /plan → /tdd → /code-review      │
│  Bug Fix:      /tdd → /code-review              │
│  Refactor:     /plan → /tdd → /refactor-clean   │
│  Build Error:  /build-fix                       │
│                                                  │
├──────────────────────────────────────────────────┤
│ QUY TẮC VÀNG                                     │
│                                                  │
│  ✓ Tests trước, code sau (TDD)                  │
│  ✓ Coverage ≥ 80% (100% nếu critical)           │
│  ✓ Security review bắt buộc                     │
│  ✓ Functions ≤50 lines, Files ≤800 lines        │
│  ✗ No console.log, secrets, TODOs               │
│                                                  │
├──────────────────────────────────────────────────┤
│ BEFORE COMMIT CHECKLIST                          │
│                                                  │
│  ☐ /code-review pass                            │
│  ☐ npm run build ✓                              │
│  ☐ npm test ✓                                   │
│  ☐ Coverage ≥80% ✓                              │
│  ☐ No secrets ✓                                 │
│                                                  │
└──────────────────────────────────────────────────┘
```

---

**Pro tip:** In trang này ra và dán lên tường! 📄

*Last updated: 2026-01-27*
