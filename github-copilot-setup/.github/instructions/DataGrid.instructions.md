---
applyTo: 'packages/components/src/DataGrid/*'
---

# DataGrid Component

## Component Overview

The `DataGrid` component is a high-performance, feature-rich data grid for displaying and interacting with structured datasets. It provides enterprise-grade functionality including sorting, filtering, pagination, row selection, drag-and-drop, column management, and virtual scrolling for datasets of any size. Use it whenever you need to display tabular data with user interactions, data manipulation, or complex data relationships. Located in `packages/components/src/DataGrid/`.

## Key Features

- **Virtual Scrolling**: Efficiently renders large datasets (1000+ rows) by only rendering visible cells in the viewport, with automatic performance optimization
- **Row Selection**: Single or multi-row selection with checkbox controls, attribute-based selection, and programmatic selection management
- **Column Management**: Drag-and-drop reordering, resizing, freezing (pin left/right), hiding/showing, and auto-fit width capabilities
- **Sorting & Filtering**: Multi-level column sorting and quick filters with customizable filter types per column
- **Expandable Rows**: Nested data display with custom renderers for expanded content, supporting hierarchical data structures
- **Row Actions**: Contextual action buttons per row with configurable visibility and behavior
- **Drag-and-Drop Rows**: Reorder rows via drag handles with multi-row drag support and validation callbacks
- **Column Grouping**: Group data by columns with collapsible groups and aggregate summaries
- **Pagination**: Client-side and server-side pagination with customizable page sizes and navigation controls
- **Fixed Layout**: Header and column freezing for consistent visibility during scrolling
- **Keyboard Navigation**: Full keyboard support for accessibility with arrow key cell navigation and tab traversal
- **Context Menus**: Header column context menus with custom actions and selection context menus for bulk operations
- **Custom Renderers**: Replace default cell rendering with custom components for complex data visualization

## Component Behavior

- **Performance Optimization**: Automatically enables virtual scrolling when cell count exceeds threshold (default: 1000 cells). Uses memoization and context to prevent unnecessary re-renders
- **Loading States**: Displays loading spinner overlay while fetching data, preventing interactions during loading
- **Empty States**: Shows customizable empty data placeholder when no data is available
- **Responsive Layout**: Automatically adjusts column widths and layout based on container size with breakpoints at 600px, 400px, and 250px
- **Height Management**: Supports fixed height (fills container), custom height (explicit pixel value), or auto-height (grows with content). Fixed mode enables vertical scrolling
- **Nested Grids**: Can be rendered inside expandable rows of parent grids, automatically managing height synchronization
- **State Preservation**: Maintains sort order, filter values, and scroll position when data updates
- **Error Handling**: Built-in development error detection for duplicate keys, missing keys, invalid data formats with console warnings
- **Controlled & Uncontrolled**: Supports both fully controlled (parent manages state) and uncontrolled (internal state) patterns for all features

## Architecture & Patterns

- **Context-Based Architecture**: Uses multiple React Contexts to share state between grid, header, body, and cell components without prop drilling
- **Feature Composition**: Features are modular hooks (`useFeatureRowSelection`, `useFeatureColumnGrouping`, etc.) that inject functionality into the core grid
- **Compound Component**: Exposes sub-components (`DataGrid.useRowSelection`, `DataGrid.CellWrapper`, etc.) for advanced customization
- **Virtual Grid Rendering**: Custom virtualization engine calculates visible rows/columns and renders only what's in viewport with offset buffers
- **Size-Aware Rendering**: Wrapped with `react-sizeme` to reactively recalculate layout when container dimensions change
- **Ref-Based Controls**: Exposes imperative API via ref for programmatic scrolling and focus management (`scrollToRow`, `focusCell`, etc.)

## Usage Guidelines

### ✅ DO:

- Use for displaying 20+ rows of structured, tabular data with consistent columns
- Provide unique `rowKey` matching a field in `dataSource` (defaults to `key`) for optimal performance and state management
- Use `fixed={true}` when grid should fill its container with scrollable body and sticky header
- Enable `columnsAutoWidth` when you want columns to distribute evenly within available space
- Use feature hooks (`useLocalFiltering`, `useServerFiltering`, `useRowSelection`) in parent component to manage complex state
- Memoize `columns` array and callback functions to prevent unnecessary re-renders
- Use `cellFormatterV2` for consistent data formatting across all cells with custom locale and number formatting
- Set `enableVirtualMinCellCount` appropriately based on your data size (lower for better performance, higher for simpler DOM)
- Leverage `rowClassName` and `rowHighlights` for visual feedback on row states (error, warning, selected, hover)
- Use `expandable` for hierarchical data or detailed row information that doesn't fit in standard cells
- Implement `headerContextMenu` for custom column actions like filtering, sorting presets, or data exports

### ❌ DON'T:

- Use for simple lists with single column or card-like layouts (use List or Card components instead)
- Modify `dataSource` or `columns` arrays in place (always create new array references)
- Put heavy computations or API calls inside cell renderers (pre-process data or memoize results)
- Use for real-time streaming data with frequent updates (causes full re-renders; batch updates instead)
- Enable all features simultaneously without considering performance impact (each feature adds complexity)
- Forget to handle loading and empty states (grid will show default messages, but custom is better UX)
- Use nested grids more than 2 levels deep (performance degrades and UX becomes confusing)
- Mix server-side and client-side pagination/sorting/filtering (choose one approach and stick with it)
- Override CSS classes without understanding BEM structure (can break responsive behavior)
- Use without TypeScript types (column types, data source types are critical for type safety)

## Integration Points

- **Data Format**: Expects `dataSource` as array of objects with consistent keys. Each record must have a unique key field (default: `key` property)
- **Design System**: Integrates with Pagination, Checkbox, Button, Icon, Spin, Menu, SkipLinks, and QuickFilter components from the component library
- **Feature Hooks**: Provides reusable hooks for common patterns:
  - `useLocalFiltering` - Client-side filtering logic
  - `useServerFiltering` - Server-side filtering with API integration
  - `useRowSelection` - Row selection state management
  - `useColumnGrouping` - Column grouping with aggregation
  - `useExpandable` - Expandable row state management
  - `useRowDragDrop` - Drag-and-drop row reordering
- **Formatters**: Uses `L18NContext` for internationalization and `cellFormatterV2` for consistent number, date, and text formatting
- **Accessibility**: Fully keyboard navigable with ARIA attributes, screen reader support, and skip links for navigation
- **Styling**: Follows BEM methodology with `.dataGrid` as base class. Uses Less variables for theming and design tokens
- **Testing**: Provides `data-test` attributes for E2E testing and custom test utilities in `test/queries.ts`

## Common Use Cases

1. **Product Catalog Management**: Display product list with filtering by category/price, sorting by popularity, bulk selection for operations, row actions for edit/delete, and expandable rows for detailed specifications

2. **User Administration**: Show user grid with search/filter, multi-level sorting (role → department → name), row selection for bulk actions (activate/deactivate), inline editing of user properties, and pagination for thousands of users

3. **Financial Data Dashboard**: Present transaction data with column freezing for key columns, custom cell renderers for currency/percentages, column summaries for totals, column grouping by date/category, and export functionality via header context menu

4. **Workflow Queue Management**: Display task queue with drag-and-drop row reordering for priority, row highlights for status (error/warning/success), real-time loading states during actions, expandable rows for task details, and selection context menu for bulk assignment

5. **Data Comparison View**: Show side-by-side data with column resizing for flexible viewing, column hiding for focus, fixed header for reference during scrolling, and custom cell renderers for visual diff indicators

## Feature-Specific Notes

### Virtual Scrolling

- Automatically enabled when `columns.length × rows.length > enableVirtualMinCellCount`
- Renders only visible cells plus 2-row/2-column buffer for smooth scrolling
- Disable by setting `enableVirtualMinCellCount` very high if you need all cells in DOM

### Row Selection

- Supports `byRowKey` (checkbox per row) and `byAttr` (selection by column value)
- `maxRows: 1` enables radio button behavior instead of checkboxes
- Can be disabled but still show selected state with `disabled: true`
- Integrates with selection context menu for bulk actions on selected rows

### Column Features

- **Freezing**: Pin columns left or right with `columnFreezing` - frozen columns scroll independently
- **Resizing**: Drag column borders to resize with `columnResizing` - persists via callback
- **Drag-Drop**: Reorder columns by dragging headers with `columnDragDrop`
- **Auto-fit**: Double-click column border or use context menu to fit content width
- **Hiding**: Toggle column visibility via header context menu with `columnHiding`

### Expandable Rows

- Custom renderer receives record, index, and nesting level
- Can specify fixed or dynamic `expandedRowHeight` (min 28px)
- Supports nested grids inside expanded content
- `showTotal` adjusts total row count to include collapsed children

### Pagination

- Client-side: Filter `dataSource` yourself, pass subset to grid
- Server-side: Use `useServerFiltering` hook to manage API calls and pagination state
- Customizable page sizes via `pageSizeOptions`
- Shows total count and current range via `showTotal`
