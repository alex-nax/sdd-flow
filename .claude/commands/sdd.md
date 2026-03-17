# Spec-Driven Development Flow

You are entering SDD (Spec-Driven Development) mode. Read `flows/sdd.md` for the complete flow reference.

## Command: $ARGUMENTS

Parse the arguments to determine the action:

### `start [name]` - Start new SDD flow
1. Create directory `flows/sdd-[name]/`
2. Copy templates from `flows/.templates/` 
3. Create `_status.md` with phase = REQUIREMENTS
4. Begin requirements elicitation with user

### `resume [name]` - Resume existing flow
1. Read `flows/sdd-[name]/_status.md` to determine current phase
2. Read all existing artifacts in the spec dir
3. Report current state to user
4. Continue from where left off

### `fork [existing] [new]` - Fork for context recovery
Use fork when the current flow is exhausted (context rot, wrong approaches, dead ends).

**Do NOT blindly copy all flow docs.** Instead:

1. **Analyze the exhausted flow** `flows/sdd-[existing]/`:
   - Read all artifacts (requirements, specs, plan, implementation log)
   - Identify what worked well (keep these insights)
   - Identify what went wrong (failed approaches, wrong assumptions, dead ends)
   - Note any valuable learnings or discoveries

2. **Create summary document** in new flow `flows/sdd-[new]/`:
   - Create `00-fork-context.md` with:
     ```markdown
     # Fork Context

     ## Origin
     Forked from: `sdd-[existing]`
     Reason for fork: [user explains or inferred from analysis]

     ## What Worked
     - [List successful decisions, valid requirements, good discoveries]

     ## What Failed
     - [List failed approaches with brief explanation of why]
     - [Wrong assumptions that led us astray]
     - [Dead ends encountered]

     ## Key Learnings
     - [Insights to carry forward]

     ## Recommendations for New Approach
     - [What to do differently this time]
     ```

3. **Start fresh in REQUIREMENTS phase**:
   - Create `_status.md` with phase = REQUIREMENTS
   - Reference the fork context in requirements gathering
   - Do NOT copy old requirements/specs/plan verbatim
   - Use learnings to ask better questions and avoid known pitfalls

4. **Begin requirements elicitation**:
   - Ask user: "What should we keep from the original requirements?"
   - Ask user: "What needs to change given what we learned?"
   - Build new requirements informed by past experience

### `status` - Show all active SDD flows
1. List all `flows/sdd-*/` directories
2. Read each `_status.md` and summarize phase + blockers

### No arguments or `help`
1. Show available commands and current active flows

---

## Phase Behaviors

### REQUIREMENTS Phase
- Elicit what user wants to build and why
- Ask clarifying questions
- Document user stories with acceptance criteria
- Identify constraints and non-goals
- Update `01-requirements.md` iteratively
- Wait for explicit "requirements approved" before advancing

### SPECIFICATIONS Phase  
- Analyze codebase for affected systems
- Design interfaces and data models
- Document edge cases
- Update `02-specifications.md` iteratively
- Wait for explicit "specs approved" before advancing

### PLAN Phase
- Break specs into atomic tasks
- Identify file changes and dependencies
- Estimate complexity
- Update `03-plan.md` iteratively
- Wait for explicit "plan approved" before advancing

### IMPLEMENTATION Phase
- Execute plan task by task
- Follow CLAUDE.md testing protocol (one test at a time)
- Log progress in `04-implementation-log.md`
- Document any deviations from plan

---

## Always

- Update `_status.md` after every significant change
- Never skip phases or assume approval
- When uncertain, ask rather than assume
- Before ending session, ensure handoff notes are complete
