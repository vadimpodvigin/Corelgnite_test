# YAML Workflow Format

This directory contains workflow definitions in YAML format for CoreIgnite Docs website.

## Workflow Object Properties

| Property      | Type   | Required | Description                                                                                                                             |
| ------------- | ------ | -------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| `title`       | string | Yes      | Name of the workflow                                                                                                                    |
| `description` | string | Yes      | Detailed description of the workflow                                                                                                    |
| `category`    | string | Yes      | Category a workflow is placed under, this field can take one of the existing categories (listed below) or option to create new category |
| `icon`        | string | Yes      | Icon representative of the workflow, this field can only take one of the available options (listed below)                                |

### Workflow Object Schema

The root of each YAML file should contain a `workflow` object and a `cards` array. Optionally, you can define `sharedSideCards` for reusable side card groups:

```yaml
workflow:
  title: Workflow Title
  description: Workflow description
  category: CoreIgnite Setup | CoreFlow | <custom string>
  icon: piggy-bank | fragments | finance | money | book | application-mobile | user-profile

# Optional: Define reusable side card groups
sharedSideCards:
  groupName:
    - id: "unique-id"
      title: "Card Title"
      badge: "badge text"
      description: "Card description"
      icon: "icon-name"
      arrows: [...]

cards:
  - id: "card1"
    # ... card properties
```

### Current Workflow Categories

- **CoreIgnite Setup:** Workflows related to initial setup, configuration, and onboarding

- **CoreFlow:** Workflows related to CoreFlow operational processes

## Shared Side Cards (Optional)

Shared side cards allow you to define reusable side card groups that can be referenced by multiple cards. This helps maintain consistency and reduces duplication.

**_Shared Side Cards Properties_**

- `groupName` (object, required): Name of the side card group
  - `id` (string, required): Unique identifier for the side card
  - `title` (string, required): Card title
  - `badge` (string, required): Badge text/number
  - `description` (string, required): Card description
  - `icon` (string, required): Icon name (24px, for side cards)
  - `arrows` (array, required): Navigation arrows (same structure as card arrows)

```yaml
sharedSideCards:
  coreBankGroup:
    - id: "card31"
      title: "Core Bank Plugin (Hogan)"
      badge: ""
      description: "Pellentesque sit amet nunc sodales..."
      icon: "settings"
      arrows:
        - direction: "down"
          targetCardId: "card32"
```

## Card Object Properties

| Property         | Type     | Required | Description                                           |
| ---------------- | -------- | -------- | ----------------------------------------------------- |
| `id`             | `string` | **Yes**  | **Unique identifier for the card**                    |
| `title`          | `string` | **Yes**  | **The name of the step**                              |
| `badge`          | `string` | **Yes**  | **Step number to display on card (e.g., "1", "2.1")** |
| `description`    | `string` | **Yes**  | **Detailed description of the step**                  |
| `arrows`         | `array`  | **Yes**  | **Navigation arrows to other cards**                  |
| `accordion`      | `array`  | No       | Expandable/collapsible accordion sections             |
| `button`         | `object` | No       | Button with label and URL                             |
| `checkboxGroup`  | `object` | No       | Group of checkboxes with various states               |
| `codeSnippet`    | `object` | No       | Display snippet of code in a card                     |
| `icon`           | `string` | No       | Icon (24px, for side cards)                           |
| `list`           | `object` | No       | List component (ordered or unordered)                 |
| `nestedcards`    | `array`  | No       | Display nested cards within a card                    |
| `notifications`  | `array`  | No       | Display notification messages with types              |
| `sections`       | `object` | No       | Highlight sections within a card                      |
| `sideCardRef`    | `string` | No       | Reference to shared side cards group                  |
| `sideCardSpanEnd`| `string` | No       | ID where shared cards span ends (auto-calculated)     |
| `stepper`        | `object` | No       | Step-by-step progress indicator                       |
| `table`          | `object` | No       | Display data in table format with headers and rows    |
| `tabs`           | `array`  | No       | Show multiple tabs of content                         |
| `tags`           | `array`  | No       | Tags with label and optional color                    |

## Card Object Schema

Each card in the `cards` array follows this structure:

```yaml
cards:
  - id: "card1"
    title: "Card Title"
    badge: "1"
    description: "Card description"
    arrows: [...]
    accordion: [...] # Optional
    button: {...} # Optional
    checkboxGroup: {...} # Optional
    codeSnippet: {...} # Optional
    icon: "wallet" # Optional
    list: {...} # Optional
    nestedcards: [...] # Optional
    notifications: [...] # Optional
    sections: {...} # Optional
    sideCardRef: "groupName" # Optional
    sideCardSpanEnd: "card3" # Optional
    stepper: {...} # Optional
    table: {...} # Optional
    tabs: [...] # Optional
    tags: [...] # Optional
```

### Arrows

Arrows define the flow between cards.

**_Arrow Properties_**

- `direction` (string, required): Direction: "down", "right", "left", or "up"
- `targetCardId` (string, required): ID of the target card
- `rowIndex` (number, optional): For horizontal arrows (0=top, 1=middle, etc.)

```yaml
arrows:
  - direction: up | down | right | left
    targetCardId: card2
    rowIndex: 0 # Optional
```

If on the last card, Arrows array can be left empty, but must be listed since it is a required property of a card like so:

```yaml
arrows: []
```

### Accordion (Optional)

Accordion allows you to create expandable/collapsible sections within a card.

Since this property is optional, if a card does not require an accordion make sure to omit it entirely from a card object.

**_Accordion Item Properties_**

- `title` (string, required): Accordion item title
- `content` (string, required): Content to display when expanded
- `defaultExpanded` (boolean, optional): Whether the item is expanded by default
- `disabled` (boolean, optional): Whether the item is disabled

```yaml
accordion:
  - title: Expanded
    content: Donec ornare lacus id risus venenatis.
    defaultExpanded: true
  - title: Collapsed
    content: Content for the second item
  - title: Disabled
    content: Proin sit amet accumsan turpis.
    disabled: true
```

### Button (Optional)

Button allows you to add a clickable button to a card that links to an external URL.

Since this property is optional, if a card does not require a button make sure to omit it entirely from a card object.

**_Button Properties_**

- `label` (string, required): Text to display on the button
- `url` (string, required): URL to navigate to when button is clicked
- `type` (string, required): Style variations of button

```yaml
button:
  label: View Documentation
  url: https://example.com/docs
  type: primary | tertiary
```

### CheckboxGroup (Optional)

CheckboxGroup allows you to display a group of checkboxes with various states within a card.

Since this property is optional, if a card does not require a checkbox group make sure to omit it entirely from a card object.

**_CheckboxGroup Properties_**

- `title` (string, required): Title for the checkbox group
- `items` (array, required): Array of checkbox items

**_Checkbox Item Properties_**

- `label` (string, required): Checkbox label text
- `state` (string, required): State of the checkbox: `"checked"`, `"unchecked"`, or `"error"`

```yaml
checkboxGroup:
  title: Nullam elementum
  items:
    - label: Nunc ut porta diam
      state: checked
    - label: Suspendisse congue magna
      state: error
    - label: Vestibulum metus orci
      state: unchecked
```

### Code Snippet (Optional)

Code Snippet allows you to display some simple code in its own section with content displayed in a monospaced block. Note this is not meant for complex or long pieces of code, but just to make references to helpful details.

Since this property is optional, if a card does not require a code snippet make sure to omit it entirely from a card object.

**_Code Snippet Properties_**

- `code` (object, required): Code object containing language and snippet
  - `language` (string, required): Code language
  - `snippet` (string, required): Code content
- `caption` (string, optional): Brief text underneath code to provide any comments/explanations

```yaml
codeSnippet:
  code:
    language: python
    snippet: |
      import os
      print("Hello World")
  caption: Code snippet caption
```

### Icon (Optional)

Icon allows you to display a 24px icon on a card, typically used for side cards.

**_Icon Values_**

Available icon options: `"wallet"`, `"settings"`, `"database"`, `"book"`, `"piggy-bank"`, `"fragments"`, `"application-mobile"`

```yaml
icon: wallet
```

### List (Optional)

List allows you to display an ordered or unordered list within a card or section.

Since this property is optional, if a card does not require a list make sure to omit it entirely from a card object.

**_List Properties_**

- `type` (string, optional): `"ordered"` or `"unordered"` (default: `"unordered"`)
- `nestedType` (string, optional): For nested items, `"alpha"` or `"bullet"` (default: `"bullet"`)
- `items` (array, required): Array of list items

**_List Item Properties_**

- `text` (string, required): Text content of the list item
- `nested` (array, optional): Nested list items with same structure

```yaml
list:
  type: ordered
  nestedType: alpha
  items:
    - text: Item 1
    - text: Item 2
      nested:
        - text: Nested item 2.1
        - text: Nested item 2.2
```

### Nested Cards (Optional)

Nested cards are smaller cards within a card. Since this property is optional, if a card does not required nested cards, make sure to omit it entirely from a card object.

**_Nested Card Properties_**

- `title` (string, required): Nested card name
- `subtext` (string, optional): Nested card subtext

```yaml
nestedcards:
  - title: Nested Card Title
    subtext: Nested card description
  - title: Another Nested Card
```

### Notifications (Optional)

Notifications allow you to display notification messages with different types (info, success, warning, error) within a card.

Since this property is optional, if a card does not require notifications make sure to omit it entirely from a card object.

**_Notification Item Properties_**

- `type` (string, required): Type of notification: `"info"`, `"success"`, `"warning"`, or `"error"`
- `title` (string, required): Notification title
- `message` (string, required): Notification message

```yaml
notifications:
  - type: info
    title: Info
    message: Aliquam id ultricies dolor.
  - type: success
    title: Success
    message: Proin sit amet accumsan turpis
  - type: warning
    title: Warning
    message: Pellentesque ac lorem
  - type: error
    title: Error
    message: "502"
```

### Sections (Optional)

Sections are sub-sections within a card. Since this property is optional, if a card does not required sections make sure to omit it entirely from a card object.

**_Sections Object Properties_**

- `direction` (string, required): Layout direction for sections. Must be either `"col"` or `"row"`
- `items` (array, required): Array of section objects

**_Section Item Properties_**

- `title` (string, required): Section card name
- `badge` (string, required): Sub-step number to display on card (e.g., "1.1", "A")
- `description` (string, optional): Section card subtext
- `button` (object, optional): Button object (same structure as card button)
- `codeSnippet` (object, optional): Code snippet object (same structure as card codeSnippet)
- `icon` (string, optional): Icon name (24px, for side cards)
- `list` (object, optional): List object (same structure as card list)
- `nestedcards` (array, optional): Array of nested cards (same structure as card nestedcards)
- `tabs` (array, optional): Array of tabs (same structure as card tabs)
- `tags` (array, optional): Tags array (same structure as card tags)

```yaml
sections:
  direction: col
  items:
    - title: Section Title
      badge: 1.1
      description: Section description
    - title: Section Title
      badge: 1.2
      button:
        label: Learn More
        url: https://example.com
      icon: wallet
      tags:
        - label: Tag 1
          color: "#FF5733"
      list:
        type: ordered
        items:
          - text: Item 1
```

### Side Card Reference (Optional)

Side card reference allows a card to reference a shared side card group defined in `sharedSideCards`.

**_Side Card Reference Properties_**

- `sideCardRef` (string, optional): Name of the shared side card group to reference
- `sideCardSpanEnd` (string, optional): ID of the card where the shared cards span ends (usually auto-calculated)

```yaml
sideCardRef: coreBankGroup
sideCardSpanEnd: card3
```

### Stepper (Optional)

Stepper allows you to display a step-by-step progress indicator within a card.

Since this property is optional, if a card does not require a stepper make sure to omit it entirely from a card object.

**_Stepper Properties_**

- `direction` (string, required): Direction of the stepper: `"vertical"` or `"horizontal"`
- `steps` (array, required): Array of step objects

**_Step Item Properties_**

- `label` (string, required): Step label
- `description` (string, required): Step description
- `state` (string, required): State of the step: `"completed"`, `"current"`, `"incomplete"`, or `"error"`

```yaml
stepper:
  direction: vertical
  steps:
    - label: Donec ultrices
      description: Nunc ut porta diam
      state: completed
    - label: Suspendisse congue
      description: Class aptent taciti sociosqu
      state: current
    - label: Proin porttitor
      description: Aliquam id ultricies
      state: incomplete
    - label: In nec dolor
      description: Nunc ut porta diam
      state: error
```

### Table (Optional)

Table allows you to display data in a table format with headers and rows within a card.

Since this property is optional, if a card does not require a table make sure to omit it entirely from a card object.

**_Table Properties_**

- `headers` (array, required): Array of header strings
- `rows` (array, required): Array of row arrays, where each row contains cell values

```yaml
table:
  headers:
    - Class aptent
    - Aliquam ut
    - Nunc ut porta
  rows:
    - [Pellentesque habitant, Nunc ut porta diam, Suspendisse congue magna]
    - [Aliquam, Class aptent taciti, Vestibulum metus orci]
    - [Vestibulum quis ante ex, Proin sed nisi nulla, Proin porttitor tincidunt]
```

### Tabs (Optional)

Tabs allows you to show multiple tabs of content within a card. A viewer will be able to toggle between the different tabs. Make sure to format your tabs in the order you would like them to be displayed.

Since this property is optional, if a card does not required tabs make sure to omit it entirely from a card object.

**_Tab Item Properties_**

- `id` (string, required) Unique Identifier
- `label` (string, required): Tab label/name
- `content` (object, required): Content object containing tab content

**_Content Object Properties_**

- `text` (string, optional): Text description for the tab
- `codeSnippet` (object, optional): Code snippet object (same structure as card codeSnippet)
- `button` (object, optional): Button object (same structure as card button)
- `icon` (string, optional): Icon name (24px, for side cards)
- `list` (object, optional): List object (same structure as card list)
- `nestedcards` (array, optional): Array of nested cards (same structure as card nestedcards)
- `sections` (object, optional): Sections object (same structure as card sections)
- `tabs` (array, optional): Array of tabs (same structure as card tabs)
- `tags` (array, optional): Tags array (same structure as card tags)

```yaml
tabs:
  - id: tab2
    label: Tab 2
    content:
      codeSnippet:
        language: <language name>
        snippet: <custom code>
        caption: Code snippet caption
```

**If `type` is `"sections"`:**
- `sections` (object, required): Sections object with same structure as card sections

```yaml
tabs:
  - label: Tab 1
    content:
      description: Tab 1 description
  - label: Tab 2
    content:
      button:
        label: View More
        url: https://example.com
      description: Tab 2 description
      nestedcards:
        - title: Nested Card Title
          subtext: Nested card description
      sections:
        direction: col
        items:
          - title: Section Title
            badge: 2.1
            description: Section description
```

### Tags (Optional)

Tags allow you to display labeled tags on a card or section, optionally with custom colors.

Since this property is optional, if a card does not require tags make sure to omit it entirely from a card object.

**_Tag Properties_**

- `label` (string, required): Tag label text
- `color` (string, optional): Custom hex color for the tag (e.g., "#FF5733")

```yaml
tags:
  - label: Tag 1
    color: "#FF5733"
  - label: Tag 2
```

## Complete Schema

```yaml
workflow:
  title: Workflow Title
  description: Workflow Description
  category: CoreIgnite Setup | CoreFlow | <custom string>
  icon: piggy-bank | fragments | finance | money | book | application-mobile | user-profile

sharedSideCards:
  groupName:
    - id: "unique-id"
      title: "Card Title"
      badge: "badge text"
      description: "Card description"
      icon: "icon-name"
      arrows: [...]

cards:
  - id: card1 # Card with only required properties
    title: Card Title
    badge: "1"
    description: Card description
    arrows:
      - direction: up | down | right | left
        targetCardId: cardx

  - id: card2 # Card with all possible properties
    title: Card Title
    badge: "2"
    description: Card description
    arrows:
      - direction: up | down | right | left
        targetCardId: cardx
        rowIndex: 0 # Optional
    accordion:
      - title: Accordion Item
        content: Accordion content
        defaultExpanded: true
    button:
      label: View Documentation
      url: https://example.com/docs
    checkboxGroup:
      title: Checkbox Group Title
      items:
        - label: Checkbox 1
          state: checked
        - label: Checkbox 2
          state: unchecked
    codeSnippet: 
      code: <custom code>
      caption: Code Snippet Caption
    icon: wallet
    list:
      type: ordered
      nestedType: alpha
      items:
        - text: Item 1
        - text: Item 2
          nested:
            - text: Nested item 2.1
    nestedcards:
      - title: Nested Card Title
        subtext: Nested card description
      - title: Another Nested Card
    notifications:
      - type: info
        title: Info
        message: Information message
    sections:
      direction: col
      items:
        - title: Section Title
          badge: 2.1
          description: Section description
        - title: Section Title
          badge: 2.2
          button:
            label: Learn More
            url: https://example.com
          icon: wallet
          list:
            type: ordered
            items:
              - text: Item 1
          tabs:
            - label: Tab 1
              content:
                text: Tab content
          tags:
            - label: Tag 1
              color: "#FF5733"
    sideCardRef: groupName
    sideCardSpanEnd: card3
    stepper:
      direction: vertical
      steps:
        - label: Step 1
          description: Step description
          state: completed
    table:
      headers:
        - Header 1
        - Header 2
      rows:
        - [Row 1 Col 1, Row 1 Col 2]
        - [Row 2 Col 1, Row 2 Col 2]
    tabs:
      - label: Tab 1
        content:
          text: Tab 1 content
      - label: Tab 2
        content:
          code: console.log('code');
          caption: Optional caption
      - label: Tab 3
        content:
          sections:
            direction: col
            items:
              - title: Section Title
                badge: 2.1
                description: Section description
    tags:
      - label: Tag 1
        color: "#FF5733"
      - label: Tag 2
```

## Best Practices

1. **Unique IDs**: Ensure each card has a unique `id` value
2. **Valid Targets**: All `targetCardId` values in arrows must reference existing card IDs
3. **Consistent Badges**: Use consistent numbering schemes (e.g., "1", "2", "3" or "1.0", "2.0", "3.0")
4. **Complete Workflows**: Ensure workflows have a clear start and end (cards with no incoming/outgoing arrows)
5. **Descriptive Content**: Provide meaningful titles and descriptions for better understanding
6. **Alphabetical Ordering**: After required properties, organize optional properties alphabetically for consistency

## YAML Syntax Notes

- Use 2-space indentation consistently
- Use dashes (`-`) for array items
- Use colons (`:`) for key-value pairs
- Quote strings that contain special characters or numbers
- Empty arrays can be represented as `[]`
- Use `|` for multi-line strings to preserve newlines
- Use `>` for folded block scalars to convert newlines to spaces
