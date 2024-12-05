# Basic Github Workflow Setup for KaaS

```YAML
---
  name: 'Proof Runner'
  on:
    workflow_dispatch:
      inputs:
        branch_name:
          description: 'Branch Name of Specific Code to Test'
          required: true
        org:
          description: 'Organization Name'
          required: true
        vault:
          description: 'Vault Name (Connected Github Repository)'
          required: true
    pull_request:
      branches:
        - main

  # Stop in progress workflows on the same branch and same workflow to use latest committed code
  concurrency:
    group: ${{ github.workflow }}-${{ github.ref }}-${{ github.event.inputs.branch_name }}
    cancel-in-progress: true
  
  jobs:
    test-proofs:
      name: 'Test Proofs'
      runs-on: [ubuntu-latest]
      steps:
        - name: 'Check out code'
          uses: actions/checkout@v4
          with:
            fetch-depth: 0

        - name: "Install KaaS"
          uses: runtimeverification/install-kaas@v0.2.1

        # This mode will run proofs on Runtime Verification Servers using dedicated hardware built for proof execution.
        # A kontrol.toml and foundry.tom file are still requried to be present in the root of the test directory.
        # Github App installation of "KaaS Storage & Compute" is required to use this mode to support access to test source code.
        - name: 'Run KaaS in Remote Mode'
          shell: bash
          run: |
            kaas-cli run -m remote --watch -t ${{ secrets.KAAS_TOKEN }} --branch ${{ github.event.inputs.branch_name }}  -vs ${{ github.event.inputs.org }}/${{ github.event.inputs.vault }}

        # This is the default mode, and will run the proofs in a containerized environment. Depends on DOCKER being installed and will fail if it is not.
        # A kontrol.toml and foundry.tom file are still requried to be present in the root of the test directory.
        # Logs and output are streamed to stdout, and saved to file on completion. Any kcfg data produced is pulled back to the host at the test root (where kontrol.toml is defined).
        - name: 'Run KaaS in Container Mode'
          shell: bash
          run: |
            kaas-cli run -m container
        
        # This is an advanced mode, and expects the user understands their kontrol installation and setup. 
        # A kontrol.toml and foundry.tom file are still requried to be present in the root of the test directory.
        - name: 'run KaaS in Local Mode'
          shell: bash
          run: |
            kaas-cli run -m local
```
