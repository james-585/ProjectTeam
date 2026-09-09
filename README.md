# PRINCE2 Agile Delivery Team — Claude Code Subagent Package

## What this is
Your Delivery Director + four specialists, rebuilt as genuinely independent
Claude Code subagents instead of one shared context. The key engineering
change from the original design: each specialist now runs in its own
context window, and the Independent Assurance Reviewer has no Write/Edit
tool at all — it structurally cannot alter what it's reviewing.

## Install
1. Copy the four files in `agents/` into your project's `.claude/agents/`
   directory (create it if it doesn't exist).
2. Put `CLAUDE.md` at your project root — this is what makes the main
   session act as Delivery Director and dispatch via the Task tool.
3. Keep `reference/` and `governance/` at project root too. Subagents read
   `reference/` on demand for depth beyond their condensed prompts;
   `governance/` holds the append-only decision and findings logs.

## How invocation actually works
Claude Code reads each subagent's `description` field to decide when to
dispatch automatically, or you can invoke one explicitly ("use the
independent-assurance-reviewer subagent on drafts/business-case-v2.md").
Each subagent call is a fresh context — it only sees what's in its prompt
file plus whatever you (as Director) pass it in the Task call.

**This is what makes independence real rather than aspirational**: the
Assurance Reviewer subagent has never seen the Producer's drafting
reasoning unless you explicitly paste it in. Don't. Pass file paths and a
review question — nothing else.

## Two things worth doing before you rely on this for real governance work
1. **Test the routing.** Run 5-10 realistic requests through it and check
   the Director dispatches to the right specialist, in the right sequence,
   without over- or under-invoking. Your skill-creator skill can help
   structure this as a proper eval rather than ad hoc spot-checks.
2. **Test the independence boundary deliberately.** Give the Assurance
   Reviewer a document you know has a planted flaw, and confirm it catches
   it without you having hinted at it in the invocation. If it doesn't
   catch planted flaws, the review isn't earning its keep — that's worth
   knowing before this touches an actual TRAILS governance submission.

## What's condensed vs what's preserved
Each subagent prompt keeps identity, invocation triggers, core operating
discipline, response format and prohibited behaviours. The exhaustive
checklists (every non-functional requirement category, every stakeholder
type, etc.) live in `reference/` — the subagent reads the relevant original
file via its Read tool when a task actually needs that depth, rather than
carrying it in every context window by default. This mirrors the
proportionality principle the instructions already argue for, just applied
to token budget instead of documentation.
