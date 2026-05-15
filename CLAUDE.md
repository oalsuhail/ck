# Claude Development Guide

This file contains project-specific instructions for Claude and other AI agents working on the ck codebase.
Whenever you actually use ck and it does something unexpected, jot it down in a file could UNEXPECTED.md - supply what you ran, what you expected to happen, what happened instead.

## Resources

- [superpowers](https://github.com/obra/superpowers.git) - Additional capabilities and tools for Claude agents


## Release Process

### Version Tagging Convention

**IMPORTANT**: Tags follow the format `X.Y.Z` (NO `v` prefix) to match current standard:

```bash
# Correct format (current standard since 0.3.8+)
git tag 0.4.1
git tag 0.3.9

# Old format (deprecated, do not use)
git tag v0.3.4
```

Always check existing tags first: `git tag --sort=-version:refname`

### Pre-Commit Quality Checks

**ALWAYS** run these commands in order before any commit:

1. **Linting**: `cargo clippy` - Fix all warnings
2. **Formatting**: `cargo fmt` - Format all code  
3. **Testing**: `cargo test` - Ensure all tests pass

### Version Bump Process

When bumping versions:

1. **Update workspace version**: `Cargo.toml` (workspace level)
2. **Update ALL crate versions**: Use find/replace across all `Cargo.toml` files
   ```bash
   find . -name "Cargo.toml" -exec sed -i '' 's/version = "OLD"/version = "NEW"/g' {} \;
   ```
3. **Update documentation versions**: Check `PRD.txt` and other docs
4. **Update CHANGELOG.md**: Add comprehensive release notes (see format below)

### CHANGELOG.md Format

Always update CHANGELOG.md with new releases. Follow this structure:

```markdown
## [X.Y.Z] - YYYY-MM-DD

### Added
- **Feature name**: Clear user-facing description
- **Technical capability**: What it enables

### Fixed  
- **Bug description**: What was broken and how it's fixed
- **Performance issue**: Specific improvements made

### Technical
- **Implementation details**: For maintainers and contributors
- **Dependencies**: New dependencies added
```

### Development Notes

- **Test coverage**: Maintain comprehensive test coverage (currently 65+ tests)
- **Cross-platform**: Ensure features work on Windows, macOS, and Linux
- **Performance**: Consider impact on indexing and search performance
- **User experience**: Maintain grep compatibility and intuitive CLI design

### Common Patterns in this Codebase

- **Error handling**: Use `anyhow::Result` consistently
- **Async/await**: Tokio runtime for async operations  
- **Parallel processing**: Rayon for CPU-intensive tasks
- **File I/O**: Memory-mapped files for large data access
- **Configuration**: Workspace-level dependency management

### Quality Standards

- All clippy warnings must be resolved
- Code must be formatted with `cargo fmt`
- All tests must pass
- New features require comprehensive test coverage
- Breaking changes require major version bump
- --help reflects any new features
- README incorporates any new user features (e.g. flags etc)

# gstack

Use the `/browse` skill from gstack for all web browsing. Never use `mcp__claude-in-chrome__*` tools.

## Available Skills

- `/office-hours` - Office hours workflow
- `/plan-ceo-review` - CEO plan review
- `/plan-eng-review` - Engineering plan review
- `/plan-design-review` - Design plan review
- `/design-consultation` - Design consultation
- `/design-shotgun` - Rapid design exploration
- `/design-html` - HTML design generation
- `/review` - Code review
- `/ship` - Ship a change
- `/land-and-deploy` - Land and deploy
- `/canary` - Canary deployment
- `/benchmark` - Benchmarking
- `/browse` - Web browsing (use this for all web browsing)
- `/connect-chrome` - Connect to Chrome
- `/qa` - QA testing
- `/qa-only` - QA only
- `/design-review` - Design review
- `/setup-browser-cookies` - Set up browser cookies
- `/setup-deploy` - Set up deployment
- `/setup-gbrain` - Set up gbrain
- `/retro` - Retrospective
- `/investigate` - Investigation workflow
- `/document-release` - Document a release
- `/document-generate` - Generate documentation
- `/codex` - Codex workflow
- `/cso` - CSO workflow
- `/autoplan` - Automatic planning
- `/plan-devex-review` - DevEx plan review
- `/devex-review` - DevEx review
- `/careful` - Careful mode
- `/freeze` - Freeze changes
- `/guard` - Guard mode
- `/unfreeze` - Unfreeze changes
- `/gstack-upgrade` - Upgrade gstack
- `/learn` - Learning workflow
## gstack (REQUIRED — global install)

**Before doing ANY work, verify gstack is installed:**

```bash
test -d ~/.claude/skills/gstack/bin && echo "GSTACK_OK" || echo "GSTACK_MISSING"
```

If GSTACK_MISSING: STOP. Do not proceed. Tell the user:

> gstack is required for all AI-assisted work in this repo.
> Install it:
> ```bash
> git clone --depth 1 https://github.com/garrytan/gstack.git ~/.claude/skills/gstack
> cd ~/.claude/skills/gstack && ./setup --team
> ```
> Then restart your AI coding tool.

Do not skip skills, ignore gstack errors, or work around missing gstack.

Using gstack skills: After install, skills like /qa, /ship, /review, /investigate,
and /browse are available. Use /browse for all web browsing.
Use ~/.claude/skills/gstack/... for gstack file paths (the global path).
