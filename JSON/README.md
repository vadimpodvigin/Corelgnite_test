# JSON Format

This directory contains workflow definitions in JSON format for CoreIgnite Docs website.

## Workflow Object Properties

| Property      | Type   | Required | Description                                                                                                                             |
| ------------- | ------ | -------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| `title`       | string | Yes      | Name of the workflow                                                                                                                    |
| `description` | string | Yes      | Detailed description of the workflow                                                                                                    |
| `category`    | string | Yes      | Category a workflow is placed under, this field can take one of the existing categories (listed below) or option to create new category |
| `icon`        | string | Yes      | Icon represntitive of the workflow, this field can only take one of the available options (listed below)                                |

### Workflow Object Schema

The root of each JSON file should contain a `workflow` object and a `cards` array:

```json

"workflow": {
    "category": "CoreIgnite Setup" | "CoreFlow" | "<custom string>",
    "title": "Workflow Title",
    "description": "Workflow description",
    "icon": "piggy-bank" | "fragments" | "finance" | "money" | "book" | "application-mobile" | "user-profile"
},

```

## Card Object Properties

| Property      | Type     | Required | Description                                       |
| ------------- | -------- | -------- | ------------------------------------------------- |
| `id`          | `string` | Yes      | Unique identifier for the card                    |
| `title`       | `string` | Yes      | The name of the step                              |
| `badge`       | `string` | Yes      | Step number to display on card (e.g., "1", "2.1") |
| `description` | `string` | Yes      | Detailed description of the step                  |
| `nestedcards` | `array`  | No       | Array of nested cards within the card             |
| `sections`    | `array`  | No       | Array of sections within the card                 |
| `arrows`      | `array`  | Yes      | Array of navigation arrows to other cards         |

### Card Object Schema

Each card in the `cards` array follows this structure:

```json
"cards": [
    {
        "id": "card1",
        "title": "Card Title",
        "badge": "1",
        "description": "Card description",
        "nestedcards": [...],  // Optional
        "sections": [...],  // Optional
        "arrows": [...]
    },
    {...}
]
```

### Nested Cards (Optional)

Nested cards are smaller cards within a card. Since this property is optional, if a card does not required nested cards, make sure to omit it entirely from a card object.

**_Nested Card Properties_**

- `title` (string, required): Nested card name
- `subtext` (string, optional): Nested card subtext

```json
"nestedcards": [
    {
        "title": "Nested Card Title",
        "description": "Nested Card Description",
    },
    {
        "title": "Nested Card Title",
        "description": "Nested Card Description",
    },
    {
        "title": "Nested Card Title",
        "description": "Nested Card Description",
    }
]
```

### Sections (Optional)

Sections are sub-sections within a card. Since this property is optional, if a card does not required sections make sure to omit it entirely from a card object.

**_Section Properties_**

- `title` (string, required): Section name
- `badge` (string, required): Sub-step number to display on card (e.g., "1.1", "1.2")
- `description` (string, optional): Section subtext

```json
"sections": [
    {
        "title": "Section Title",
        "badge": "1.1",
        "description": "Section description"
    },
    {
        "title": "Section Title",
        "badge": "1.2",
        "description": "Section description"
    }
]
```

### Arrows

Arrows define the flow between cards.

**_Arrow Properties_**

- `direction` (string, required): Direction: "down", "right", "left", or "up"
- `targetCardId` (string, required): ID of the target card

```json
"arrows": [
    {
        "direction": "up" | "down" | "right" | "left",
        "targetCardId": "card2"
    }
]
```

If on the last card, Arrows array can be left empty, but must be listed since it is a required property of a card like so:

```json
"arrows": []
```

## Complete Schema

```json
{
  "workflow": {
    "category": "CoreIgnite Setup" | "CoreFlow" | "<custom string>",
    "title": "Workflow Title",
    "description": "Workflow description",
    "icon": "piggy-bank" | "fragments" | "finance" | "money" | "book" | "application-mobile" | "user-profile"
  },
  "cards": [
    {
        "id": "card1",  // Card with only required properties
        "title": "Card 1 Title",
        "badge": "1",
        "description": "Card 1 description",
        "arrows": [
            {
                "direction": "up" | "down" | "right" | "left",
                "targetCardId": "card2"
            }
        ]
    },
    {
        "id": "card2",  // Card with all possible properties
        "title": "Card 2 Title",
        "badge": "2",
        "description": "Card 2 description",
        "nestedcards": [
            {
                "title": "Nested Card 1 Title",
                "description": "Nested Card 1 Description",
            },
            {
                "title": "Nested Card 2 Title",
                "description": "Nested Card 2 Description",
            },
            {
                "title": "Nested Card 3 Title",
                "description": "Nested Card 3 Description",
            }
        ],
        "sections": [
            {
                "title": "Section 1 Title",
                "badge": "2.1",
                "description": "Section 1 description"
            },
            {
                "title": "Section 2 Title",
                "badge": "2.2",
                "description": "Section 2 description"
            }
        ],
        "arrows": [
            {
                "direction": "up" | "down" | "right" | "left",
                "targetCardId": "card3"
            }
        ]
    },
    {...}
  ]
}
```

## Best Practices

1. **Unique IDs**: Ensure each card has a unique `id` value
2. **Valid Targets**: All `targetCardId` values in arrows must reference existing card IDs
3. **Consistent Badges**: Use consistent numbering schemes (e.g., "1", "2", "3" or "1.0", "2.0", "3.0")
4. **Complete Workflows**: Ensure workflows have a clear start and end (cards with no incoming/outgoing arrows)
5. **Descriptive Content**: Provide meaningful titles and descriptions for better understanding

## JSON Syntax Notes
- Use double quotes `"` for all keys and string values.
- Arrays are enclosed in square brackets `[ ... ]`, with each item separated by a comma.
- Objects are enclosed in curly braces `{ ... }`.
- Keys and string values must be in double quotes; numbers do not need quotes.
- Do not use trailing commas after the last item in an array or object.
- Empty arrays should be written as `[]`, and empty objects as `{}`.
- Indent nested structures consistently, typically with two spaces.