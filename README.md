# Validate JSON Action

Validates JSON files or strings, providing clear error messages on malformed input. Critical for preventing runtime failures from invalid configuration.

## Usage

```yaml
- name: Validate options file
  uses: skyhook-io/validate-json@v1
  with:
    json_file: .koala/build.targets.json

- name: Validate JSON string
  uses: skyhook-io/validate-json@v1
  with:
    json_string: ${{ inputs.options_json }}
```

## Inputs

| Name | Description | Required | Default |
|------|-------------|----------|---------|
| `json_string` | JSON string to validate | No | - |
| `json_file` | Path to JSON file to validate | No | - |
| `schema_file` | Optional JSON schema for validation* | No | - |

*Note: Currently validates that the schema file is valid JSON, but does not validate input conformance to the schema.

## Outputs

| Name | Description |
|------|-------------|
| `valid` | Whether the JSON is valid (true/false) |
| `error` | Error message if validation failed |
| `parsed` | The parsed JSON content (if valid) |

## Examples

### Validate configuration file
```yaml
- name: Validate build configuration
  uses: skyhook-io/validate-json@v1
  with:
    json_file: .koala/build.targets.json
  continue-on-error: false  # Fail workflow on invalid JSON
```

### Validate and use parsed output
```yaml
- name: Validate and parse config
  id: config
  uses: skyhook-io/validate-json@v1
  with:
    json_file: config.json

- name: Use parsed config
  run: |
    echo "Config: ${{ steps.config.outputs.parsed }}"
```