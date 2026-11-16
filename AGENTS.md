# Agent notes

Rules for AI coding agents working in this repository. The repository is public, so everything in a commit, pull request or comment is public too.

## Commits and pull requests

- **No session identifiers.** Never put a session ID or session link in a commit message, a pull request title or description, a comment or a file. This covers trailers such as `Claude-Session:` and any `claude.ai/code/session_...` URL, and the equivalent from any other tool.
- **No "generated with" footers.** Do not add lines such as "Generated with Claude Code", tool badges or tool links to commits, pull requests or comments.
- A plain `Co-Authored-By:` trailer naming the model is fine.

If your tooling adds these lines by default, remove them before committing or opening a pull request. Rewriting pushed history to remove them afterwards does not fully erase them from GitHub.
