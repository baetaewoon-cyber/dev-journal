# Dev Journal — Claude Code Instructions

This repo is an auto-maintained development journal and portfolio. Follow these instructions when the user asks to log progress or summarize their day.

## Commands

### "log my progress" / "log session" / "log today's work"

Generate a session entry:

1. **Scan recent git activity** across all repos the user works on. Check these locations:
   - `~/dev-journal` (this repo)
   - `~/Claude Code Project` (trading-system / multi-agent-system)
   - `~/.claude` (claude-agents)
   - Any other git repos in `~/` (check with `find ~ -maxdepth 2 -name ".git" -type d`)

   For each repo, run:
   ```bash
   git -C <repo-path> log --oneline --since="8 hours ago" --author="Taewoon"
   git -C <repo-path> diff --stat HEAD~5..HEAD 2>/dev/null
   ```

2. **Determine session number** for today:
   ```bash
   ls sessions/$(date +%Y-%m-%d)/ 2>/dev/null | wc -l
   ```
   Next session = count + 1

3. **Create session entry** at `sessions/YYYY-MM-DD/session-{N}.md` using this format:

   ```markdown
   # Session - {Month Day, Year} (#{N})

   **Time context:** {morning/afternoon/evening} session
   **Projects touched:** {list of repos with changes}

   ## What I Did
   - {Concrete bullet points of work completed}
   - {Include specific files, features, or systems touched}

   ## Key Decisions & Learnings
   - {Why you chose approach X over Y}
   - {What you learned that was non-obvious}
   - {Problems encountered and how they were solved}

   ## Tools & Concepts
   `tag1` `tag2` `tag3`
   ```

4. **Commit and push**:
   ```bash
   cd ~/dev-journal
   git add sessions/
   git commit -m "session: YYYY-MM-DD #N - {brief description}"
   git push origin master
   ```

### "summarize today" / "daily summary"

Generate a daily summary:

1. **Read all session entries** for today from `sessions/YYYY-MM-DD/`

2. **Create daily summary** at `daily/YYYY-MM-DD.md` using this format:

   ```markdown
   # {Month Day, Year}

   ## Highlight
   {One sentence — the single most important thing today.}

   ## Summary
   {2-3 paragraphs narrative. What moved forward, key decisions made,
   problems solved, what's next. Write this as if explaining to a
   future hiring manager what you accomplished and why it matters.}

   ## Projects
   - **{project-name}**: {What changed and why — 1-2 sentences}

   ## Tags
   `tag1` `tag2` `tag3`
   ```

3. **Update README.md**:
   - Increment "Days Active" stat
   - Update "Total Sessions" with cumulative count
   - Add today's summary link under "Recent Daily Summaries" (keep last 10, newest first):
     ```markdown
     - [{Month Day, Year}](daily/YYYY-MM-DD.md) — {highlight one-liner}
     ```

4. **Commit and push**:
   ```bash
   cd ~/dev-journal
   git add daily/ README.md
   git commit -m "daily: YYYY-MM-DD - {highlight one-liner}"
   git push origin master
   ```

## Writing Style

- **Session entries**: Structured, concise, technical. Bullet points. Include specifics (file names, function names, tool names).
- **Daily summaries**: Narrative, reflective, portfolio-quality. Write as if a technical hiring manager at a finance or AI company will read this. Show thinking process, not just output.
- **Tags**: Use lowercase, hyphenated terms. Mix technical (`claude-code-hooks`, `alpaca-api`) with conceptual (`multi-agent-orchestration`, `risk-management`).

## Important

- Never fabricate or embellish work. Only log what actually happened based on git history.
- If no meaningful work was done, say so briefly rather than padding the entry.
- Keep session entries factual. Save reflection and narrative for daily summaries.
- This repo works on ANY machine with Claude Code — all instructions are self-contained here.
