# YAML Workflow Format

This directory contains workflow definitions in YAML format for CoreIgnite Docs website.

## Workflow Object Properties

| Property      | Type   | Required | Description                                                                                                                             |
| ------------- | ------ | -------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| `title`       | string | Yes      | Name of the workflow                                                                                                                    |
| `description` | string | Yes      | Detailed description of the workflow                                                                                                    |
| `category`    | string | Yes      | Category a workflow is placed under, this field can take one of the existing categories (listed below) or option to create new category |
| `icon`        | string | Yes      | Icon represntitive of the workflow, this field can only take one of the available options (listed below)                                |

### Workflow Object Schema

The root of each YAML file should contain a `workflow` object and a `cards` array:

```yaml
workflow:
  title: Workflow Title
  description: Workflow description
  category: CoreIgnite Setup | CoreFlow | <custom string>
  icon: piggy-bank | fragments | finance | money | book | application-mobile | user-profile
```

### Current Workflow Categories

- **CoreIgnite Setup:** Workflows related to initial setup, configuration, and onboarding

- **CoreFlow:** Workflows related to CoreFlow operational processes

## Card Object Properties

| Property      | Type     | Required | Description                                           |
| ------------- | -------- | -------- | ----------------------------------------------------- |
| `id`          | `string` | **Yes**  | **Unique identifier for the card**                    |
| `title`       | `string` | **Yes**  | **The name of the step**                              |
| `badge`       | `string` | **Yes**  | **Step number to display on card (e.g., "1", "2.1")** |
| `description` | `string` | **Yes**  | **Detailed description of the step**                  |
| `arrows`      | `array`  | **Yes**  | **Navigation arrows to other cards**                  |
| `codeSnippet` | `object` | No       | Display snippet of code in a card                     |
| `nestedcards` | `array`  | No       | Display nested cards within the card                  |
| `sections`    | `array`  | No       | Highlight sections within a card                      |
| `tabs`        | `array`  | No       | Show multiple tabs of content                         |

## Card Object Schema

Each card in the `cards` array follows this structure:

```yaml
cards:
    - id: card1
    title: Card Title
    badge: "1"
    description: Card description
    arrows: [...]
    codeSnippet: {...} # Optional
    nestedcards: [...] # Optional
    sections: [...] # Optional

```

### Arrows

Arrows define the flow between cards.

**_Arrow Properties_**

- `direction` (string, required): Direction: "down", "right", "left", or "up"
- `targetCardId` (string, required): ID of the target card

```yaml
arrows:
  - direction: up | down | right | left
    targetCardId: card2
```

If on the last card, Arrows array can be left empty, but must be listed since it is a required property of a card like so:

```yaml
arrows: []
```

### Code Snippet (Optional)

Code Snippet allows you to display some simple code in its own section with content displayed in a monospaced block. Note this is not meant for complex or long pieces of code, but just to make references to helpful details.

Since this property is optional, if a card does not required sections make sure to omit it entirely from a card object.

**_Code Snippet Properties_**

- `code` (string, required): Code content
- `caption` (string, optional): Brief text underneath code to provide any comments/explainations

```yaml
codeSnippet:
  code: <custom code>
  caption: Code snippet caption
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
  - title: Another Nested Card
```

### Sections (Optional)

Sections are sub-sections within a card. Since this property is optional, if a card does not required sections make sure to omit it entirely from a card object.

**_Sections Object Properties_**

- `direction` (string, required): Layout direction for sections. Must be either "col" or "row"
- `items` (array, required): Array of section objects

**_Section Item Properties_**

- `title` (string, required): Section card name
- `badge` (string, required): Sub-step number to display on card (e.g., "1.1", "1.2")
- `description` (string, optional): Section card subtext
- `nestedcards` (array, optional): Array of nested cards (same structure as card nestedcards)
- `tabs` (array, optional): Array of tabs (same structure as card tabs)
- `codeSnippet` (object, optional): Code snippet object (same structure as card codeSnippet)

```yaml
sections:
  direction: col
  items:
    - title: Section Title
      badge: 1.1
      description: Section description
    - title: Section Title
      badge: 1.2
      description: Section description
      nestedcards:
        - title: Nested Card Title
          subtext: Nested card description
    - title: Section Title
      badge: 1.3
      description: Section description
      codeSnippet:
        code: <custom code>
        caption: Code snippet caption
    - title: Section Title
      badge: 1.4
      description: Section description
      tabs:
        - id: tab1
          label: Tab 1
          content:
            description: Tab content
```

### Table (Optional)
Table allows you to show a data table with custom defined rows and columns. There is a maximum of 10 rows and/or columns allowed on this component to maintain easy readability. 

Since this property is optional, if a card does not required a table make sure to omit it entirely from a card object.

**_Table Properties_**



### Tabs (Optional)

Tabs allows you to show multiple tabs of content within a card. A viewer will be able to toggle between the different tabs. Make sure to format your tabs in the order you would like them to be displayed.

Since this property is optional, if a card does not required tabs make sure to omit it entirely from a card object.

**_Tab Item Properties_**

- `id` (string, required): Unique identifier for the tab
- `label` (string, required): Tab label/name
- `content` (object, required): Content object containing tab content

**_Content Object Properties_**

- `description` (string, optional): Text description for the tab
- `sections` (object, optional): Sections object (same structure as card sections)
- `nestedcards` (array, optional): Array of nested cards (same structure as card nestedcards)
- `codeSnippet` (object, optional): Code snippet object (same structure as card codeSnippet)

```yaml
tabs:
  - id: tab1
    label: Tab 1
    content:
      description: Tab 1 description
  - id: tab2
    label: Tab 2
    content:
      description: Tab 2 description
      sections:
        direction: col
        items:
          - title: Section Title
            badge: 2.1
            description: Section description
  - id: tab3
    label: Tab 3
    content:
      description: Tab 3 description
      nestedcards:
        - title: Nested Card Title
          subtext: Nested card description
```

## Complete Schema

```yaml
workflow:
  title: Workflow Title
  description: Workflow Description
  category: CoreIgnite Setup | CoreFlow | <custom string>
  icon: piggy-bank | fragments | finance | money | book | application-mobile | user-profile

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
    codeSnippet: 
        code: <custom code>
        caption: Code Snippet Caption
    nestedcards:
      - title: Nested Card Title
        subtext: Nested card description
      - title: Another Nested Card
      - title: Another Nested Card
    sections:
      direction: col
      items:
        - title: Section Title
          badge: 2.1
          description: Section description
        - title: Section Title
          badge: 2.2
          description: Section description
          nestedcards:
            - title: Nested Card Title
              subtext: Nested card description
        - title: Section Title
          badge: 2.3
          description: Section description
          codeSnippet:
            code: <custom code>
            caption: Code snippet caption
        - title: Section Title
          badge: 2.4
          description: Section description
          tabs:
            - id: tab1
              label: Tab 1
              content:
                description: Tab content
    tabs:
      - id: tab1
        label: Tab 1
        content:
          description: Tab 1 content
      - id: tab2
        label: Tab 2
        content:
          description: Tab 2 content
      - id: tab3
        label: Tab 3
        content:
          description: Tab 3 content
```

## Best Practices

1. **Unique IDs**: Ensure each card has a unique `id` value
2. **Valid Targets**: All `targetCardId` values in arrows must reference existing card IDs
3. **Consistent Badges**: Use consistent numbering schemes (e.g., "1", "2", "3" or "1.0", "2.0", "3.0")
4. **Complete Workflows**: Ensure workflows have a clear start and end (cards with no incoming/outgoing arrows)
5. **Descriptive Content**: Provide meaningful titles and descriptions for better understanding

## YAML Syntax Notes

- Use 2-space indentation consistently
- Use dashes (`-`) for array items
- Use colons (`:`) for key-value pairs
- Quote strings that contain special characters or numbers
- Empty arrays can be represented as `[]`
