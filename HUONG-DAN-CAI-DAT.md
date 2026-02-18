# Hướng Dẫn Cài Đặt Claude Code - Chi Tiết

## Hiểu Về Cấu Trúc

### Claude Code Config ≠ Project Code

```
❌ SAI: Copy vào project code
d:\MyProject\
├── src\
├── package.json
└── agents\       ← KHÔNG copy vào đây
    └── planner.md

✅ ĐÚNG: Copy vào thư mục config Claude Code
C:\Users\YourName\.claude\
├── agents\       ← Copy vào đây
│   └── planner.md
├── skills\
├── rules\
└── settings.json
```

### Hai Loại Config

```
┌─────────────────────────────────────────────────────┐
│  1. USER-LEVEL (áp dụng cho TẤT CẢ projects)       │
│     Vị trí: ~/.claude/ (Windows: C:\Users\Name\.claude\) │
└─────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────┐
│  2. PROJECT-LEVEL (chỉ cho 1 project cụ thể)       │
│     Vị trí: d:\MyProject\.claude\                   │
└─────────────────────────────────────────────────────┘
```

---

## Bước 1: Tìm Thư Mục Config Claude Code

### Trên Windows:
```
C:\Users\<TênBạn>\.claude\
```

### Trên Mac/Linux:
```
~/.claude/
```

### Cách tìm nhanh:

#### Option 1: Trong Claude Code
```bash
# Trong Claude Code terminal, chạy:
echo $HOME\.claude

# Hoặc:
cd ~/.claude
pwd
```

#### Option 2: File Explorer
1. Mở File Explorer
2. Gõ vào address bar: `%USERPROFILE%\.claude`
3. Enter

Nếu thư mục chưa tồn tại → tạo thủ công:

```powershell
# Windows PowerShell
mkdir "$env:USERPROFILE\.claude"
mkdir "$env:USERPROFILE\.claude\agents"
mkdir "$env:USERPROFILE\.claude\skills"
mkdir "$env:USERPROFILE\.claude\commands"
mkdir "$env:USERPROFILE\.claude\rules"
mkdir "$env:USERPROFILE\.claude\hooks"

# Mac/Linux
mkdir -p ~/.claude/{agents,skills,commands,rules,hooks}
```

---

## Bước 2: Clone Repository

```bash
# Clone về máy
git clone https://github.com/affaan-m/everything-claude-code.git

# Hoặc tải ZIP từ GitHub
```

---

## Bước 3: Copy Files Vào User-Level Config

### Windows (PowerShell):

```powershell
# Đi vào thư mục đã clone
cd everything-claude-code

# Copy agents
Copy-Item -Path "agents\*" -Destination "$env:USERPROFILE\.claude\agents\" -Recurse

# Copy skills
Copy-Item -Path "skills\*" -Destination "$env:USERPROFILE\.claude\skills\" -Recurse

# Copy commands
Copy-Item -Path "commands\*" -Destination "$env:USERPROFILE\.claude\commands\" -Recurse

# Copy rules
Copy-Item -Path "rules\*" -Destination "$env:USERPROFILE\.claude\rules\" -Recurse
```

### Mac/Linux:

```bash
cd everything-claude-code

# Copy tất cả
cp -r agents/* ~/.claude/agents/
cp -r skills/* ~/.claude/skills/
cp -r commands/* ~/.claude/commands/
cp -r rules/* ~/.claude/rules/
```

---

## Bước 4: Setup Hooks

Hooks cần config trong file `settings.json`.

### Tìm file settings.json:

```
Windows: C:\Users\<Name>\.claude\settings.json
Mac/Linux: ~/.claude/settings.json
```

### Copy hooks config:

1. Mở file `hooks/hooks.json` từ repo
2. Copy nội dung
3. Paste vào `~/.claude/settings.json` trong phần `"hooks": []`

**Ví dụ:**

```json
{
  "model": "sonnet",
  "hooks": [
    {
      "matcher": "tool == \"Edit\" && tool_input.file_path matches \"\\.(ts|tsx|js|jsx)$\"",
      "hooks": [{
        "type": "command",
        "command": "#!/bin/bash\ngrep -n 'console\\.log' \"$file_path\" && echo '[Hook] Consider removing console.log for production' >&2"
      }]
    },
    {
      "matcher": "tool == \"Edit\" && tool_input.new_string matches \"(api[_-]?key|password|secret)\"",
      "hooks": [{
        "type": "command",
        "command": "echo '[SECURITY WARNING] Possible secret detected in edit!' >&2"
      }]
    }
  ]
}
```

---

## Bước 5: Setup MCP Servers (Optional)

MCP servers cần config trong file riêng.

### Tìm file MCP config:

```
Windows: C:\Users\<Name>\.claude.json
Mac/Linux: ~/.claude.json
```

**LƯU Ý:** File này là `.claude.json` (không phải `.claude/`)

### Copy MCP config:

1. Mở file `mcp-configs/mcp-servers.json` từ repo
2. Copy các MCP servers bạn cần
3. Paste vào `~/.claude.json`

**Ví dụ:**

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-server-github"],
      "env": {
        "GITHUB_TOKEN": "YOUR_GITHUB_TOKEN_HERE"
      }
    },
    "supabase": {
      "command": "npx",
      "args": ["-y", "@supabase/mcp-server"],
      "env": {
        "SUPABASE_URL": "YOUR_SUPABASE_URL_HERE",
        "SUPABASE_KEY": "YOUR_SUPABASE_KEY_HERE"
      }
    }
  }
}
```

**⚠️ QUAN TRỌNG:** Thay `YOUR_*_HERE` bằng API keys thật của bạn!

---

## Bước 6: Verify Installation

### Kiểm tra cấu trúc:

```powershell
# Windows
tree /F $env:USERPROFILE\.claude

# Mac/Linux
tree ~/.claude
```

**Kết quả mong đợi:**

```
.claude\
├── agents\
│   ├── planner.md
│   ├── tdd-guide.md
│   ├── code-reviewer.md
│   └── ...
├── skills\
│   ├── tdd-workflow\
│   ├── security-review\
│   └── ...
├── commands\
│   ├── plan.md
│   ├── tdd.md
│   └── ...
├── rules\
│   ├── security.md
│   ├── coding-style.md
│   └── ...
└── settings.json
```

### Test commands:

1. Mở Claude Code
2. Gõ: `/` → xem danh sách commands
3. Thử: `/plan Test` → xem có hoạt động không

---

## Bước 7: Project-Level Config (Optional)

Nếu bạn muốn config riêng cho 1 project cụ thể:

### Tạo file config trong project:

```powershell
# Trong thư mục project của bạn
cd d:\MyProject

# Tạo .claude folder
mkdir .claude

# Tạo config.json
New-Item .claude\config.json
```

### Nội dung `.claude/config.json`:

```json
{
  "disabledMcpServers": [
    "github",
    "supabase"
  ],
  "additionalRules": [
    "- Use React 18+ features only",
    "- Prefer Tailwind CSS for styling",
    "- Use Supabase for database"
  ],
  "contexts": {
    "dev": "contexts/dev.md",
    "review": "contexts/review.md"
  }
}
```

**Tính năng:**
- `disabledMcpServers`: Tắt MCPs không cần cho project này
- `additionalRules`: Rules riêng cho project
- `contexts`: Dynamic system prompts

---

## Cách Sử Dụng Commands

### ❌ KHÔNG phải chạy trong terminal:

```bash
# SAI - đây không phải bash command
$ /plan Add feature
bash: /plan: No such file or directory
```

### ✅ ĐÚNG - gọi trong Claude Code conversation:

```
Bạn (trong Claude Code):
/plan Add user authentication feature

Claude:
[Agent planner được gọi]
# Implementation Plan: User Authentication
...
```

---

## Hiểu Về Commands

### Commands là gì?

Commands = shortcuts để gọi agents với instructions cụ thể.

**Khi bạn gõ:**
```
/plan Add shopping cart
```

**Thực tế Claude Code làm:**
```
1. Đọc file ~/.claude/commands/plan.md
2. Gọi agent planner với instructions từ file đó
3. Agent planner sử dụng skills (tdd-workflow, etc.)
4. Agent tuân thủ rules (security.md, coding-style.md)
```

### Workflow:

```
Bạn gõ: /plan
    ↓
Claude đọc: commands/plan.md
    ↓
Claude gọi: agents/planner.md
    ↓
Agent dùng: skills/tdd-workflow/
    ↓
Agent tuân thủ: rules/security.md
```

---

## Ví Dụ Session Đầy Đủ

### Scenario: Thêm feature "dark mode"

```
# 1. Mở project trong Claude Code
cd d:\MyProject
claude

# 2. (Trong Claude Code conversation)

Bạn: /plan Add dark mode toggle to user settings

Claude:
[Agent: planner]
# Implementation Plan: Dark Mode Toggle

## Phases
1. Add theme context (React Context API)
2. Create toggle component
3. Add CSS variables for themes
4. Persist preference in localStorage

## Risks
- Flash of wrong theme on page load
- Need to update all components

WAITING FOR CONFIRMATION

Bạn: proceed

Claude:
[Agent: tdd-guide]
Starting TDD implementation...

Step 1: Interface
```typescript
interface ThemeContextValue {
  theme: 'light' | 'dark'
  toggleTheme: () => void
}
```

Step 2: Tests (FAIL)
...

# 3. (Sau khi implement xong)

Bạn: /code-review

Claude:
[Agent: code-reviewer]
Reviewing changes...

✓ No hardcoded values
✓ Tests passing
✓ Coverage: 85%
⚠️ Consider adding Playwright E2E test for theme toggle

All checks PASS

# 4. Commit

Bạn: Create commit for dark mode feature

Claude:
✓ git add src/context/ThemeContext.tsx
✓ git add src/components/ThemeToggle.tsx
✓ git commit -m "feat: add dark mode toggle

Implements theme switching with user preference persistence

Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>"
```

---

## Troubleshooting

### 1. Command không hoạt động

**Triệu chứng:**
```
Bạn: /plan Add feature
Claude: I don't recognize that command
```

**Giải pháp:**
```powershell
# Kiểm tra file có tồn tại không
ls $env:USERPROFILE\.claude\commands\plan.md

# Nếu không có → copy lại
Copy-Item -Path "commands\plan.md" -Destination "$env:USERPROFILE\.claude\commands\"
```

### 2. Agent không được gọi

**Triệu chứng:**
```
Command /tdd chạy nhưng không có agent tdd-guide
```

**Giải pháp:**
```powershell
# Kiểm tra agent file
ls $env:USERPROFILE\.claude\agents\tdd-guide.md

# Copy nếu thiếu
Copy-Item -Path "agents\tdd-guide.md" -Destination "$env:USERPROFILE\.claude\agents\"
```

### 3. Rules không được áp dụng

**Giải pháp:**
- Rules tự động áp dụng, không cần gọi
- Kiểm tra file có trong `~/.claude/rules/`
- Restart Claude Code

### 4. Hooks không chạy

**Giải pháp:**
```powershell
# Kiểm tra settings.json
cat $env:USERPROFILE\.claude\settings.json

# Đảm bảo có section "hooks": [...]
```

### 5. MCP servers không hoạt động

**Triệu chứng:**
```
Error: Cannot find MCP server 'github'
```

**Giải pháp:**
```powershell
# Kiểm tra .claude.json (không phải .claude/config.json!)
cat $env:USERPROFILE\.claude.json

# Đảm bảo có API keys đúng
# Đảm bảo có internet connection
```

---

## Context Window Management

### Vấn đề: Quá nhiều MCPs → Context bị đầy

**Giải pháp:**

#### Project-level: Disable MCPs không dùng

```json
// d:\MyProject\.claude\config.json
{
  "disabledMcpServers": [
    "github",      // Không dùng GitHub trong project này
    "vercel",      // Không deploy với Vercel
    "railway"      // Không dùng Railway
  ]
}
```

#### User-level: Chỉ enable cái cần

```json
// ~/.claude.json
{
  "mcpServers": {
    "github": { ... },      // Enable
    "supabase": { ... },    // Enable
    // "docker": { ... },   // Comment out = disable
    // "kubernetes": { ... } // Comment out = disable
  }
}
```

**Rule of thumb:**
- Có thể config 20-30 MCPs
- Chỉ enable ≤10 per project
- Tổng tools <80

---

## Tóm Tắt Vị Trí Files

```
┌────────────────────────────────────────────────────┐
│ USER-LEVEL CONFIG (cho tất cả projects)           │
├────────────────────────────────────────────────────┤
│ ~/.claude/                                         │
│   ├── agents/          → Copy từ repo             │
│   ├── skills/          → Copy từ repo             │
│   ├── commands/        → Copy từ repo             │
│   ├── rules/           → Copy từ repo             │
│   └── settings.json    → Thêm hooks config        │
│                                                    │
│ ~/.claude.json         → Thêm MCP servers         │
└────────────────────────────────────────────────────┘

┌────────────────────────────────────────────────────┐
│ PROJECT-LEVEL CONFIG (optional, per project)      │
├────────────────────────────────────────────────────┤
│ d:\MyProject\.claude\                              │
│   └── config.json      → Project-specific config  │
└────────────────────────────────────────────────────┘
```

---

## Checklist Cài Đặt

```
☐ Clone repo về máy
☐ Tạo thư mục ~/.claude/ (nếu chưa có)
☐ Copy agents/ vào ~/.claude/agents/
☐ Copy skills/ vào ~/.claude/skills/
☐ Copy commands/ vào ~/.claude/commands/
☐ Copy rules/ vào ~/.claude/rules/
☐ Config hooks trong ~/.claude/settings.json
☐ Config MCPs trong ~/.claude.json (và thay API keys)
☐ Test commands bằng cách gõ / trong Claude Code
☐ Test bằng lệnh đơn giản: /plan Test
☐ (Optional) Tạo project-level config .claude/config.json
```

---

## Quick Start (Nhanh Nhất)

Nếu muốn start nhanh, chỉ cần:

```powershell
# Windows
git clone https://github.com/affaan-m/everything-claude-code.git
cd everything-claude-code
Copy-Item -Path "agents\*" -Destination "$env:USERPROFILE\.claude\agents\" -Recurse -Force
Copy-Item -Path "commands\*" -Destination "$env:USERPROFILE\.claude\commands\" -Recurse -Force
Copy-Item -Path "rules\*" -Destination "$env:USERPROFILE\.claude\rules\" -Recurse -Force
Copy-Item -Path "skills\*" -Destination "$env:USERPROFILE\.claude\skills\" -Recurse -Force

# Test
claude
# Trong Claude Code, gõ: /plan Test
```

```bash
# Mac/Linux
git clone https://github.com/affaan-m/everything-claude-code.git
cd everything-claude-code
cp -r agents/* ~/.claude/agents/
cp -r commands/* ~/.claude/commands/
cp -r rules/* ~/.claude/rules/
cp -r skills/* ~/.claude/skills/

# Test
claude
# Trong Claude Code, gõ: /plan Test
```

---

## Next Steps

Sau khi cài đặt xong:

1. **Đọc:** [CHEAT-SHEET-TIENG-VIET.md](./CHEAT-SHEET-TIENG-VIET.md) để biết commands cơ bản
2. **Đọc:** [HUONG-DAN-SKILLS-TIENG-VIET.md](./HUONG-DAN-SKILLS-TIENG-VIET.md) để hiểu workflows
3. **Thử:** Chạy `/plan Test` trong Claude Code để verify
4. **Học:** Thử workflow đầy đủ với feature nhỏ

**Happy coding với Claude Code!** 🚀

---

*Tài liệu này giải thích chi tiết cách cài đặt và config everything-claude-code repository.*
