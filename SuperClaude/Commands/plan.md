---
allowed-tools: [Read, Glob, Grep, TodoWrite, Task, Bash, Edit, MultiEdit, Write, LS, WebFetch, WebSearch, NotebookRead, NotebookEdit]
description: "Convert user requirements into structured execution plans, optimized for session management and context limits"
wave-enabled: true
complexity-threshold: 0.4
performance-profile: balanced
personas: [analyzer, architect, qa]
mcp-servers: [sequential-thinking, context7, playwright, magic]
---

# /sc:plan - Session-Aware Planning Generator

## Purpose
Transform user requirements into structured execution plans with intelligent session management for 128k context limits.

## Language Protocol
- **User Interaction**: Chinese (用户用中文描述需求和交流)
- **Plan Output**: English (生成的计划文件用英文，便于代码实现)
- **Code & Commands**: English (所有代码、命令、文件名用英文)
- **Technical Terms**: English (API, database, function names等技术术语用英文)

This ensures fast comprehension for Chinese users while maintaining English code standards.

## Interactive Planning Process
**IMPORTANT**: Planning is a collaborative discussion, not a one-way generation.

### Decision Points Protocol
- **When encountering choices**: Stop and ask user for decisions
- **Architecture options**: Present alternatives with pros/cons
- **Technology selection**: Explain trade-offs, get user preference
- **Implementation approaches**: Clarify user priorities
- **Resource allocation**: Confirm timeline and complexity preferences

### Common Decision Points
- Database schema design choices
- Frontend framework/library selection
- API design patterns (REST/GraphQL/etc)
- Testing strategy depth
- Deployment approach
- Performance vs simplicity trade-offs
- Security level requirements

### Interaction Flow
```
1. Analyze requirements
2. Identify decision points
3. Ask clarifying questions (中文)
4. Wait for user responses
5. Continue analysis
6. Repeat until all decisions clear
7. Generate final plan (English)
```

**Never assume user preferences - always ask when unclear!**

User requirements:
======
$ARGUMENTS
======

## EXECUTION PROTOCOL

### Step 1: Analysis & Planning
1. Analyze user requirements (中文交流)
2. Ask clarifying questions if needed
3. Determine complexity level and session strategy

### Step 2: MANDATORY File Generation
**CRITICAL**: Always create a markdown file using Write tool

```python
# File naming logic
import datetime
task_slug = requirements.lower().replace(' ', '-')[:30]
date_str = datetime.now().strftime('%Y-%m-%d')
filename = f"plan-{date_str}-{task_slug}.md"
```

### Step 3: File Content Population
Use the structured template from "File Content Structure" section

### Step 4: Completion Response
Provide Chinese summary with file reference

## AUTO-UPDATE IMPLEMENTATION

### Integration with TodoWrite
When using TodoWrite during execution, automatically update the plan file:

```markdown
# After TodoWrite operations
1. Check if any plan file exists in current directory (plan-*.md)
2. If found, update the matching todo items
3. Update progress percentage
4. Update "Next Action" field
5. Add timestamp to show last update
```

### Smart Update Logic
- **Todo Status Sync**: TodoWrite completion → Plan file checkbox update
- **Progress Calculation**: Auto-calculate completion percentage
- **Context Enrichment**: Add new files/commands discovered during execution
- **Error Tracking**: Document blockers and their resolutions

### Implementation Pattern
```python
# Pseudo-code for auto-update
def auto_update_plan():
    plan_files = glob("plan-*.md")
    if plan_files:
        for file in plan_files:
            update_todo_status(file)
            update_progress_tracking(file)
            update_context_references(file)
            add_timestamp(file)
```

## Execution Modes

### Auto-Detection Rules
- **Simple** (≤5 todos): Direct execution in current session
- **Moderate** (6-15 todos): Plan + single exec session
- **Complex** (16-30 todos): Plan + multi-exec sessions (exec1, exec2...)
- **Enterprise** (30+ todos): Plan + staged execution with handoffs

### Session Strategy
```
Simple:    [Current Session] → Complete
Moderate:  [Plan Session] → [Exec Session]
Complex:   [Plan Session] → [Exec1] → [Exec2] → [ExecN]
Enterprise: [Plan Session] → [Stage1: Discovery] → [Stage2: Implementation] → [Stage3: Testing]
```

## Core Workflow

### 1. Requirements Analysis
- **Domain**: Frontend, Backend, Infrastructure, Security, DevOps
- **Complexity**: Count estimated todos, assess dependencies
- **Context Budget**: Estimate token usage for each stage
- **Session Plan**: Determine optimal session breakdown

### 2. Session-Optimized Plan Generation

#### MANDATORY: File Generation First
```
1. Generate detailed markdown plan file
2. Use Write tool to create the file
3. Confirm file creation success
4. Provide chat summary with file reference
```

#### Session Boundaries
- **Plan Session**: Analysis, architecture, detailed breakdown → **MUST generate .md file**
- **Exec Sessions**: Implementation, testing, deployment → **Reference .md file**
- **Context Handoffs**: Clear state transfer between sessions → **Update .md file**

#### Todo Structure
```markdown
## Session 1: Plan (Current)
- [ ] Analyze requirements and scope
- [ ] Design architecture/approach
- [ ] Create detailed implementation plan
- [ ] Generate exec session instructions

## Session 2: Exec1 (Next)
- [ ] Implementation todos (specific actions)
- [ ] Testing validation points
- [ ] Success criteria checkpoints
- [ ] **Update plan document**: Mark Session 2 tasks complete, update progress

## Session N: ExecN (Final)
- [ ] Final integration
- [ ] E2E validation
- [ ] Documentation updates
- [ ] **Update plan document**: Mark project complete, record final outcomes
```

### 3. Context Management

#### Handoff Documentation
Each session ends with:
- **State Summary**: What was completed
- **Next Session Brief**: Exact starting point
- **Context Transfer**: Essential information only
- **Success Criteria**: Clear validation points

#### Session Continuity
- **File References**: Specific file:line locations
- **Command History**: Exact commands to reproduce state
- **Test URLs**: Direct validation endpoints
- **Error Patterns**: Known issues and solutions

## Testing Integration

### Testing by Complexity
- **Simple**: Basic validation (curl, manual check)
- **Moderate**: Playwright automation + manual verification
- **Complex**: Full E2E suite with staged validation
- **Enterprise**: Comprehensive testing pipeline

### Staged E2E Testing (Critical for Complex/Enterprise Tasks)
**Philosophy**: Test early, test often, correct immediately

- **After Core Logic Changes**: Verify new algorithms work in isolation
- **After Infrastructure Changes**: Test deployment and migration tools
- **After Each Major Component**: Validate integration points
- **Before Final Deployment**: Comprehensive end-to-end validation

**Benefits**:
- 🔧 **Immediate Correction**: Fix issues when scope is small
- 🎯 **Precise Debugging**: Limited change set makes problems easier to isolate
- 💰 **Cost Efficiency**: Early fixes prevent expensive rollbacks
- 🛡️ **Risk Mitigation**: Avoid "big bang" deployment failures

**Implementation**:
- Add E2E test tasks after each major implementation phase
- Use real test environments for validation
- Test both positive and negative scenarios
- Verify existing functionality remains intact

### Quality Gates
- **Pre-Implementation**: Requirements validated
- **Mid-Implementation**: Core functionality working ← Add E2E test here
- **Post-Component**: Each major component verified ← Add E2E test here
- **Pre-Deployment**: All tests passing ← Add E2E test here
- **Post-Deployment**: Production validation

## Advanced Features

### Session Memory
- **Project Context**: Leverage CLAUDE.md patterns
- **Previous Sessions**: Reference successful patterns
- **Error Learning**: Avoid known pitfalls
- **Tool Preferences**: Use established workflows

### Learning and Adaptation (Post-Completion)
- **Todo Pattern Updates**: After successful completion, update templates with proven approaches
- **Complexity Calibration**: Adjust estimation models based on actual vs predicted complexity
- **Session Optimization**: Refine session boundaries based on context usage patterns
- **Error Pattern Recognition**: Document common failure modes and prevention strategies
- **Tool Chain Refinement**: Update preferred tool sequences based on successful workflows

### Parallel Execution
- **Independent Tracks**: Frontend + Backend simultaneously
- **Dependency Chains**: Sequential when required
- **Resource Optimization**: Minimize context overlap

### Failure Recovery
- **Checkpoint System**: Save progress at validation points
- **Rollback Plan**: Quick revert on failures
- **Alternative Paths**: Backup approaches for blocked tasks

## Example Session Plans

### Simple Task (Direct Execution)
```
Current Session: Fix bug, test, deploy (≤5 todos)
```

### Moderate Task (Plan + Exec)
```
Session 1: Analyze + Plan (current)
Session 2: Implement + Test + Deploy
```

### Complex Task (Multi-Exec)
```
Session 1: Analysis + Architecture Plan
Session 2: Core Implementation (Exec1)
Session 3: Integration + Testing (Exec2)
Session 4: Deployment + Validation (Exec3)
```

### Enterprise Task (Staged)
```
Session 1: Discovery + Requirements Analysis
Session 2: Architecture Design + Planning
Session 3: Infrastructure Setup (Stage1)
Session 4: Backend Implementation (Stage2)
Session 5: Frontend Implementation (Stage3)
Session 6: Integration Testing (Stage4)
Session 7: Deployment + Production Validation (Stage5)
```

## Output Format

### File Generation Strategy
**CRITICAL**: Always generate a markdown file to persist the plan

#### File Naming Convention
```
plan-{date}-{task-slug}.md
# Example: plan-2024-01-15-user-auth-system.md
```

#### File Location
- **Current Directory**: Save in current working directory
- **Backup Location**: Also mention path for reference

#### File Content Structure
```markdown
# {Task Name} - Implementation Plan
*Generated: {ISO datetime}*
*Complexity: {Level} | Sessions: {N} | Duration: {estimated}*

## 📋 Executive Summary
- **Goal**: {One sentence description}
- **Scope**: {Key deliverables}
- **Success Criteria**: {How we know it's done}

## 🔄 Session Breakdown

### Session 1: Planning (Current) ✅
- [x] Requirements analysis
- [x] Architecture design
- [x] Detailed task breakdown
- [x] **Plan file generated**: `{filename}`

### Session 2: Implementation (Next)
**Context Handoff**: {Specific files, commands, state}
**Starting Point**: {Exact next action}
**Success Criteria**: {Clear validation points}

- [ ] {Implementation tasks}
- [ ] {Testing and validation}
- [ ] **Update this plan document**: Update progress, mark completed tasks, record session outcomes

### Session N: Final (If needed)
**Context Handoff**: {Previous session results}
**Starting Point**: {Final implementation steps}
**Success Criteria**: {Project completion validation}

- [ ] {Final tasks}
- [ ] {Deployment/delivery}
- [ ] **Update this plan document**: Mark project complete, document final state and lessons learned

## 📝 Implementation Todos

### Phase 1: Core Implementation
- [ ] {Specific actionable task with file:line references}
- [ ] {Next task with expected outcomes}
- [ ] {Include validation steps}
- [ ] **Update this plan document**: Mark completed tasks, update progress percentage, record any new findings or blockers

### Phase 2: Testing & Validation
- [ ] {Specific test commands}
- [ ] {Validation URLs and endpoints}
- [ ] {Success indicators and metrics}
- [ ] **Update this plan document**: Mark completed tasks, update progress percentage, record test results and any issues found

### Phase 3: Deployment & Cleanup
- [ ] {Deployment steps}
- [ ] {Documentation updates}
- [ ] {Cleanup and optimization}
- [ ] **Update this plan document**: Mark all tasks complete, set final status, document any lessons learned

## 🧪 Quality Assurance

### Pre-Implementation Checklist
- [ ] Requirements fully understood
- [ ] Architecture validated
- [ ] Dependencies identified
- [ ] Risk assessment complete

### Mid-Implementation Checkpoints
- [ ] Core functionality working
- [ ] Integration points tested
- [ ] Error handling implemented
- [ ] Performance acceptable
- [ ] **Update this plan document**: Record checkpoint results, update progress status

### Post-Implementation Validation
- [ ] All todos completed
- [ ] Tests passing
- [ ] Documentation updated
- [ ] Deployment successful
- [ ] **Update this plan document**: Mark project complete, document final outcomes and recommendations

## 📊 Progress Tracking
*IMPORTANT: Update this section regularly as required by the tasks above*

**Current Status**: Planning Complete
**Next Action**: {Specific next step}
**Completion**: 0/X todos completed
**Last Updated**: {Timestamp}
**Session Notes**: {Key findings, blockers, or changes from original plan}

## 🔗 Context & References
- **Related Files**: {List key files}
- **Commands History**: {Important commands}
- **Test URLs**: {Validation endpoints}
- **Documentation**: {Relevant docs}

---
*Generated by SuperClaude /sc:plan | Auto-updates enabled*
```

### Auto-Update Mechanism
**ENHANCED**: Plan files include explicit update tasks in every phase

#### Update Strategy
1. **Embedded Update Tasks**: Every phase includes "Update this plan document" as explicit task
2. **Session-Level Updates**: Each session includes plan document update requirement
3. **Checkpoint Updates**: Quality assurance phases include documentation updates
4. **Progress Tracking**: Regular updates to progress section with timestamps and notes

#### Benefits
- **Session Independent**: Works regardless of TodoWrite integration status
- **Always Visible**: Update tasks are explicit and cannot be missed
- **Context Preservation**: Ensures all sessions update the same document
- **Audit Trail**: Creates clear record of progress and decisions

#### Implementation Logic
```python
# Plan document update logic
def should_update_plan():
    return (
        todo_status_changed() or
        major_milestone_reached() or
        session_ending() or
        user_requests_update()
    )

def update_plan_file(filename, updates):
    # Read current plan
    # Update progress section
    # Update todo checkboxes
    # Update context references
    # Save back to file
```

#### What Gets Updated
- **Todo Status**: `- [ ]` → `- [x]` for completed items
- **Progress Tracking**: Current completion percentage
- **Context References**: New files, commands, URLs
- **Next Action**: Updated based on current state
- **Timestamps**: Last updated time

### Chat Response Format
After generating the file, respond with:
```
✅ **计划已生成**: `{filename}`

📋 **总结**:
- 复杂度: {Level}
- 预计时长: {duration}
- 包含 {X} 个具体任务

📁 **下一步**: 参考 `{filename}` 文件开始执行
🔄 **文档更新**: 每个阶段都包含明确的"更新计划文档"任务，确保跨session准确性

💡 **使用提示**: 
- 每完成一个阶段，务必执行其中的"Update this plan document"任务
- 更新进度跟踪部分的状态、完成度和时间戳
- 记录任何发现的问题、解决方案或计划变更
```

### Post-Completion Learning Output
```markdown
# {Task Name} - Completion Summary

## Actual vs Predicted
- **Estimated Complexity**: Moderate (12 todos)
- **Actual Complexity**: Simple (8 todos completed)
- **Session Usage**: Planned 2 sessions, used 1
- **Time Spent**: 2 hours vs estimated 4-6 hours

## Pattern Updates
- **Successful Approach**: Direct API testing instead of port-forwarding
- **Tool Chain**: `kubectl exec → curl → playwright` worked efficiently
- **Error Resolution**: Frontend data parsing issue (result.data vs result)

## Template Improvements
- For similar frontend display bugs: Check data structure first
- For K8s environments: Use existing test URLs before port-forwarding
- Add to JavaScript debugging checklist: Validate API response format

## Next Session Recommendations
- Update complexity detection for frontend data issues
- Add CLAUDE.md reminder about test environment URLs
- Include API response validation in standard debugging flow
```

## Key Improvements
1. **Context-Aware**: Designed for 128k limits
2. **Session-Optimized**: Clean handoffs between executions
3. **Complexity-Adaptive**: Auto-scales approach to task size
4. **Recovery-Friendly**: Clear checkpoints and rollback plans
5. **Practical Focus**: Removes theoretical complexity, adds operational value
