# Repro of Cloud Build issue: global env variables do not support substitutions

### Problem:
When environment variables are specified globally (in the "options" section), Cloud Build fails to interpret substitutions.
* built-in substitutions fail by passing through as the variable name
* custom substitutions cause the entire build to fail

## To reproduce:
**Expected output for all commands (not necessarily in order):**
```
VAR_1_BUILTIN=gcb-sandbox
VAR_2_LITERAL=hello
VAR_3_CUSTOM=world
```

### ISSUE 1: Built-in substitutions fail as global env variables
```
gcloud builds submit --no-source --config=global-vars.cloudbuild.yaml
```
Actual output:
```
VAR_1_BUILTIN=$PROJECT_ID
VAR_2_LITERAL=hello 
```
*(VAR_3 is omitted here because of ISSUE 2...)*

### ISSUE 2: Custom substitutions fail as global env variables
```
gcloud builds submit --no-source --config=global-vars-custom.cloudbuild.yaml`
```
Actual output:
```
(gcloud.builds.submit) INVALID_ARGUMENT: invalid build: key "_CUSTOM_VAR" in the substitution data is not matched in the template
```

### (Step Env Variables work correctly)
```
gcloud builds submit --no-source --config=step-vars.cloudbuild.yaml
```
...actual outut is correct