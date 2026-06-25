---
name: umbraco-dashboard
description: Implement dashboards in Umbraco backoffice using official docs
version: 1.0.0
location: managed
allowed-tools: Read, Write, Edit, WebFetch
---

# Umbraco Dashboard

## What is it?
Dashboards are customizable components that appear in Umbraco's backoffice sections to display information and functionality. They show an 'editor' for the selected item in the tree or default section information when no item is selected. Dashboards use conditions to control where and when they appear in the backoffice.

## Documentation
Always fetch the latest docs before implementing:

- **Main docs**: https://docs.umbraco.com/umbraco-cms/customizing/extending-overview/extension-types/dashboard
- **Foundation**: https://docs.umbraco.com/umbraco-cms/customizing/foundation
- **Extension Registry**: https://docs.umbraco.com/umbraco-cms/customizing/extending-overview/extension-registry
- **Tutorial**: https://docs.umbraco.com/umbraco-cms/tutorials/creating-a-custom-dashboard

## Reference Example

The Umbraco source includes a working example:

**Location**: `/Umbraco-CMS/src/Umbraco.Web.UI.Client/examples/dashboard-with-property-dataset/`

This example demonstrates a dashboard that uses property datasets for data binding. Study this for production patterns.

## Related Foundation Skills

If you need to explain these foundational concepts when implementing dashboards, reference these skills:

- **Umbraco Element / UmbElementMixin**: When implementing dashboard elements, explaining UmbElementMixin, UmbLitElement, or base class patterns
  - Reference skill: `umbraco-umbraco-element`

- **Context API**: When implementing context consumption (consumeContext), providing contexts, or accessing services like UMB_NOTIFICATION_CONTEXT
  - Reference skill: `umbraco-context-api`

- **Localization**: When implementing translations, using localize.term(), or adding multi-language support
  - Reference skill: `umbraco-localization`

- **State Management**: When implementing reactive state, using observables, UmbState, or @state() decorator
  - Reference skill: `umbraco-state-management`

- **Conditions**: When implementing visibility controls, section restrictions, or conditional rendering
  - Reference skill: `umbraco-conditions`

## Workflow

1. **Fetch docs** - Use WebFetch on the URLs above
2. **Ask questions** - What section? What functionality? Who can access? Should it appear in a specific position in the tab list?
3. **Generate files** - Create manifest + implementation based on latest docs
4. **Explain** - Show what was created and how to test

## Tab ordering

Use `weight` in the manifest to control where the dashboard appears in its section's tab list. **Higher weight = appears earlier (further left).** Umbraco's built-in dashboards use weights in the 100–500 range (e.g. "Getting Started" is around weight 20 in some versions — always check the docs). To appear first, use a higher value than the built-ins; to appear second, use a value between the first and third.

## Minimal Examples

### Manifest (manifest.ts)
```typescript
import type { ManifestDashboard } from '@umbraco-cms/backoffice/dashboard';

export const manifest: ManifestDashboard = {
  type: 'dashboard',
  alias: 'my.dashboard',
  name: 'My Dashboard',
  weight: 100, // higher = appears earlier in the tab list
  js: () => import('./my-dashboard.element.js'),
  meta: {
    label: 'My Dashboard',
    pathname: 'my-dashboard',
  },
  conditions: [
    {
      alias: 'Umb.Condition.SectionAlias',
      match: 'Umb.Section.Content',
    },
  ],
};
```

### Implementation (my-dashboard.element.ts)
```typescript
import { html, css } from '@umbraco-cms/backoffice/external/lit';
import { UmbLitElement } from '@umbraco-cms/backoffice/lit-element';
import { customElement } from '@umbraco-cms/backoffice/external/lit';

@customElement('my-dashboard')
export class MyDashboardElement extends UmbLitElement {
  override render() {
    return html`
      <uui-box headline="My Dashboard">
        <p>Dashboard content goes here</p>
      </uui-box>
    `;
  }

  static override styles = css`
    :host {
      display: block;
      padding: var(--uui-size-space-4);
    }
  `;
}

export default MyDashboardElement;

declare global {
  interface HTMLElementTagNameMap {
    'my-dashboard': MyDashboardElement;
  }
}
```

That's it! Always fetch fresh docs, keep examples minimal, generate complete working code.
