# Amazon Inspector Vulnerability Scan Workflow

[![GitHub Super-Linter](https://github.com/build-failure/starter-workflows/actions/workflows/linter.yml/badge.svg)](https://github.com/super-linter/super-linter)
![CI](https://github.com/build-failure/starter-workflows/actions/workflows/ci.yml/badge.svg)

Contains reusable GitHub Actions workflow to generate container image SBOM using
[Amazon Inspector SBOM Generator](https://docs.aws.amazon.com/inspector/latest/user/sbom-generator.html)
and scan it using [Amazon Inspector Scan API].

Resulting [vulnerability report](https://docs.aws.amazon.com/inspector/latest/user/cicd-custom.html#heading:r4h:)
is validated against specified threshold.

## Inputs

### `docker-image-name`

Docker image name. Default `"test/dev"`.

### `docker-context`

Relative path to docker file. Default `"."`.

### `docker-file`

Dockerfile name. Default `"Dockerfile"`.

### `amazon-inspector-scan-assume-role`

Assume role to execute scan using
[Amazon Inspector scan API].

### `amazon-inspector-scan-region`

Region to execute scan using [Amazon Inspector scan API].
Default `"us-east-1"`.

### `amazon-inspector-scan-endpoint`

[Endpoint](https://docs.aws.amazon.com/inspector/latest/user/inspector_regions.html#inspector-scan-endpoints)
to execute scan using [Amazon Inspector scan API].
Default `"https://inspector-scan.us-east-1.amazonaws.com"`.

### `threshold`

Vulnerability threshold. Default `"critical"`.

## Example usage

```yaml
on:
  push:
  workflow_dispatch:

permissions:
  contents: read
  id-token: write

jobs:
  scan-image:
    uses: build-failure/amazon-inspector-vulnerability-scan/.github/workflows/amazon-inspector-image-scan.yml@v1
    with:
      docker-image-name: test/dev
      docker-context: .
      amazon-inspector-scan-assume-role: arn:aws:iam::<ACCOUNT_ID>:role/<ASSUME_ROLE_NAME>
      amazon-inspector-scan-region: us-east-1
      amazon-inspector-scan-endpoint: https://inspector-scan.us-east-1.amazonaws.com
      threshold: critical
```

[Amazon Inspector scan API]:https://docs.aws.amazon.com/inspector/v2/APIReference/API_Operations_Inspector_Scan.html
