<div align="center">
<img width="200" height="200" alt="arbiterLogo" src="https://github.com/user-attachments/assets/ba0b1220-6d37-4120-9f26-4d4c75dd383a" />

  # Arbiter

  ### Autonomous GitHub Issue Solver

  *Let AI solve your issues while you sleep. Wake up to pull requests ready for review.*

  [![Website](https://img.shields.io/badge/Website-git--arbiter.com-blue?style=flat-square)](https://git-arbiter.com)
  [![Discord](https://img.shields.io/badge/Discord-Join%20Community-5865F2?style=flat-square&logo=discord&logoColor=white)](https://discord.gg/placeholder)
  [![License](https://img.shields.io/badge/License-Commercial-orange?style=flat-square)](https://git-arbiter.com/terms.html)

  [Website](https://git-arbiter.com) • [Documentation](https://git-arbiter.com/docs.html) • [Discord](https://discord.gg/Jt78QpnV) • [Get License](https://git-arbiter.com/#pricing)

</div>

---

## 🎯 What is Arbiter?

Arbiter is a **local-first automation system** that monitors your GitHub repositories, picks up labeled issues, and autonomously creates pull requests to solve them. It runs on your machine, uses your AI provider of choice, and handles all the Git workflow automatically.

**Label an issue** → Arbiter picks it up → **AI solves it** → **PR ready for review**

### ✨ Key Features

- 🤖 **Autonomous Issue Resolution** - Label issues with `agent-ok` and let Arbiter handle the rest
- 🧠 **Multi-AI Support** - Works with Claude Code, Cursor, Ollama, and LM Studio
- 🔒 **Local-First** - Your code never leaves your machine
- 📊 **Web Dashboard** - Monitor progress, configure settings, view logs in real-time
- 🔄 **PR Iterations** - Comment-based commands to request changes on existing PRs
- ✅ **Automated Testing** - Runs your test suite before committing
- 🎨 **AI Code Review** - Optional second AI instance reviews PRs before posting
- 🔐 **Security Layer** - Protected commands and configurable policies

---

## 🚀 Quick Start

### Prerequisites

- **Docker** and **Docker Compose** installed
- **GitHub Personal Access Token** with repo access ([Create one here](https://github.com/settings/tokens))
- **AI Provider** (choose one):
  - [Claude Code](https://claude.ai/code) - Recommended for best quality
  - [Cursor](https://cursor.sh) - Great for integrated workflows
  - [Ollama](https://ollama.com) - Free, local models
  - [LM Studio](https://lmstudio.ai) - Free, local models with GUI

### Installation

**1. Pull and run Arbiter:**

```bash
docker run -d \
  --name arbiter \
  -p 3927:3927 \
  -v ~/.arbiter:/home/arbiter/.arbiter \
  -v ~/.ssh:/home/arbiter/.ssh:ro \
  gitarbiter/arbiter:latest
```

**2. Access the web dashboard:**

Open your browser to **http://localhost:3927**

**3. Configure your setup:**

1. Navigate to the **Configuration** tab
2. Add your **GitHub token**
3. Add repositories to monitor (e.g., `owner/repo`)
4. Configure your AI worker (Claude Code, Ollama, etc.)
5. Click **Save Configuration**

**4. Verify everything works:**

Click **Run Doctor Checks** to verify:
- ✅ GitHub token is valid
- ✅ AI worker is accessible
- ✅ Repository permissions are correct

**5. Start solving issues:**

- Label a GitHub issue with `agent-ok`
- Click **Run Now** in the dashboard
- Watch Arbiter solve the issue and create a PR!

---

## 📖 Configuration

### GitHub Setup

```yaml
github:
  token: ghp_xxxxx                    # Your GitHub Personal Access Token
  repos:                              # Repositories to monitor
    - owner/repo1
    - owner/repo2
  requiredLabel: agent-ok             # Label to trigger Arbiter
```

**Create a GitHub Token:**
1. Go to [GitHub Settings → Tokens](https://github.com/settings/tokens)
2. Click **Generate new token (classic)**
3. Grant `repo` scope (full repository access)
4. Copy the token and paste it in the Configuration tab

### AI Worker Setup

Arbiter supports multiple AI providers. Choose the one that fits your needs:

#### 🎯 Claude Code (Recommended)

Best quality, paid API costs.

```yaml
worker:
  type: claude-code
  model: sonnet                       # sonnet | opus | haiku
  maxTurns: 50
  maxBudgetUsd: 5.0
```

**Setup:**
1. Install [Claude Code CLI](https://claude.ai/download)
2. Authenticate: `claude auth login`
3. Configure in Arbiter dashboard

#### 🎨 Cursor Agent

Great for developers already using Cursor.

```yaml
worker:
  type: cursor-agent
  model: claude-sonnet-4-5
  maxTurns: 50
```

**Setup:**
1. Install [Cursor](https://cursor.sh)
2. Authenticate Cursor with your Claude API key
3. Configure in Arbiter dashboard

#### 🦙 Ollama (Free, Local)

Run models locally on your machine. Free, but requires good hardware.

```yaml
worker:
  type: ollama
  url: http://host.docker.internal:11434
  model: llama3.1:8b-instruct-q8_0
  maxTurns: 30
```

**Setup:**
```bash
# Install Ollama
curl -fsSL https://ollama.com/install.sh | sh

# Pull a model
ollama pull llama3.1:8b-instruct-q8_0

# Start server
ollama serve
```

**Recommended Models:**
- `llama3.1:8b-instruct-q8_0` - Fast, good for simple tasks
- `llama3.1:70b-instruct-q8_0` - Better reasoning (requires powerful GPU)
- `qwen2.5-coder:7b-instruct` - Code-focused model

#### 🖥️ LM Studio (Free, Local)

Local models with a user-friendly GUI.

```yaml
worker:
  type: lmstudio
  url: http://host.docker.internal:1234
  model: model-name
  maxTurns: 30
```

**Setup:**
1. Download [LM Studio](https://lmstudio.ai/)
2. Download a chat/instruct model
3. Load model and start local server
4. Configure in Arbiter dashboard

### Repository Policies

Configure per-repo settings in the **Repositories** tab:

```yaml
repos:
  owner/repo1:
    enabled: true                     # Enable/disable monitoring
    allowPush: true                   # Push branches to GitHub
    allowOpenPr: true                 # Open pull requests
    openPrAsDraft: true               # Open PRs as drafts
    reviewers:                        # Auto-request reviewers
      - username1
    checks:                           # Run before committing
      - npm test
      - npm run lint
    aiReviewer:                       # AI code review
      enabled: true
      mode: normal                    # lax | normal | hyper-critical
```

---

## 💡 Usage

### Basic Workflow

1. **Label an issue** in GitHub with `agent-ok`
2. **Arbiter picks it up** on the next run
3. **AI solves the issue** by reading code, making changes, running tests
4. **PR is created** as a draft for your review
5. **Review and merge** when satisfied

### Dashboard Features

#### 🏃 Current Run
- Real-time status and logs
- "Jump In" to view live worker output
- Stop running tasks if needed

#### ⚙️ Configuration
- Edit all settings via UI
- Manage GitHub repos
- Configure AI worker
- Test notifications

#### 📦 Repositories
- View all monitored repos
- Enable/disable per repo
- Configure policies (push, PR, checks)
- Set up AI code review

#### 📊 Logs
- Query run history
- Filter by date, repo, status
- View detailed worker output
- Export logs for debugging

### CLI Commands (Inside Container)

```bash
# Check system health
docker exec arbiter node dist/cli.js doctor

# Run once manually
docker exec arbiter node dist/cli.js run

# View current configuration
docker exec arbiter cat ~/.arbiter/config.yaml
```

---

## 🔧 Advanced Features

### PR Iteration Support

Arbiter can iterate on existing PRs based on your comments:

**Comment Commands:**
- `/arbiter-change <instructions>` - Request changes to the PR
- `/arbiter-review` - Request AI code review
- `#arbiter-note <text>` - Add context for next iteration

**Example:**
```
/arbiter-change Please add error handling for the API call and update the tests accordingly
```

Arbiter will:
1. Read your comment
2. Re-run the AI worker with your instructions
3. Push new commits to the same PR
4. Comment when complete

### Auto-Run Scheduler

Run Arbiter automatically at intervals:

1. Go to **Dashboard** tab
2. Set interval (e.g., 60 minutes)
3. Click **Start Auto-Run**
4. Arbiter runs every N minutes

Perfect for overnight processing or continuous issue resolution.

### AI Code Review

Enable a second AI instance to review PRs before posting:

```yaml
repos:
  owner/repo:
    aiReviewer:
      enabled: true
      mode: normal                    # lax | normal | hyper-critical
```

The reviewer checks for:
- Security vulnerabilities
- Logic errors
- Code quality issues
- Missing edge cases

Results posted as GitHub review (Approve / Request Changes / Comment).

### Desktop Notifications

Get notified when runs complete:

```yaml
notifications:
  desktop:
    enabled: true
    onSuccess: true
    onFailure: true
  browser:
    enabled: true                     # In-dashboard notifications
```

---

## 🔒 Security

Arbiter takes security seriously:

### Local-First Architecture
- Your code **never leaves your machine**
- All processing happens locally
- Only GitHub API calls go to the internet

### Protected Commands
- Command allowlisting for safety
- Forbidden path protection (e.g., `.github/workflows/**`)
- Diff size limits (max files/lines changed)
- Draft PRs by default for human review

### Comment Authorization
- Only repo members with **write access** can use PR iteration commands
- Unauthorized comments get 😕 reaction and are ignored

### Token Security
- GitHub token stored locally in `~/.arbiter/config.yaml`
- Never logged or exposed in output
- Only used for GitHub API calls

---

## 📚 How It Works

1. **Scan GitHub** - Arbiter checks your repos for issues labeled `agent-ok`
2. **Prioritize** - Issues sorted by labels, age, and custom rules
3. **Create Workspace** - Clone repo, create feature branch
4. **AI Worker** - Claude/Ollama/LM Studio reads the issue and implements solution
5. **Validate** - Check diff size, run tests, validate changes
6. **AI Review** (optional) - Second AI reviews the code
7. **Create PR** - Open pull request as draft
8. **Iterate** (if needed) - Respond to your comments with additional commits

---

## 🆘 Support

### Getting Help

- 💬 **Discord Community**: [Join our Discord](https://discord.gg/Jt78QpnV)
- 📧 **Email Support**: support@git-arbiter.com
- 📖 **Documentation**: [git-arbiter.com/docs.html](https://git-arbiter.com/docs.html)
- 🌐 **Website**: [git-arbiter.com](https://git-arbiter.com)

### Common Issues

**Arbiter isn't finding issues:**
- Verify issues have the `agent-ok` label
- Check GitHub token has repo access
- Run `docker exec arbiter node dist/cli.js doctor`

**AI worker not responding:**
- Verify Claude Code / Ollama / LM Studio is running
- Check worker configuration in dashboard
- View logs in the Logs tab

**Permission errors:**
- Ensure GitHub token has `repo` scope
- Verify Docker volumes are mounted correctly
- Check file permissions in `~/.arbiter` directory

---

## 📊 System Requirements

### Minimum (Claude Code / Cursor)
- **CPU**: 2 cores
- **RAM**: 4GB
- **Disk**: 10GB free space
- **Network**: Internet connection for GitHub and AI APIs

### Recommended (Local Models)
- **CPU**: 8+ cores
- **RAM**: 16GB+ (32GB for large models)
- **GPU**: NVIDIA GPU with 8GB+ VRAM (for Ollama/LM Studio)
- **Disk**: 50GB+ free space (model storage)

---

## 💰 Pricing

Arbiter is a commercial product with flexible subscription options:

- **Pay what you think is fair, starting at $3/mo or $32** (save 15% on annual plans)
- Unlimited repositories and issues
- 7-day free trial
- Student discount: 50% off

**No auto-renewal** - You decide if it's worth continuing each billing period.

[Get Started with Free Trial](https://git-arbiter.com/#pricing)

---

## 📜 License

Arbiter is commercial software. See [Terms of Service](https://git-arbiter.com/terms.html) for details.

---

## 🙏 Credits

Built with:
- [Claude AI](https://claude.ai) - Anthropic's AI assistant
- [Cursor](https://cursor.sh) - AI-powered code editor
- [Ollama](https://ollama.com) - Local LLM runtime
- [Docker](https://docker.com) - Containerization

---

<div align="center">

  **Ready to automate your GitHub issues?**

  [Get Started](https://git-arbiter.com) • [Documentation](https://git-arbiter.com/docs.html) • [Join Discord](https://discord.gg/Jt78QpnV)

  Made with ❤️ by [Mason](https://git-arbiter.com/about.html)

</div>
