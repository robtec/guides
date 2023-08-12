

```
name: Deploy Code

on:
  workflow_dispatch:
  push:
    branches: [ "main" ]
    paths-ignore:
      - '.github/workflows'
      - 'README.md'

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3
    - name: Deploy
      run: ls -la
```
