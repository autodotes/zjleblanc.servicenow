# zjleblanc.servicenow.records

Idempotently insert or update ServiceNow table records.

## Requirements

- Ansible `>=2.15.0`
- Collection `servicenow.itsm` (`servicenow.itsm.api` and `servicenow.itsm.api_info`)
- Instance URL and credentials configured for `servicenow.itsm`. This role does not set them.

## Entry points

### main

Query each entry in `snow_records` and insert it when the exists query matches nothing. Patch the first matching row when a record already exists. Append each API response to `snow_records_out` under the table name.

`tasks/insert.yml` is an internal task file included by `main`. It is not a public entry point.

#### Options

| Name | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| snow_records | list of dict | no | `[]` | ServiceNow table records to insert or update. |
| snow_records.type | str | yes | | ServiceNow table name passed as the API resource. |
| snow_records.data | dict | yes | | Field values sent as the API request body. The loop label reads `data.name`, and the default exists query is `name=<data.name>`. |
| snow_records.exists_query | str | no | | `sysparm_query` used to find an existing row. When omitted, the role queries `name=<data.name>`. |
| snow_records_out | dict | no | `{}` | Records already written, keyed by table name. The role combines each API result into this dict. |

## Dependencies

Calls modules from `servicenow.itsm`. No role dependencies.

## Example Playbook

```yaml
- name: Insert ServiceNow records idempotently
  hosts: localhost
  gather_facts: false
  tasks:
    - name: Insert the OAuth entity
      ansible.builtin.include_role:
        name: zjleblanc.servicenow.records
      vars:
        snow_records:
          - type: oauth_entity
            data:
              name: Ansible Controller

    - name: Insert the OAuth entity scope
      ansible.builtin.include_role:
        name: zjleblanc.servicenow.records
      vars:
        snow_records:
          - type: oauth_entity_scope
            data:
              name: Ansible Controller Write
              oauth_entity: "{{ snow_records_out['oauth_entity'][0].sys_id }}"
              oauth_entity_scope: write
```

The second task reads `snow_records_out`, which the first task fills under the table name.

## License

GPL-3.0. See the collection `LICENSE`.

## Author

Zachary LeBlanc (@zleblanc)
