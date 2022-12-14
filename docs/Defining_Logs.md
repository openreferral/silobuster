### Log record.

These are the columns in our `log` table. They cover basic data identifying
down to the step that's generating a log. All specifics are stored in a JSON
blob in the `log_message` field.

```json
{
    "id": "uuid",
    "job_id": "string",
    "iteration_id": "string",
    "step_name": "clean|dedupe'",
    "contributor_name": "string",
    "log_message": "json"
}
``` 

### Notes on naming conventions.

`link_entity` is table name.
`link_id` is record Id.

These field names are borrowed from HSDS 3.0 and used to maintain system wide
consistency.

### log_message for Cleaning logs.

Cleaning log JSON identifies the table, record, and field this log is for. In
other words, cleaning logs are generated at the field (aka column) level. There
should be exactly one log per field. However, in some cases there will be
multiple messages for a single field, hence the "prompts" array.

Where possible, include a suggestion, or "prompt", that guides end users in
curating or selecting a correct value quickly.


```json
{
    "id": "string",
    "link_entity": "string",
    "link_id": "string",
    "link_column": "string",
    "prompts": [
      {
        "description": "string",
        "suggested_value": "string"
      }
    ]
}
```

If we discover a use case for relating a log to other logs or fields, we can also add an array of log or field IDs to this schema. In the interest of not over engineering this, we are waiting for positive feedback for that use case before implementing.

### log_message for Dedupe logs.

Dedupe records generated from Dedupe.io include a "cluster ID" that relates two
or more records that are likely duplicates. We are still discovering what other
data should be logged, so the following represents the _minimum_ data required.
For now, just dump the remainder of the JSON log that's generated by dedupe.

```json
{
    "cluster_id": "uuid"
}
```