# CoreIgnite Workflow Definitions

This repository contains workflow definitions for CoreIgnite Docs website. The workflows can be defined in either JSON or YAML formats and describe step-by-step processes for various operations.

## Project Structure

```
Corelgnite_test/
├── JSON/              # Workflow definitions in JSON format
├── YAML/              # Workflow definitions in YAML format
└── README.md          # This file
```

## Workflow Object Structure

Each workflow file contains a `workflow` object with the following properties:

- **category** (string): The category of the workflow.
- **title** (string): The name of the workflow
- **description** (string): A detailed description of what the workflow does
- **icon** (string): An icon identifier for the workflow.


## Cards Object Structure

Workflows are composed of **cards**, which represent individual steps or stages in the process. Each card contains:

- **id** (string): Unique identifier for the card
- **title** (string): The name of the step
- **badge** (string): Step number displayed on card
- **description** (string): Detailed description of the step
- **nestedcards** (array, optional): Nested cards within a card
- **sections** (array, optional): Sub-sections within a card
- **arrows** (array): Navigation connections to other cards

## Supported Formats

This project supports two formats for workflow definitions:

- **JSON**: See [JSON/README.md](JSON/README.md) for detailed documentation
- **YAML**: See [YAML/README.md](YAML/README.md) for detailed documentation

## Getting Started

1. Choose your preferred format (JSON or YAML)
2. Review the examples in the respective folder
3. Follow the schema documentation in the folder-specific README files
4. Create your workflow definition following the established structure

## Documentation

- [JSON Workflow Format](JSON/README.md) - Complete guide for JSON workflow definitions
- [YAML Workflow Format](YAML/README.md) - Complete guide for YAML workflow definitions

