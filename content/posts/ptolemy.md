---
aliases:
  - /tech/2016/11/22/ptolemy
date: 2016-11-22
layout: post
title: "Ptolemy: An AWS DMS Table Mapping Generator"
---

This post introduces Ptolemy, a tool for creating AWS DMS mapping tables,
recently open sourced by Cloudreach. Ptolemy's source code can be found on
[GitHub](https://github.com/cloudreach/ptolemy).

## Motivation

Amazon Web Services provides the
[Database Migration Service](https://aws.amazon.com/documentation/dms/) (DMS)
tool for migrating data to, from, or between SQL databases. When running DMS,
users can supply a table mapping, which allows the user to specify what data is
sent from the source database to the target database.

Table mappings are written as JSON documents, which can grow to be long and
complex. Ptolemy allows the user to write terse YAML source files, which can be
compiled to valid JSON table mappings, in a similar way to compiling Markdown or
RST to HTML.

## Reducing Table Mapping Complexity

Ptolemy simplifies native mappings in the following ways:

- The `object-locator` key has been replaced with the pluralised
  `object-locators`; this allows a particular rule to be applied to multiple
  schema/tables/columns
- For a clearer syntax, human-friendly `YAML` is used instead of `JSON`

These simplifications allow users to write mapping tables faster, with fewer
mistakes.

## Example

The following Ptolemy source file:

```yaml
selection:
  include:
    - object-locators:
        schema-names:
          - Test
        table-names:
          - "TableA"
          - "TableB"
```

Is compiled to the following DMS mapping table:

```json
{
  "rules": [
    {
      "object-locator": {
        "schema-name": "Test",
        "table-name": "TableA"
      },
      "rule-action": "include",
      "rule-id": "1",
      "rule-name": "1",
      "rule-type": "selection"
    },
    {
      "object-locator": {
        "schema-name": "Test",
        "table-name": "TableB"
      },
      "rule-action": "include",
      "rule-id": "2",
      "rule-name": "2",
      "rule-type": "selection"
    }
  ]
}
```

More examples are available on the
[examples](https://github.com/cloudreach/ptolemy/tree/master/examples) page.

## Further Information

Ptolemy is fully unit and integration tested, with 100% code coverage, and has
been successfully used in production.

For more information, and instructions on how to install Ptolemy, visit the
project's [GitHub](https://github.com/cloudreach/ptolemy) page.
