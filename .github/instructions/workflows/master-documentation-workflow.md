# Master Documentation & Architecture Workflow with Validation

You are executing a comprehensive documentation workflow that generates, validates, and improves project documentation through automated accuracy verification, then creates architecture diagrams.

## Workflow Overview

This workflow creates a **self-improving documentation system** that:

1. Applies agent rules and context
2. Generates comprehensive documentation from source code using **generic prompt**
3. Validates documentation accuracy against the actual codebase AND applies corrections using **composite context**
4. Creates architecture diagrams based on validated documentation
5. Creates a feedback loop for continuous improvement **without polluting generic prompts**
6. **Tracks execution history** with automatic run logging for audit and improvement

## Workflow Run History Tracking

Every workflow execution automatically creates/updates `docs/.temp/workflow-run-history.md` with:

- **Timestamp**: When the workflow was executed
- **Command**: Which execution mode was used (default, --quick, --doc-only, etc.)
- **Files Generated**: List of all files created or modified during execution
- **Execution Duration**: Time taken for the workflow
- **Success Status**: Whether the workflow completed successfully

**History File Format**:
```markdown
# Workflow Execution History

## Run #[N] - [YYYY-MM-DD HH:MM:SS]
- **Command**: Execute documentation workflow [flags]
- **Mode**: [Full/Quick/Doc-Only/Diagram-Only/Validation-Only]
- **Duration**: [X] minutes
- **Status**: [SUCCESS/FAILED/PARTIAL]
- **Files Generated**:
  - docs/PROJECT_DOCUMENTATION.md (created/updated)
  - docs/architecture_diagram.html (created/updated)
  - docs/.temp/workflow-learnings.md (created/updated)
- **Notes**: [Any relevant execution notes]
```

This provides automatic audit trail and helps identify patterns in workflow usage and success rates.

## Execution Sequence

### Phase 1: Agent Context Setup

**Execute**: Read and apply agent rules from `.github/instructions/rules/rules.md`

**Goal**: Define your role as a principal software engineer and set the boundaries for your analysis.

**Output**: Agent context and role established

### Phase 2: Documentation Generation

**Execute**: `.github/instructions/prompts/documentation-generation-prompt.md`

**Goal**: Generate comprehensive project documentation using the **generic, technology-agnostic prompt** with universal validation requirements.

**Important**: The documentation-generation-prompt.md must remain **generic and reusable** across all project types (frontend, backend, different tech stacks).

**Output**: `docs/PROJECT_DOCUMENTATION.md`

### Phase 3: Documentation Validation & Correction (Enhanced)

**Execute**: `.github/instructions/prompts/documentation-validation-prompt.md`

**Goal**: Systematically validate the generated documentation against the source code to identify inaccuracies AND apply corrections using composite context.

**Inputs**:

- Generated documentation from Phase 2
- Access to complete source codebase
- The **generic** documentation generation prompt

**Actions**:

1. **Validation**: Systematically verify every claim in the documentation against the actual source code
2. **Learning Creation**: Create `docs/.temp/workflow-learnings.md` with **project-specific validation insights, patterns, and technology-specific validation rules**
3. **Composite Context Application**: Update `docs/PROJECT_DOCUMENTATION.md` using BOTH:
   - The generic `.github/instructions/prompts/documentation-generation-prompt.md`
   - The project-specific `docs/.temp/workflow-learnings.md`
4. **Learning Enhancement**: Update `docs/.temp/workflow-learnings.md` with additional execution metrics, patterns, and insights
5. **Generic Prompt Preservation**: Keep `.github/instructions/prompts/documentation-generation-prompt.md` unchanged and reusable

**Outputs**:

- Display validation findings in chat window
- Corrected and validated `docs/PROJECT_DOCUMENTATION.md`
- `docs/.temp/workflow-learnings.md` with project-specific validation insights and technology-specific rules
- `docs/.temp/VALIDATION_REPORT.md` with detailed accuracy metrics and validation findings
- **DO NOT** update the generic documentation-generation-prompt.md

### Phase 4: Architecture Diagram Generation

**Execute**: `.github/instructions/prompts/diagram-generation-prompt.md`

**Goal**: Create architecture diagram using the validated documentation as additional context.

**Inputs**:

- Validated and corrected documentation from Phase 3
- Codebase analysis
- Component mapping information

**Output**: `docs/architecture_diagram.html`

## Execution Instructions

### Universal Step 0: Initialize Workflow Run History
```
Create or append to docs/.temp/workflow-run-history.md with:
- Current timestamp
- Command being executed (with flags)
- Execution mode
- Start time for duration tracking
```

### Universal Step 1: Apply Agent Rules
```
Read .github/instructions/rules/rules.md and acknowledge the agent role as a principal software engineer with expertise in frontend web applications and backend REST services.
```

### Flag-Based Execution Steps

#### Default (No Flags): Full Workflow
```
Step 0: Initialize Workflow Run History
Step 1: Apply Agent Rules
Step 2: Generate Initial Documentation (using generic prompt)
Step 3: Validate Documentation Accuracy and Apply Corrections
Step 4: Generate Architecture Diagram
Step 5: Complete Workflow Run History (log completion, duration, files generated)
```

#### --quick: Quick Mode
```
Step 0: Initialize Workflow Run History
Step 1: Apply Agent Rules
Step 2: Generate Initial Documentation (using generic prompt)
Step 3: Generate Architecture Diagram (skip validation)
Step 4: Complete Workflow Run History (log completion, duration, files generated)
```

#### --doc-only: Documentation Only
```
Step 0: Initialize Workflow Run History
Step 1: Apply Agent Rules
Step 2: Generate Initial Documentation (using generic prompt)
Step 3: Complete Workflow Run History (log completion, duration, files generated)
```

#### --diagram-only: Diagram Only
```
Step 0: Initialize Workflow Run History
Step 1: Apply Agent Rules
Step 2: Generate Architecture Diagram
   - Requires existing docs/PROJECT_DOCUMENTATION.md
   - Uses existing documentation as primary source
Step 3: Complete Workflow Run History (log completion, duration, files generated)
```

#### --validation-only: Validation Only
```
Step 0: Initialize Workflow Run History
Step 1: Apply Agent Rules
Step 2: Validate Documentation Accuracy and Apply Corrections
   - Requires existing docs/PROJECT_DOCUMENTATION.md
   - Creates learning file and applies composite context corrections
Step 3: Complete Workflow Run History (log completion, duration, files generated)
```

## Success Criteria

### Universal Success (All Executions)
- [ ] Workflow run history initialized and tracked
- [ ] Agent rules applied and acknowledged
- [ ] Role and expertise boundaries established
- [ ] Flag-specific phases completed successfully
- [ ] All outputs generated according to flag requirements
- [ ] Workflow run history completed with execution details

### Flag-Specific Success Criteria

#### Default (No Flags): Full Workflow Success
- [ ] Comprehensive documentation generated using generic prompt
- [ ] Every factual claim verified against source code
- [ ] All inaccuracies corrected using composite context
- [ ] Architecture diagram created with clickable components
- [ ] Project-specific learning file with technology insights
- [ ] Complete documentation system ready for team use

#### --quick: Quick Mode Success
- [ ] Documentation generated using generic prompt
- [ ] Architecture diagram created with GitHub navigation
- [ ] Total execution time under 35 minutes
- [ ] All major project aspects covered (unvalidated)

#### --doc-only: Documentation Only Success
- [ ] Comprehensive documentation generated using generic prompt
- [ ] All major project aspects covered
- [ ] Documentation follows specified structure
- [ ] Draft ready for review or further validation

#### --diagram-only: Diagram Only Success
- [ ] Architecture diagram created from existing documentation
- [ ] Diagram accurately represents system architecture
- [ ] Clickable components link to actual source files
- [ ] Visual representation matches documented structure

#### --validation-only: Validation Only Success
- [ ] Every factual claim verified against source code
- [ ] All inaccuracies corrected using composite context
- [ ] Project-specific learning file created with technology insights
- [ ] Documentation accuracy improved to 95%+ from baseline

## Final Deliverables by Flag

### Default (No Flags): Full Workflow
- ✅ `docs/PROJECT_DOCUMENTATION.md` (validated and corrected)
- ✅ `docs/architecture_diagram.html` (interactive with GitHub links)
- ✅ `docs/.temp/workflow-learnings.md` (project-specific insights)
- ✅ `docs/.temp/VALIDATION_REPORT.md` (accuracy metrics and validation findings)
- ✅ `docs/.temp/workflow-run-history.md` (execution audit trail)
- ✅ Validation report with accuracy metrics
- ⏱️ **Time Investment**: 40-58 minutes

### --quick: Quick Mode
- ✅ `docs/PROJECT_DOCUMENTATION.md` (unvalidated, comprehensive)
- ✅ `docs/architecture_diagram.html` (interactive with GitHub links)
- ✅ `docs/.temp/workflow-run-history.md` (execution audit trail)
- ⏱️ **Time Investment**: 22-33 minutes

### --doc-only: Documentation Only
- ✅ `docs/PROJECT_DOCUMENTATION.md` (comprehensive draft)
- ✅ `docs/.temp/workflow-run-history.md` (execution audit trail)
- ⏱️ **Time Investment**: 12-18 minutes

### --diagram-only: Diagram Only
- ✅ `docs/architecture_diagram.html` (interactive with GitHub links)
- ✅ `docs/.temp/workflow-run-history.md` (execution audit trail)
- ⏱️ **Time Investment**: 12-18 minutes

### --validation-only: Validation Only
- ✅ `docs/PROJECT_DOCUMENTATION.md` (validated and corrected)
- ✅ `docs/.temp/workflow-learnings.md` (project-specific insights)
- ✅ `docs/.temp/VALIDATION_REPORT.md` (accuracy metrics and validation findings)
- ✅ `docs/.temp/workflow-run-history.md` (execution audit trail)
- ✅ Validation report with accuracy metrics
- ⏱️ **Time Investment**: 20-28 minutes

## Quality Assurance

### Universal Quality Requirements
- [ ] Generic prompt remains technology-agnostic and reusable
- [ ] Clean separation between generic and project-specific components
- [ ] All outputs follow established formatting standards
- [ ] Flag-specific requirements met within timeline

### Validation-Enabled Flags Quality (Default, --validation-only)
- [ ] Final documentation contains only verified, accurate information
- [ ] Consistent package manager usage throughout
- [ ] Only features that actually exist are documented
- [ ] All commands and setup instructions work correctly
- [ ] Documentation matches actual project structure and patterns

### Diagram-Enabled Flags Quality (Default, --quick, --diagram-only)
- [ ] Architecture diagram accurately represents system architecture
- [ ] Clickable components link to actual source files
- [ ] Visual representation matches documented/actual structure
- [ ] Interactive features (zoom, navigation) work correctly

## 🎊 Simple & Powerful

This streamlined documentation workflow provides **maximum flexibility with minimal complexity**:

### ✅ **One Base Command, Five Modes**
```bash
Execute documentation workflow                    # Full workflow (recommended)
Execute documentation workflow --quick           # Fast docs + diagram, skip validation  
Execute documentation workflow --doc-only        # Documentation draft only
Execute documentation workflow --diagram-only    # Architecture diagram only
Execute documentation workflow --validation-only # Accuracy check only
```

### 🚀 **Key Benefits**
- **📚 One Command to Learn**: `Execute documentation workflow` with optional flags
- **⚡ Time Flexibility**: Choose 12 minutes to 58 minutes based on needs
- **🎯 Perfect Defaults**: No flags = complete workflow with validation
- **🔧 Developer-Friendly**: Standard CLI flag syntax familiar to all developers
- **📊 Clear Outputs**: Always know exactly what you'll get

### 🎯 **Perfect For**
- **New Projects**: Use default (no flags) for complete documentation
- **Quick Prototypes**: Use `--quick` for fast results without validation
- **Documentation Drafts**: Use `--doc-only` for initial documentation
- **Existing Documentation**: Use `--validation-only` to check accuracy
- **Visual Architecture**: Use `--diagram-only` for diagrams

### 🏆 **Enterprise Ready**
- **100% Accuracy Standard**: Maintained for validation-enabled modes
- **Technology Agnostic**: Works with any programming language or framework
- **Self-Improving**: Builds project-specific learning without polluting generic prompts
- **Production Quality**: Generates professional documentation ready for team use

**Start with the default command and add flags as needed** - it's that simple! 🎉

## Continuous Improvement Architecture

This workflow creates a **clean learning system** where:

- **Generic Prompt**: Remains reusable across all project types (frontend React, backend Java, Python APIs, etc.)
- **Project Learnings**: Technology-specific validation rules and patterns are captured per project
- **Composite Context**: Final documentation updates use both generic principles and project-specific learnings
- **Scalability**: Each project type builds its own learning base without polluting generic prompts
- **Reusability**: The same generic prompt works for diverse technology stacks
