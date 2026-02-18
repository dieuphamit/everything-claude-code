# Hướng Dẫn Sử Dụng Skills Claude Code - Dành Cho Người Mới

## Mục Lục
1. [Giới Thiệu Tổng Quan](#giới-thiệu-tổng-quan)
2. [Các Thành Phần Chính](#các-thành-phần-chính)
3. [Use Cases Thực Tế](#use-cases-thực-tế)
4. [Workflow Workflows Chi Tiết](#workflow-workflows-chi-tiết)
5. [Tips & Tricks](#tips--tricks)

---

## Giới Thiệu Tổng Quan

Claude Code có 4 thành phần chính:

```
┌─────────────┬──────────────────────────────────────────────┐
│ Thành phần  │ Mục đích                                     │
├─────────────┼──────────────────────────────────────────────┤
│ **Rules**   │ Quy tắc luôn tuân theo (bắt buộc)           │
│ **Skills**  │ Kiến thức chuyên môn (workflows, patterns)  │
│ **Commands**│ Lệnh nhanh bắt đầu bằng /                   │
│ **Agents**  │ Trợ lý chuyên biệt xử lý task phức tạp      │
│ **Hooks**   │ Tự động hóa dựa trên sự kiện                │
└─────────────┴──────────────────────────────────────────────┘
```

### Mối Quan Hệ

```
Commands ──gọi──> Agents ──sử dụng──> Skills ──tuân thủ──> Rules
                                         ▲
                                         │
                                      Hooks (tự động kích hoạt)
```

---

## Các Thành Phần Chính

### 1. Rules (Quy tắc bắt buộc)

**Là gì?** Quy tắc luôn phải tuân theo trong mọi tình huống.

**Khi nào dùng?** Tự động áp dụng, bạn không cần gọi.

**Ví dụ:**
- `security.md` - Không được hardcode secrets
- `coding-style.md` - Dùng immutability, file tối đa 800 dòng
- `testing.md` - Bắt buộc TDD, coverage ≥80%

### 2. Skills (Kiến thức chuyên môn)

**Là gì?** Workflow và patterns cho các tình huống cụ thể.

**Khi nào dùng?** Được gọi bởi commands hoặc agents khi cần.

**Các Skills quan trọng:**

| Skill | Mô tả | Khi nào dùng |
|-------|-------|--------------|
| `tdd-workflow` | Test-driven development | Viết feature mới, fix bug |
| `security-review` | Kiểm tra bảo mật | Làm auth, API, xử lý user input |
| `coding-standards` | Best practices theo ngôn ngữ | Viết code mới |
| `backend-patterns` | Patterns cho backend | API, database, caching |
| `frontend-patterns` | Patterns cho frontend | React, Next.js components |

### 3. Commands (Lệnh nhanh)

**Là gì?** Lệnh bắt đầu bằng `/` để thực hiện task cụ thể.

**Commands phổ biến:**

```bash
/plan              # Lập kế hoạch trước khi code
/tdd               # Viết test trước, code sau
/code-review       # Review code về security & quality
/build-fix         # Fix lỗi build tự động
/e2e               # Generate E2E tests
/refactor-clean    # Dọn dẹp dead code
/learn             # Học patterns từ session hiện tại
```

### 4. Agents (Trợ lý chuyên biệt)

**Là gì?** AI agents chuyên về một lĩnh vực cụ thể.

**Agents quan trọng:**

| Agent | Chức năng | Tools được dùng |
|-------|-----------|-----------------|
| `planner` | Lập kế hoạch implementation | Read, Grep, Glob |
| `tdd-guide` | Hướng dẫn TDD | Read, Edit, Write, Bash |
| `code-reviewer` | Review code quality | Read, Grep, Glob |
| `security-reviewer` | Review bảo mật | Read, Grep |
| `build-error-resolver` | Fix lỗi build | Read, Edit, Bash |
| `e2e-runner` | Chạy E2E tests | Bash, Read |

### 5. Hooks (Tự động hóa)

**Là gì?** Script tự động chạy khi có sự kiện.

**Các loại hooks:**

```json
{
  "PreToolUse": "Trước khi dùng tool",
  "PostToolUse": "Sau khi dùng tool",
  "Stop": "Khi session kết thúc",
  "SessionStart": "Khi bắt đầu session"
}
```

**Ví dụ hook hữu ích:**
- Cảnh báo khi có `console.log`
- Tự động lưu context trước khi compact
- Kiểm tra secrets trước khi commit

---

## Use Cases Thực Tế

### USE CASE 1: Phát Triển Feature Mới (Đầy Đủ)

**Tình huống:** Bạn cần thêm tính năng "tìm kiếm sản phẩm theo ngữ nghĩa"

#### Bước 1: Lập Kế Hoạch
```
Lệnh: /plan Tôi muốn thêm tính năng tìm kiếm sản phẩm theo ngữ nghĩa

Claude sẽ:
✓ Phân tích yêu cầu
✓ Chia nhỏ thành các phases
✓ Nhận diện rủi ro
✓ Đề xuất kế hoạch
✗ CHỜ XÁC NHẬN (không code gì cả)

Bạn: "ok" hoặc "proceed"
```

**Output mẫu:**
```
# Implementation Plan

## Phase 1: Database Setup
- Thêm vector column vào products table
- Tạo index cho vector search

## Phase 2: Embedding Service
- Tích hợp OpenAI embeddings API
- Cache embeddings trong Redis

## Phase 3: Search API
- Tạo endpoint /api/search
- Implement similarity search

## Phase 4: Frontend
- Thêm search bar component
- Hiển thị kết quả real-time

Rủi ro: OpenAI rate limits, vector search performance

WAITING FOR CONFIRMATION
```

#### Bước 2: Implement với TDD
```
Lệnh: /tdd Implement search API endpoint

Claude sẽ:
1. Viết interface/types trước
2. Viết tests (sẽ FAIL)
3. Chạy tests → xác nhận FAIL
4. Viết code tối thiểu để pass tests
5. Chạy tests → xác nhận PASS
6. Refactor code
7. Kiểm tra coverage ≥80%
```

**Luồng TDD tự động:**
```typescript
// Bước 1: Interface
interface SearchRequest {
  query: string
  limit?: number
}

// Bước 2: Test (FAIL)
describe('POST /api/search', () => {
  it('returns relevant products', async () => {
    const res = await fetch('/api/search', {
      method: 'POST',
      body: JSON.stringify({ query: 'smartphone' })
    })
    expect(res.status).toBe(200)
  })
})

// Bước 3: Implementation để pass
export async function POST(request: Request) {
  const { query } = await request.json()
  const embedding = await generateEmbedding(query)
  const results = await searchByVector(embedding)
  return NextResponse.json({ results })
}

// Bước 4: Coverage check
Coverage: 85% ✓
```

#### Bước 3: Security Review
```
Lệnh: /code-review

Claude sẽ kiểm tra:
✓ Input validation (query length, type)
✓ Rate limiting
✓ SQL injection risks
✓ API key exposure
✗ Console.log statements
✗ Hardcoded secrets

Nếu có CRITICAL issues → block commit
```

#### Bước 4: Build & Deploy
```
Lệnh: npm run build

Nếu có lỗi:
Lệnh: /build-fix

Claude sẽ:
1. Đọc error output
2. Tìm file liên quan
3. Fix từng lỗi
4. Chạy lại build
5. Lặp lại cho đến khi build success
```

**Tổng kết workflow:**
```
/plan → confirm → /tdd → /code-review → build → deploy
  ↓                 ↓         ↓
planner agent   tdd-guide  code-reviewer
                  ↓
              tdd-workflow skill
                  ↓
            security.md rules
```

---

### USE CASE 2: Fix Bug Nhanh

**Tình huống:** Users báo lỗi "giỏ hàng không cập nhật"

#### Workflow ngắn gọn:
```bash
# 1. TDD ngay - viết test reproduce bug trước
/tdd Fix cart update bug: items not refreshing after add

Claude:
→ Viết test mô phỏng bug (test sẽ FAIL - tốt!)
→ Debug và fix code
→ Test PASS
→ Coverage check

# 2. Review nhanh
/code-review

# 3. Commit
git add . && git commit -m "fix: cart refresh issue"
```

**Không cần /plan vì:**
- Bug nhỏ, phạm vi rõ ràng
- 1-2 files ảnh hưởng
- Fix nhanh, không đổi architecture

---

### USE CASE 3: Refactor Code Cũ

**Tình huống:** File `OrderService.ts` 1200 dòng, khó maintain

#### Bước 1: Lập kế hoạch refactor
```
/plan Refactor OrderService.ts - split into smaller modules

Claude phân tích:
- File quá dài (>800 dòng → vi phạm rules)
- Đề xuất split thành: OrderValidator, OrderRepository, OrderNotifier
- Identify dependencies
```

#### Bước 2: Refactor từng phần với TDD
```
/tdd Extract OrderValidator from OrderService

Claude:
1. Viết tests cho OrderValidator trước
2. Extract code sang module mới
3. Tests phải pass (không break logic)
4. Clean up OrderService
```

#### Bước 3: Clean up dead code
```
/refactor-clean

Claude tự động:
→ Tìm unused imports
→ Tìm unused functions
→ Xóa commented code
→ Remove duplicate logic
```

---

### USE CASE 4: Làm Feature Có Authentication

**Tình huống:** Thêm trang admin panel với role-based access

#### Workflow bảo mật cao:

```bash
# Bước 1: Plan với security checklist
/plan Add admin panel with RBAC

# Skill tự động kích hoạt: security-review
# Claude sẽ nhắc về:
# - JWT vs Session
# - Row Level Security
# - CSRF protection
# - Rate limiting

# Bước 2: TDD với security tests
/tdd Implement admin authentication

Claude viết tests:
✓ Test unauthorized access → 401
✓ Test non-admin access → 403
✓ Test valid admin token → 200
✓ Test expired token → 401
✓ Test CSRF token

# Bước 3: Review bảo mật nghiêm ngặt
/code-review

Claude kiểm tra:
✗ Hardcoded secrets?
✗ Token in localStorage? (phải dùng httpOnly cookie)
✗ No authorization checks?
✗ SQL injection risks?
✗ Rate limiting missing?
```

**Security checklist tự động:**
```
Pre-deployment Security Checklist:
☑ No hardcoded secrets
☑ Input validation
☑ Parameterized queries
☑ httpOnly cookies
☑ CSRF protection
☑ Rate limiting
☑ RLS enabled in Supabase
☐ HTTPS enforced (production)
```

---

### USE CASE 5: Viết E2E Tests

**Tình huống:** Cần test user flow "đăng ký → tạo sản phẩm → bán"

```bash
/e2e User can signup, create product, and sell

Claude tự động:
1. Tạo file e2e/user-flow.spec.ts
2. Generate Playwright tests với:
   - Navigation
   - Form filling
   - Assertions
   - Screenshots on failure
3. Setup test data
4. Chạy tests

Output:
```typescript
// e2e/user-flow.spec.ts
test('complete user journey', async ({ page }) => {
  // Signup
  await page.goto('/signup')
  await page.fill('[name="email"]', 'test@example.com')
  await page.fill('[name="password"]', 'secure123')
  await page.click('button[type="submit"]')

  // Verify redirect
  await expect(page).toHaveURL('/dashboard')

  // Create product
  await page.click('text=New Product')
  await page.fill('[name="title"]', 'iPhone 15')
  await page.fill('[name="price"]', '999')
  await page.click('button:has-text("Publish")')

  // Verify product created
  await expect(page.locator('text=iPhone 15')).toBeVisible()

  // Make sale
  await page.click('text=iPhone 15')
  await page.click('button:has-text("Buy Now")')
  await expect(page.locator('text=Payment Successful')).toBeVisible()
})
```

---

### USE CASE 6: Fix Build Errors Tự Động

**Tình huống:** Sau khi merge code, build bị fail với 15 lỗi TypeScript

```bash
# Một lệnh duy nhất
/build-fix

Claude tự động:
1. Chạy npm run build
2. Parse error output
3. Identify files có lỗi
4. Fix từng lỗi:
   - Type mismatches
   - Missing imports
   - Unused variables
5. Re-run build
6. Repeat cho đến khi success

Output:
✓ Fixed 15 type errors
✓ Fixed 3 missing imports
✓ Removed 2 unused variables
✓ Build successful
```

---

## Workflow Workflows Chi Tiết

### Workflow A: New Feature (Đầy Đủ)

```
Kịch bản: Feature mới, phức tạp, nhiều files

┌─────────────┐
│   /plan     │ → Lập kế hoạch chi tiết
└──────┬──────┘
       │
       ↓ confirm
┌─────────────┐
│    /tdd     │ → Implement với tests
└──────┬──────┘
       │
       ↓ code done
┌─────────────┐
│/code-review │ → Security & quality check
└──────┬──────┘
       │
       ↓ pass
┌─────────────┐
│npm run build│ → Build check
└──────┬──────┘
       │
       ↓ if errors
┌─────────────┐
│ /build-fix  │ → Auto-fix build issues
└──────┬──────┘
       │
       ↓ build success
┌─────────────┐
│    /e2e     │ → E2E tests cho critical flows
└──────┬──────┘
       │
       ↓ all pass
┌─────────────┐
│git commit   │ → Commit & push
└─────────────┘
```

**Thời gian:** 30 phút - 2 giờ tùy độ phức tạp

---

### Workflow B: Quick Bug Fix

```
Kịch bản: Bug nhỏ, 1-2 files

┌─────────────┐
│    /tdd     │ → Viết test reproduce bug → fix
└──────┬──────┘
       │
       ↓
┌─────────────┐
│/code-review │ → Quick check
└──────┬──────┘
       │
       ↓
┌─────────────┐
│git commit   │ → Done
└─────────────┘
```

**Thời gian:** 5-15 phút

---

### Workflow C: Refactoring

```
Kịch bản: Code cũ, cần tổ chức lại

┌─────────────┐
│   /plan     │ → Plan refactor strategy
└──────┬──────┘
       │
       ↓
┌─────────────┐
│    /tdd     │ → Refactor từng module (tests đảm bảo không break)
└──────┬──────┘
       │
       ↓
┌─────────────┐
│/refactor-   │ → Dọn dẹp dead code
│   clean     │
└──────┬──────┘
       │
       ↓
┌─────────────┐
│/code-review │ → Final check
└──────┬──────┘
       │
       ↓
┌─────────────┐
│git commit   │
└─────────────┘
```

**Thời gian:** 1-4 giờ

---

### Workflow D: Security-Critical Feature

```
Kịch bản: Auth, Payment, API với sensitive data

┌─────────────┐
│   /plan     │ → Plan với security considerations
└──────┬──────┘
       │
       ↓ (security-review skill tự động active)
┌─────────────┐
│    /tdd     │ → TDD với security tests ưu tiên
│             │   - Test unauthorized access
│             │   - Test injection attacks
│             │   - Test rate limiting
└──────┬──────┘
       │
       ↓
┌─────────────┐
│/code-review │ → STRICT security review
└──────┬──────┘
       │
       ↓ pass all security checks
┌─────────────┐
│npm audit    │ → Check dependencies
└──────┬──────┘
       │
       ↓
┌─────────────┐
│    /e2e     │ → Security E2E tests
└──────┬──────┘
       │
       ↓
┌─────────────┐
│Deploy       │
└─────────────┘
```

**Thời gian:** 2-8 giờ (không được rush!)

---

## Tips & Tricks

### 1. Khi Nào Cần /plan?

**CẦN /plan:**
- ✅ Feature mới, chưa rõ approach
- ✅ Thay đổi architecture
- ✅ Ảnh hưởng >3 files
- ✅ Có nhiều cách implement khác nhau

**KHÔNG CẦN /plan:**
- ✗ Fix typo, bug nhỏ
- ✗ Thêm 1 function đơn giản
- ✗ Yêu cầu đã rất chi tiết

### 2. TDD Luôn Luôn

**Nguyên tắc vàng:**
```
KHÔNG BAO GIỜ viết code trước test
```

**TDD cycle:**
```
RED (test fail) → GREEN (code pass) → REFACTOR (improve) → REPEAT
```

**Ngoại lệ duy nhất:** Prototype nhanh để demo (nhưng phải viết test sau!)

### 3. Commands Combo Mạnh

**Combo 1: Feature mới hoàn chỉnh**
```bash
/plan → /tdd → /code-review → /e2e
```

**Combo 2: Fix bug nhanh**
```bash
/tdd → /code-review
```

**Combo 3: Refactor an toàn**
```bash
/plan → /tdd → /refactor-clean → /code-review
```

**Combo 4: Security-first**
```bash
/plan → /tdd (với security tests) → /code-review (strict)
```

### 4. Coverage Requirements

```
Minimum Coverage: 80%

Yêu cầu 100% coverage cho:
✓ Financial calculations
✓ Authentication logic
✓ Payment processing
✓ Security-critical code
```

**Check coverage:**
```bash
npm run test:coverage
```

### 5. Sử Dụng Agents Trực Tiếp

Ngoài commands, bạn có thể gọi agent trực tiếp:

```bash
# Thay vì /plan, gọi agent planner
claude> @planner Analyze the authentication flow

# Thay vì /code-review
claude> @code-reviewer Review changes in src/auth/

# Multiple agents song song
claude> @security-reviewer và @code-reviewer review PR #123
```

### 6. Hooks Hữu Ích

**Hook 1: Cảnh báo console.log**
```json
{
  "matcher": "tool == \"Edit\" && file_path matches \"\\.(ts|tsx)$\"",
  "hooks": [{
    "type": "command",
    "command": "grep -n 'console.log' \"$file_path\" && echo 'Warning: Remove console.log' >&2"
  }]
}
```

**Hook 2: Auto-format trước commit**
```json
{
  "matcher": "tool == \"Bash\" && command matches \"git commit\"",
  "hooks": [{
    "type": "command",
    "command": "npm run format"
  }]
}
```

**Hook 3: Kiểm tra secrets**
```json
{
  "matcher": "tool == \"Edit\"",
  "hooks": [{
    "type": "command",
    "command": "if grep -i 'api[_-]key\\|password\\|secret' \"$file_path\"; then echo 'DANGER: Possible secret detected!' >&2; exit 1; fi"
  }]
}
```

### 7. Context Window Management

**Vấn đề:** MCP servers ăn context

**Giải pháp:**
```json
// .claude/config.json (project level)
{
  "disabledMcpServers": [
    "github",
    "supabase",
    // Chỉ enable những gì đang cần
  ]
}
```

**Rule of thumb:**
- Có thể config 20-30 MCPs
- Chỉ enable ≤10 MCPs per project
- Giữ tổng tools <80

### 8. Continuous Learning

Sau mỗi session, rút trích patterns:

```bash
/learn

Claude sẽ:
→ Analyze session
→ Extract reusable patterns
→ Suggest new skills/rules
→ Update configs
```

**Ví dụ output:**
```
Learned patterns from this session:

1. New pattern: Error handling for Stripe webhooks
   → Suggested skill: stripe-integration-patterns.md

2. Repeated pattern: Zod validation for API routes
   → Add to backend-patterns.md

3. Security issue caught: Missing rate limiting
   → Update security-review checklist
```

### 9. Debugging với Agents

**Problem:** Feature không hoạt động như mong đợi

```bash
# Debug systematic với plan mode
/plan Debug why semantic search returns irrelevant results

Claude sẽ:
1. Phân tích architecture
2. Identify potential issues:
   - Embedding quality?
   - Similarity threshold?
   - Query preprocessing?
3. Đề xuất debug steps
4. Implement fixes với TDD
```

### 10. Tài Liệu Tự Động

**Không cần viết docs thủ công!**

```bash
# Generate docs từ code
claude> @doc-updater Update README with new API endpoints

# Generate API docs
claude> @doc-updater Generate OpenAPI spec from route handlers

# Update docs after refactor
claude> @doc-updater Sync docs with code changes
```

---

## Checklist Tổng Hợp

### ✅ Trước Khi Bắt Đầu Code

```
☐ Đã hiểu rõ requirements?
☐ Feature phức tạp → cần /plan?
☐ Security-critical → note thêm security tests
☐ Đã có tests cho bug (nếu fix bug)?
```

### ✅ Trong Quá Trình Code

```
☐ Viết tests TRƯỚC code (TDD)
☐ Tests FAIL trước khi implement
☐ Tests PASS sau khi implement
☐ Coverage ≥80% (100% nếu critical)
☐ No console.log
☐ No hardcoded secrets
☐ Functions ≤50 lines
☐ Files ≤800 lines
```

### ✅ Trước Khi Commit

```
☐ /code-review đã pass (no CRITICAL/HIGH issues)
☐ npm run build success
☐ npm test pass
☐ E2E tests pass (nếu có)
☐ No secrets in git
☐ Clean commit message
```

### ✅ Trước Deploy Production

```
☐ Security checklist 100% complete
☐ All tests green
☐ Performance tested
☐ Error monitoring setup
☐ Rollback plan ready
```

---

## Ví Dụ Session Hoàn Chỉnh

### Session: "Thêm Payment với Stripe"

```bash
# 1. Planning
claude> /plan Add Stripe payment integration for marketplace

[Agent: planner]
# Implementation Plan: Stripe Payment Integration

## Phases
1. Database: Add payment_intents table
2. Backend: Stripe webhook handlers
3. Frontend: Checkout component
4. Testing: Simulate payments

## Risks
- HIGH: Webhook signature verification critical
- MEDIUM: Handling payment failures
- MEDIUM: Refund logic

## Dependencies
- Stripe API keys (production + test)
- Webhook endpoint (https required)

WAITING FOR CONFIRMATION

claude> proceed

# 2. TDD Implementation
claude> /tdd Implement Stripe payment intent creation

[Agent: tdd-guide]

## Step 1: Define Interface
```typescript
interface CreatePaymentIntentRequest {
  amount: number
  currency: string
  marketId: string
}

interface CreatePaymentIntentResponse {
  clientSecret: string
  paymentIntentId: string
}
```

## Step 2: Write Tests (FAIL)
```typescript
describe('POST /api/payments/intent', () => {
  it('creates payment intent successfully', async () => {
    const res = await POST({
      amount: 1000,
      currency: 'usd',
      marketId: 'market-123'
    })

    expect(res.status).toBe(200)
    expect(res.clientSecret).toBeDefined()
  })

  it('validates amount > 0', async () => {
    const res = await POST({ amount: -100, ... })
    expect(res.status).toBe(400)
  })
})
```

## Step 3: Run Tests → FAIL ✓

## Step 4: Implementation
[... code implementation ...]

## Step 5: Run Tests → PASS ✓

## Step 6: Coverage
Coverage: 92% ✓

# 3. Security Review
claude> /code-review

[Agent: code-reviewer]

## Security Issues Found

CRITICAL:
✗ None

HIGH:
✗ Webhook signature not verified in POST /api/webhooks/stripe
  → Fix: Add stripe.webhooks.constructEvent()

MEDIUM:
✗ No rate limiting on payment endpoint
  → Add: Rate limiter (10 req/min per user)

## Suggested Fixes
[... detailed fixes ...]

claude> Fix the HIGH issue

[Fixes applied...]

claude> /code-review again

All checks PASS ✓

# 4. E2E Tests
claude> /e2e Test complete payment flow

[Agent: e2e-runner]

Generated: e2e/payment-flow.spec.ts
✓ User can add item to cart
✓ User can proceed to checkout
✓ User can complete payment
✓ Order status updates correctly

# 5. Build
claude> npm run build

Build successful ✓

# 6. Commit
claude> Create a commit for this payment integration

[Git operations...]

✓ Staged 8 files
✓ Commit created: "feat: add Stripe payment integration"

Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>
```

**Tổng thời gian:** 45 phút
**Coverage:** 92%
**Security:** Pass all checks

---

## Kết Luận

### Nguyên Tắc Vàng

1. **Plan trước, code sau** - `/plan` cho features phức tạp
2. **Tests trước, code sau** - `/tdd` luôn luôn
3. **Security không thương lượng** - `/code-review` bắt buộc
4. **Coverage ≥80%** - Không exceptions
5. **Agents làm việc nặng** - Sử dụng commands, để agents xử lý

### Workflow Mặc Định

```
Feature mới:     /plan → /tdd → /code-review → /e2e → commit
Bug fix:         /tdd → /code-review → commit
Refactor:        /plan → /tdd → /refactor-clean → /code-review
Security:        /plan → /tdd → /code-review (strict) → /e2e
Build errors:    /build-fix → /code-review
```

### Resources

- [Shorthand Guide](https://x.com/affaanmustafa/status/2012378465664745795) - Đọc đầu tiên
- [Longform Guide](https://x.com/affaanmustafa/status/2014040193557471352) - Kỹ thuật nâng cao
- [GitHub Repo](https://github.com/affaan-m/everything-claude-code) - Code examples

---

**Chúc bạn code hiệu quả với Claude Code!** 🚀

*Tài liệu này được tạo từ phân tích toàn bộ hệ thống skills, commands, agents và rules trong everything-claude-code repository.*
