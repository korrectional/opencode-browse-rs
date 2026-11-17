# Project Completion Roadmap: opencode-browse-rs

## Objective
Rewrite OpenCode Browse (https://github.com/anomalyco/browse) completely in idiomatic, production-grade Rust.

Architecture & Core Deliverables:
1. Cargo Workspace / Crate (`opencode-browse-rs`, binary `browse`):
   - Async runtime using `tokio`, JSON-RPC 2.0 / MCP protocol with `serde` and `serde_json`, CLI with `clap`.
2. MCP Server (`browse serve`):
   - Implements Model Context Protocol over stdio (JSON-RPC 2.0 `initialize`, `tools/list`, `tools/call`).
   - Exposes full browser control tool suite: `navigate`, `snapshot` (compact DOM accessibility/element outline with ref IDs like `e12`), `click`, `fill`, `type`, `press`, `select`, `hover`, `drag`, `scroll`, `wait_for`, `dialog`, `upload`, `eval`, `screenshot`, `screenshot_diff`, `network`, `console`, and `recording`.
3. Chromium Native Messaging Host & IPC Bridge (`browse native-host`):
   - Implements Chrome/Chromium Native Messaging protocol (4-byte native-endian uint32 length prefix + UTF-8 JSON payload over stdin/stdout).
   - Local Unix domain socket / named pipe IPC server bridging multiple MCP client sessions to the active Chromium extension native host process, with request/response correlation IDs, timeouts, and automatic reconnection.
4. Accompanying Manifest V3 Chromium Extension (`extension/`):
   - Manifest V3 extension (`manifest.json`, background service worker, content script / Chrome Debugger API integration) connecting via `chrome.runtime.connectNative` to execute DOM snapshots, element ref resolution, input dispatch, network/console capture, and tab screenshots.
5. CLI Installer (`browse install`):
   - Registers the Chrome/Chromium NativeMessagingHosts JSON manifest across Linux/macOS/Windows paths.
   - Injects the `browse` MCP server entry into agent configuration files (OpenCode, Claude Code, Claude Desktop, Cursor, Codex TOML, VS Code, Windsurf, Gemini CLI, Zed, etc.) while preserving existing comments and formatting.
6. Testing & CI:
   - Comprehensive unit and integration tests covering MCP protocol framing, Native Messaging length-prefixed framing, IPC multiplex routing, DOM snapshot ref parsing, and config file injection (`cargo test` passing 100%).
   - Clean `README.md`, `TODO.md` roadmap, and `.github/workflows/ci.yml` running `cargo fmt --check`, `cargo clippy`, and `cargo test`.

## 1. Core Architecture & Components
- [ ] Initialize project structure, configuration, and dependencies
- [ ] Implement core domain models, routes, or business logic
- [ ] Implement primary user/client interfaces (CLI, API, or Web)

## 2. Automated Test Suite
- [ ] Comprehensive unit tests for core modules
- [ ] Integration tests covering full request/data lifecycles
- [ ] Edge cases, boundary values, and error-handling tests
- [ ] Verify test suite passes with 100% success rate (zero errors)

## 3. GitHub Actions CI/CD Workflows
- [x] Create `.github/workflows/ci.yml` workflow file
- [x] Configure automated test run, linting, and build steps
- [x] Verify workflow YAML syntax and step validity

## 4. Edge Cases, Bug Audits & Polish
- [ ] Audit for runtime errors, unhandled exceptions, and edge cases
- [ ] Security hardening, input sanitization, and graceful degradation
- [ ] Comprehensive README.md with architecture, setup, and usage examples
- [ ] Clean git commit history and push to remote repository
