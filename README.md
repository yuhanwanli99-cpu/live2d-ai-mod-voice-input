# live2d-ai-mod-voice-input

> **Legacy（2026-11-19，0.7.0 收口）**：本 Mod 自 0.7.0 起**不再随发布包分发、不再挂回宿主**
> （0.7.0 打包清单 = external-input / local-tts / local-engine / a2a / cosyvoice3-tts / desk-pet）。
> 仓库保留历史与自建可能；维护级别 = legacy：只做安全与可编译修复，不承诺兼容演进。

语音输入 Mod（稳定 id voice-input；工厂 `FACTORY`（`VoiceInputFactory`））的实现 crate。本仓库是 2026-10-19 从主仓 `/home/skystar/Live2D-Ai` 拆出来的独立本地仓库：**仓库根 = crate 根**（`Cargo.toml` 在根），许可证 AGPL-3.0-only（见 `LICENSE`）。

依赖方向只有「Mod → 主仓」：`live2d-ai-mod-system` 走 path 指向同父目录的 `../Live2D-Ai/crates/...`；没有指向旧第二仓 `../Live2D-Ai-mods` 的 path，也没有 Mod→Mod 的 path 依赖。本仓是独立 workspace（`Cargo.toml` 末尾空的 `[workspace]`），可以 `cd` 进来单独跑门禁。

## 挂回主仓（维护者手动，**没有动态加载器**）

1. 主仓根 `Cargo.toml` 的 `[workspace.dependencies]` 加一行：`live2d-ai-mod-voice-input` = { path = "../live2d-ai-mod-voice-input" }；
2. `crates/live2d-ai-desktop/Cargo.toml` 加同名依赖（`live2d-ai-mod-voice-input` = { workspace = true }）；
3. 把 `FACTORY`（`VoiceInputFactory`） 加进 `AVAILABLE_MOD_FACTORIES`；
4. 宿主侧 `/api/v1/voice/transcript` 路由（主仓 `crates/live2d-ai-desktop/src/web_api/voice_routes.rs`）也要一并加回；ASR 本体在 sidecar，不在本 crate。

## 门禁

```bash
cargo test --all-targets
cargo test --doc
cargo fmt -- --check
cargo clippy --all-targets -- -D warnings
```

公开仓库：<https://github.com/yuhanwanli99-cpu/live2d-ai-mod-voice-input>（分支 `main`）。
