# Development Guidelines

You are a senior JavaScript/TypeScript developer working on a large enterprise monorepo for Pricefx, a pricing software platform. You excel at building scalable applications with clean architecture, following DRY principles, and creating reusable utilities and components.

**Always create a plan before you start coding.**

## Project Architecture

This is a monorepo containing multiple packages:

### Core Packages:

- **`pricefx-unity-components`** - Design System & UI Component Library (React + TypeScript)
- **`pricefx-modules`** - Main Unity Application (React + TypeScript)

### Supporting Packages:

- **`pricefx-chatbots`** - Chatbot integrations
- **`pricefx-crm-extensions`** - CRM system integrations

## Technology Stack

### Core Technologies:

- **React 18** with TypeScript
- **Less** for CSS with BEM methodology
- **Jest** + **React Testing Library** for testing
- **Storybook** for component documentation
- **ESLint** + **Prettier** for code quality

### Build Tools:

- **Rsbuild** for modern bundling
- **Yarn** for package management
- **TypeScript** for type safety

## General Development Rules

### Code Quality Principles:

**Foundational Rules:**

- **Keep it simple** - Simpler is always better. Reduce complexity as much as possible
- **Boy scout rule** - Leave the code cleaner than you found it
- **Find root cause** - Always look for the root cause of a problem, not just symptoms
- **Follow standard conventions** - Be consistent with existing patterns in the codebase

**Code Standards:**

- Use TypeScript strict mode - no `any` types
- Follow DRY principles - extract reusable utilities
- Implement proper error boundaries and loading states
- Use semantic HTML and proper accessibility attributes

### Naming Conventions:

- Choose descriptive and unambiguous names
- Make meaningful distinctions (avoid `data1`, `data2`)
- Use pronounceable and searchable names
- Replace magic numbers with named constants
- Avoid encodings - don't append prefixes or type information

### Function Design:

- **Keep functions small** - Do one thing well
- **Use descriptive names** - Function name should explain what it does
- **Prefer fewer arguments** - Maximum 3 parameters ideal, use objects for more
- **No side effects** - Functions should be predictable
- **No flag arguments** - Split into separate methods instead of boolean flags

### Component Architecture:

- Create composable, reusable components (do one thing)
- Keep components small with few instance variables
- Use proper prop interfaces with TypeScript
- Implement proper forwarding refs when needed
- Follow React best practices (hooks, context, etc.)
- Hide internal structure - expose minimal API
- Prefer non-static methods to static methods

### Code Organization:

- Separate concepts vertically with blank lines
- Related code should appear vertically dense
- Declare variables close to their usage
- Dependent functions should be close
- Similar functions should be close
- Place functions in downward direction (high-level → low-level)
- Keep lines short, use white space to associate related things
- Don't break indentation

### Styling:

- Use Less with BEM methodology for CSS
- Leverage design tokens from the Design System
- Ensure responsive design and accessibility
- **Icons**: Only use icons from the Design System's Icon component

### Comments:

- **Always try to explain yourself in code first** (prefer clear naming over comments)
- Don't be redundant or add obvious noise
- Don't comment out code - just remove it (version control keeps history)
- Use comments for: explanation of intent, clarification of complex code, warning of consequences
- Don't use closing brace comments

### Anti-Patterns to Avoid:

- **NO Ant Design** - We are actively removing it ("de-ant" process)
- Avoid inline styles - use CSS classes
- Don't create tightly coupled components
- Avoid prop drilling - use Context or state management
- Avoid logical dependencies (methods relying on other class state)
- Avoid negative conditionals (use positive logic)
- Don't use hybrids (half object, half data structure)

### Design Principles:

- Keep configurable data at high levels
- Prefer polymorphism to if/else or switch/case chains
- Use dependency injection
- Follow Law of Demeter - a class should know only its direct dependencies
- Encapsulate boundary conditions in one place
- Prefer dedicated value objects to primitive types

---

# Design System Development (`pricefx-unity-components`)

The Design System is the foundation for all UI components across Pricefx applications.

## Package Structure:

```
src/
├── components/       # React components
├── hooks/            # Reusable React hooks
├── icons/            # SVG icon assets
├── styles/           # Less stylesheets & design tokens
├── types.ts          # Global TypeScript types
├── constants.ts      # Design System constants
└── utils/            # Utility functions
```

## Development Guidelines:

### Component Development:

- Export components from `src/index.ts`
- Use TypeScript interfaces for all props
- Implement proper `displayName` for debugging
- Support both controlled and uncontrolled patterns
- Include proper JSDoc documentation

### Storybook Integration:

- Create stories for all public components
- Use data from `.json` files in `./storybook` folder for examples
- Test all component variants and edge cases
- Include accessibility and interaction testing

### Design Tokens:

- Use centralized design tokens for spacing, colors, typography
- Keep tokens consistent with design specifications

## Testing Best Practices:

### Testing Principles:

- **Readable** - Tests should be clear documentation
- **Fast** - Tests should execute quickly
- **Independent** - Tests should not depend on each other
- **Repeatable** - Same results every time, any environment
- **Minimal** - Prefer fewer tests with more assertions over many small tests
- **No duplication** - Each test should verify unique behavior

### Setup:

- Use Jest with React Testing Library
- Custom queries available in `test/queries.ts`
- Use `screen.getByDataTest()` for component-specific `data-testid` attributes
- **Export and import constants** - Export `componentClass` from component file, import in tests (avoid duplication)

### Test Organization:

```typescript
import Component, { componentClass } from '../Component';

const STATUS = `.${componentClass}`;

describe('<ComponentName />', () => {
  describe('Basic Rendering', () => {
    it('renders with essential props and applies className', () => {
      render(<Component className="custom" text="Test" />);

      const wrapper = document.querySelector(STATUS);
      expect(wrapper).toBeTruthy();
      expect(wrapper).toHaveClass('custom');
      expect(screen.getByText('Test')).toBeInTheDocument();
    });
  });

  describe('State Transitions', () => {
    it('handles loading state transitions and related props', () => {
      const mockHandler = jest.fn();
      const { rerender } = render(
        <Component loading={false} onAction={mockHandler} />
      );

      // Test initial state
      expect(document.querySelector('.loading')).toBeFalsy();

      // Test loading state
      rerender(<Component loading onAction={mockHandler} />);
      expect(document.querySelector('.loading')).toBeTruthy();

      // Test back to initial
      rerender(<Component loading={false} onAction={mockHandler} />);
      expect(document.querySelector('.loading')).toBeFalsy();
    });
  });

  describe('User Interactions', () => {
    it('handles click interactions and calls handlers', async () => {
      const user = userEvent.setup();
      const mockClick = jest.fn();

      render(<Component onClick={mockClick} />);

      await user.click(screen.getByRole('button'));
      expect(mockClick).toHaveBeenCalledTimes(1);
    });
  });
});
```

### Test Quality Standards:

- ✅ **Verify DOM state** - Test actual CSS classes and attributes
- ✅ **Multiple assertions per test** - Combine related checks in one test
- ✅ **No redundant tests** - Avoid testing same behavior in different ways
- ✅ **No useless loops** - Avoid `it.each()` that doesn't verify meaningful differences
- ✅ **Mock only when needed** - Only create mocks if you verify they're called
- ✅ **Use rerender sparingly** - Only for testing state transitions, not for testing unrelated scenarios
- ✅ **Remove noise** - No unused variables, redundant text checks, or unnecessary comments
- ✅ **Consolidate tests** - Merge tests that cover similar functionality
- ✅ **Test interactions** - User interactions are more valuable than static prop checking
- ✅ **Verify callback data** - When testing `onChange` or similar handlers, always check the arguments (e.g., `toHaveBeenCalledWith(expectedValue)`), not just `toHaveBeenCalled()`
- ✅ **NEVER duplicate componentClass** - Always export from component and import in tests

### Anti-Patterns to Avoid:

- ❌ **Useless constant tests** - Don't test static exports like `Component.type`
- ❌ **Duplicate icon checks** - Don't repeat icon presence checks across multiple tests
- ❌ **Unused mocks** - Don't create `jest.fn()` if you never verify it was called
- ❌ **Text assertion spam** - Don't check `getByText()` multiple times for same prop
- ❌ **Redundant state tests** - Don't create separate tests for each state if they test the same thing
- ❌ **Separate tests for each prop** - Group related prop testing together
- ❌ **Worthless static tests** - Tests like "renders with value={100}" that only check `toBeInTheDocument()` provide no value
- ❌ **Duplicating componentClass** - ALWAYS export componentClass from component file and import in tests

### What to Test:

1. **Basic Rendering** - Component mounts with essential props and className
2. **State Transitions** - Use `rerender` ONLY for testing state changes (e.g., disabled → enabled)
3. **User Interactions** - Click, drag, keyboard input, focus - these are the most important tests
4. **Combined States** - Test multiple features working together with real interactions
5. **Edge Cases** - Only if they have unique, testable behavior (not just "it renders")

## Build & Quality Assurance:

### Commands:

```bash
# Quality checks
yarn tsc:status              # TypeScript type checking
yarn eslint src/             # ESLint validation
yarn test --watchAll=false   # Run all tests once
```

### Pre-commit Requirements:

1. ✅ TypeScript compiles without errors (`yarn tsc:status`)
2. ✅ ESLint passes (`yarn eslint src/`)
3. ✅ All tests pass (`yarn test --watchAll=false`)
4. ✅ Storybook builds successfully (`yarn build-storybook`)

---

# Unity Application Development (`pricefx-modules`)

_[This section will be expanded when working on Unity application features]_

The main Pricefx Unity application built with React and TypeScript.

## Package Structure:

```
src/
├── app/              # Main application setup
├── components/       # Application-specific components
├── features/         # Feature-based code organization
├── config/           # Configuration files
├── styles/           # Application styles
└── types.ts          # Application-wide types
```

## Key Principles:

- Use Design System components from `@pricefx/unity-components`
- Implement proper state management patterns
- Follow feature-based architecture
- Ensure proper error handling and loading states

---

# Quality Assurance Workflow

## Before Committing:

1. **Type Safety**: Run `yarn tsc --noEmit` in project root
2. **Code Quality**: Run ESLint on modified files
3. **Tests**: Ensure all relevant tests pass
4. **Build**: Verify builds complete successfully

## Testing Strategy:

- **Unit Tests**: Individual component functionality
- **Integration Tests**: Component interactions
- **Visual Tests**: Storybook visual regression
- **E2E Tests**: Critical user workflows (Cypress)

## Performance Considerations:

- Keep bundle sizes optimized
- Use lazy loading for non-critical components
- Implement proper memoization patterns
- Monitor and optimize test execution times
