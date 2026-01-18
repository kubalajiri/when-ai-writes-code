# Generate Component Documentation for Copilot

## Task

Create a `AGENTS.md` file in a component directory to document the component's purpose, behavior, and usage patterns when developers work on it.

## Context

- **Tech Stack**: React 18 + TypeScript, Less (BEM), Jest + React Testing Library
- **Focus**: High-level, stable information that doesn't change frequently

## Instructions

### Analyze the Component

Review the component directory to understand:

- What problem does it solve?
- How is it structured?
- What are its main features?
- How should developers use it?

### Create Documentation

Generate a `AGENTS.md` file in the component directory with these sections:

#### 1. Component Overview (2-4 sentences)

- **What**: What is this component and what does it do?
- **Why**: What problem does it solve? When should it be used?
- **Where**: Location in the project

Example:

```markdown
The `DataGrid` component provides a flexible, performant table for displaying and
interacting with large datasets. It supports sorting, filtering, pagination, and
custom cell rendering. Use it when you need to display tabular data with user
interactions. Located in `packages/components/src/components/DataGrid/`.
```

#### 2. Key Features (4-6 bullet points)

List main capabilities, not implementation details:

- Feature 1 - what it enables
- Feature 2 - what it enables
- Feature 3 - what it enables

Example:

```markdown
- **Sorting**: Click column headers to sort data ascending/descending
- **Filtering**: Built-in filter UI for text, number, and date columns
- **Virtual Scrolling**: Efficiently renders large datasets (10,000+ rows)
- **Custom Renderers**: Replace default cell rendering with custom components
- **Row Selection**: Single or multi-row selection with callbacks
- **Export**: Export visible data to CSV or Excel
```

#### 3. Component Behavior (3-5 key points)

Describe how the component behaves, not how it's implemented:

- How does it respond to user interactions?
- What states does it have? (loading, error, empty, etc.)
- How does it handle edge cases?
- What are the default behaviors?

Example:

```markdown
- Displays loading skeleton while fetching data
- Shows empty state message when no data is available
- Automatically adjusts column widths based on content
- Preserves sort/filter state when data updates
- Handles errors gracefully with error boundary and retry option
```

#### 4. Architecture & Patterns (2-4 points)

High-level design approach, not code details:

- What design pattern? (Compound Component, Render Props, Hooks-based, etc.)
- How are sub-components organized?
- How does data flow?
- Any special architectural considerations?

Example:

```markdown
- **Pattern**: Compound component pattern (DataGrid.Column, DataGrid.Row, etc.)
- **Data Flow**: Controlled component - parent manages data and state
- **Sub-components**: Column definitions are declarative, rows are auto-generated
- **Context**: Uses DataGridContext to share state between sub-components
```

#### 5. Usage Guidelines (DO's and DON'Ts)

Practical advice developers need:

**✅ DO:**

- When to use this component
- Recommended patterns
- Best practices

**❌ DON'T:**

- Common mistakes
- Anti-patterns
- What this component is NOT for

Example:

```markdown
✅ DO:

- Use for displaying 50+ rows of structured data
- Provide unique `rowKey` for each row for performance
- Use custom renderers for complex cell content
- Memoize callbacks to prevent unnecessary re-renders

❌ DON'T:

- Use for simple lists (use List component instead)
- Modify data prop directly (creates new array reference)
- Put heavy computations in render functions
- Use for real-time updating data (use DataStream component instead)
```

#### 6. Integration Points (2-4 points)

How does it work with other parts of the system?

- What components does it work with?
- What hooks or contexts does it use?
- What data formats does it expect?

Example:

```markdown
- **Data Format**: Expects array of objects with consistent keys
- **Design System**: Uses Button, Input, Select from unity-components
- **Hooks**: Can integrate with useQuery for data fetching
- **Styling**: Supports theme tokens from Theme component
```

#### 7. Common Use Cases (2-3 examples)

Brief descriptions of typical scenarios, not full code:

Example:

```markdown
1. **Product Catalog**: Display product list with filtering by category and price
2. **User Management**: Show user table with search, sort, and action buttons
3. **Report Dashboard**: Present analytics data with export functionality
```

## Success Criteria

Documentation is complete when:

- ✅ Any developer can understand what the component does in 2 minutes
- ✅ Common questions are answered without reading code
- ✅ Examples show when to use (and not use) the component
- ✅ Information is stable and won't change with every code change
- ✅ Focus is on behavior and usage, not implementation details

## Quality Standards

- **High-Level**: Focus on "what" and "why", not "how"
- **Stable**: Document concepts that rarely change, not specific props or methods
- **Practical**: Include information developers actually need
- **Concise**: Keep it brief - 1-2 pages maximum
- **Scannable**: Use headings, bullet points, clear structure

## What NOT to Include

❌ Specific prop names and types (these change)
❌ CSS class names (implementation detail)
❌ Exact test case descriptions (code-level detail)
❌ Internal state variable names (implementation detail)
❌ Line-by-line code explanations (too detailed)

## What TO Include

✅ Component purpose and value
✅ Key features and capabilities
✅ Behavioral patterns
✅ Usage guidelines and best practices
✅ Common use cases
✅ Integration with other components
✅ When to use vs. alternatives

