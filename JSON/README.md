# JSON Workflow Format

This directory contains workflow definitions in JSON format for CoreIgnite Docs website.

## Workflow Object Properties

| Property      | Type   | Required | Description                                                                                                                             |
| ------------- | ------ | -------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| `title`       | string | Yes      | Name of the workflow                                                                                                                    |
| `description` | string | Yes      | Detailed description of the workflow                                                                                                    |
| `category`    | string | Yes      | Category a workflow is placed under, this field can take one of the existing categories (listed below) or option to create new category |
| `icon`        | string | Yes      | Icon representative of the workflow, this field can only take one of the available options (listed below)                                |

### Workflow Object Schema

The root of each JSON file should contain a `workflow` object and a `cards` array. Optionally, you can define `sharedSideCards` for reusable side card groups:

```
{
  "workflow": {
    "category": "CoreIgnite Setup" | "CoreFlow" | "<custom string>",
    "title": "Workflow Title",
    "description": "Workflow description",
    "icon": "piggy-bank" | "fragments" | "finance" | "money" | "book" | "application-mobile" | "user-profile"
  },
  "sharedSideCards": { // Optional
    "groupName": [...]
  },
  "cards": [...]
}
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

```
"sharedSideCards": {
  "coreBankGroup": [
    {
      "id": "card31",
      "title": "Core Bank Plugin (Hogan)",
      "badge": "",
      "description": "Pellentesque sit amet nunc sodales...",
      "icon": "settings",
      "arrows": [
        {
          "direction": "down",
          "targetCardId": "card32"
        }
      ]
    }
  ]
}
```

## Card Object Properties

| Property         | Type     | Required | Description                                           |
| ---------------- | -------- | -------- | ----------------------------------------------------- |
| `id`             | `string` | **Yes**  | **Unique identifier for the card**                    |
| `title`          | `string` | **Yes**  | **The name of the step**                              |
| `badge`          | `string` | **Yes**  | **Step number to display on card (e.g., "1", "2.1")** |
| `description`    | `string` | **Yes**  | **Detailed description of the step**                  |
| `arrows`         | `array`  | **Yes**  | **Navigation arrows to other cards**                  |
| `button`         | `object` | No       | Button with label and URL                             |
| `codeSnippet`    | `object` | No       | Display snippet of code in a card                     |
| `icon`           | `string` | No       | Icon (24px, for side cards)                           |
| `list`           | `object` | No       | List component (ordered or unordered)                 |
| `nestedcards`    | `array`  | No       | Display nested cards within the card                  |
| `sections`       | `object` | No       | Highlight sections within a card                      |
| `sideCardRef`    | `string` | No       | Reference to shared side cards group                  |
| `sideCardSpanEnd`| `string` | No       | ID where shared cards span ends (auto-calculated)     |
| `tabs`           | `array`  | No       | Show multiple tabs of content                         |
| `tags`           | `array`  | No       | Tags with label and optional color                    |

### Card Object Schema

Each card in the `cards` array follows this structure:

```
"cards": [
    {
        "id": "card1",
        "title": "Card Title",
        "badge": "1",
        "description": "Card description",
        "arrows": [...],
        "button": {...}, // Optional
        "codeSnippet": {...}, // Optional
        "icon": "wallet", // Optional
        "list": {...}, // Optional
        "nestedcards": [...], // Optional
        "sections": {...}, // Optional
        "sideCardRef": "groupName", // Optional
        "sideCardSpanEnd": "card3", // Optional
        "tabs": [...], // Optional
        "tags": [...] // Optional
    }
    {...}
]
```

### Arrows

Arrows define the flow between cards.

**_Arrow Properties_**

- `direction` (string, required): Direction: "down", "right", "left", or "up"
- `targetCardId` (string, required): ID of the target card
- `rowIndex` (number, optional): For horizontal arrows (0=top, 1=middle, etc.)

```
"arrows": [
    {
        "direction": "up" | "down" | "right" | "left",
        "targetCardId": "card2",
        "rowIndex": 0 // Optional
    }
]
```

If on the last card, Arrows array can be left empty, but must be listed since it is a required property of a card like so:

```
"arrows": []
```

### Button (Optional)

Button allows you to add a clickable button to a card that links to an external URL.

Since this property is optional, if a card does not require a button make sure to omit it entirely from a card object.

**_Button Properties_**

- `label` (string, required): Text to display on the button
- `url` (string, required): URL to navigate to when button is clicked

```
"button": {
    "label": "View Documentation",
    "url": "https://example.com/docs"
}
```

### Code Snippet (Optional)

Code Snippet allows you to display some simple code in its own section with content displayed in a monospaced block. Note this is not meant for complex or long pieces of code, but just to make references to helpful details.

Since this property is optional, if a card does not require a code snippet make sure to omit it entirely from a card object.

**_Code Snippet Properties_**

- `code` (string, required): Code content
- `caption` (string, optional): Brief text underneath code to provide any comments/explanations

```
"codeSnippet": {
    "code": "<custom code>",
    "caption": "Code snippet caption"
}
```

### Icon (Optional)

Icon allows you to display a 24px icon on a card, typically used for side cards.

**_Icon Values_**

Available icon options: `"wallet"`, `"settings"`, `"database"`, `"book"`, `"piggy-bank"`, `"fragments"`, `"application-mobile"`

```
"icon": "wallet"
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

```
"list": {
    "type": "ordered",
    "nestedType": "alpha",
    "items": [
        {
            "text": "Item 1"
        },
        {
            "text": "Item 2",
            "nested": [
                {
                    "text": "Nested item 2.1"
                },
                {
                    "text": "Nested item 2.2"
                }
            ]
        }
    ]
}
```

### Nested Cards (Optional)

Nested cards are smaller cards within a card.

Since this property is optional, if a card does not require nested cards, make sure to omit it entirely from a card object.

**_Nested Card Properties_**

- `title` (string, required): Nested card name
- `subtext` (string, optional): Nested card subtext

```
"nestedcards": [
    {
        "title": "Nested Card Title",
        "subtext": "Nested card description"
    },
    {
        "title": "Another Nested Card"
    },
    {
        "title": "Another Nested Card"
    }
]
```

### Sections (Optional)

Sections are sub-sections within a card.

Since this property is optional, if a card does not require sections make sure to omit it entirely from a card object.

**_Sections Object Properties_**

- `direction` (string, required): Layout direction for sections. Must be either `"col"` or `"row"`
- `items` (array, required): Array of section objects

**_Section Item Properties_**

- `title` (string, required): Section name
- `badge` (string, required): Sub-step number to display on card (e.g., "1.1", "A")
- `description` (string, optional): Section subtext
- `button` (object, optional): Button object (same structure as card button)
- `codeSnippet` (object, optional): Code snippet object (same structure as card codeSnippet)
- `icon` (string, optional): Icon name (24px, for side cards)
- `list` (object, optional): List object (same structure as card list)
- `nestedcards` (array, optional): Array of nested cards (same structure as card nestedcards)
- `tabs` (array, optional): Array of tabs (same structure as card tabs)
- `tags` (array, optional): Tags array (same structure as card tags)

```
"sections": {
    "direction": "col",
    "items": [
        {
            "title": "Section Title",
            "badge": "1.1",
            "description": "Section description"
        },
        {
            "title": "Section Title",
            "badge": "1.2",
            "button": {
                "label": "Learn More",
                "url": "https://example.com"
            },
            "icon": "wallet",
            "tags": [
                {
                    "label": "Tag 1",
                    "color": "#FF5733"
                }
            ],
            "list": {
                "type": "ordered",
                "items": [
                    {
                        "text": "Item 1"
                    }
                ]
            }
        }
    ]
}
```

### Side Card Reference (Optional)

Side card reference allows a card to reference a shared side card group defined in `sharedSideCards`.

**_Side Card Reference Properties_**

- `sideCardRef` (string, optional): Name of the shared side card group to reference
- `sideCardSpanEnd` (string, optional): ID of the card where the shared cards span ends (usually auto-calculated)

```
"sideCardRef": "coreBankGroup",
"sideCardSpanEnd": "card3"
```

### Tabs (Optional)

Tabs allows you to show multiple tabs of content within a card. A viewer will be able to toggle between the different tabs. Make sure to format your tabs in the order you would like them to be displayed.

Since this property is optional, if a card does not require tabs make sure to omit it entirely from a card object.

**_Tab Item Properties_**

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

```
"tabs": [
    {
        "label": "Tab 1",
        "content": {
            "text": "Tab 1 content text"
        }
    },
    {
        "label": "Tab 2",
        "content": {
            "type": "code",
            "code": "console.log('Hello');",
            "caption": "Optional caption"
        }
    }
]
```

**If `type` is `"sections"`:**
- `sections` (object, required): Sections object with same structure as card sections

```
"tabs": [
    {
        "label": "Tab 3",
        "content": {
            "type": "sections",
            "sections": {
                "direction": "col",
                "items": [
                    {
                        "title": "Section Title",
                        "badge": "2.1",
                        "description": "Section description"
                    }
                ]
            }
        }
    }
]
```

### Tags (Optional)

Tags allow you to display labeled tags on a card or section, optionally with custom colors.

Since this property is optional, if a card does not require tags make sure to omit it entirely from a card object.

**_Tag Properties_**

- `label` (string, required): Tag label text
- `color` (string, optional): Custom hex color for the tag (e.g., "#FF5733")

```
"tags": [
    {
        "label": "Tag 1",
        "color": "#FF5733"
    },
    {
        "label": "Tag 2"
    }
]
```

## Complete Schema

```
{
  "workflow": {
    "category": "CoreIgnite Setup" | "CoreFlow" | "<custom string>",
    "title": "Workflow Title",
    "description": "Workflow description",
    "icon": "piggy-bank" | "fragments" | "finance" | "money" | "book" | "application-mobile" | "user-profile"
  },
  "sharedSideCards": {
    "groupName": [
      {
        "id": "unique-id",
        "title": "Card Title",
        "badge": "badge text",
        "description": "Card description",
        "icon": "icon-name",
        "arrows": [...]
      }
    ]
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
                "targetCardId": "card2",
                "rowIndex": 0 // Optional
            }
        ]
    },
    {
        "id": "card2",  // Card with all possible properties
        "title": "Card 2 Title",
        "badge": "2",
        "description": "Card 2 description",
        "arrows": [
            {
                "direction": "up" | "down" | "right" | "left",
                "targetCardId": "card3"
            }
        ],
        "button": {
            "label": "View Documentation",
            "url": "https://example.com/docs"
        },
        "codeSnippet": {
            "code": "code snippet",
            "caption": "Code snippet caption"
        },
        "icon": "wallet",
        "list": {
            "type": "ordered",
            "nestedType": "alpha",
            "items": [
                {
                    "text": "Item 1"
                },
                {
                    "text": "Item 2",
                    "nested": [
                        {
                            "text": "Nested item 2.1"
                        }
                    ]
                }
            ]
        },
        "nestedcards": [
            {
                "title": "Nested Card Title",
                "subtext": "Nested card description"
            },
            {
                "title": "Another Nested Card"
            }
        ],
        "sections": {
            "direction": "col",
            "items": [
                {
                    "title": "Section 1 Title",
                    "badge": "2.1",
                    "description": "Section 1 description"
                },
                {
                    "title": "Section 2 Title",
                    "badge": "2.2",
                    "button": {
                        "label": "Learn More",
                        "url": "https://example.com"
                    },
                    "icon": "wallet",
                    "tags": [
                        {
                            "label": "Tag 1",
                            "color": "#FF5733"
                        }
                    ],
                    "list": {
                        "type": "ordered",
                        "items": [
                            {
                                "text": "Item 1"
                            }
                        ]
                    }
                }
            ]
        },
        "sideCardRef": "groupName",
        "sideCardSpanEnd": "card3",
        "tabs": [
            {
                "label": "Tab 1",
                "content": {
                    "type": "text",
                    "text": "Tab 1 content"
                }
            },
            {
                "label": "Tab 2",
                "content": {
                    "type": "code",
                    "code": "console.log('code');",
                    "caption": "Optional caption"
                }
            },
            {
                "label": "Tab 3",
                "content": {
                    "type": "sections",
                    "sections": {
                        "direction": "col",
                        "items": [
                            {
                                "title": "Section Title",
                                "badge": "2.1",
                                "description": "Section description"
                            }
                        ]
                    }
                }
            }
        ],
        "tags": [
            {
                "label": "Tag 1",
                "color": "#FF5733"
            },
            {
                "label": "Tag 2"
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
6. **Alphabetical Ordering**: After required properties, organize optional properties alphabetically for consistency

## JSON Syntax Notes

- Use double quotes `"` for all keys and string values.
- Arrays are enclosed in square brackets `[ ... ]`, with each item separated by a comma.
- Objects are enclosed in curly braces `{ ... }`.
- Keys and string values must be in double quotes; numbers do not need quotes.
- Do not use trailing commas after the last item in an array or object.
- Empty arrays should be written as `[]`, and empty objects as `{}`.
- Indent nested structures consistently, typically with two spaces.
