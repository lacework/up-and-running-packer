# Up and Running with Lacework and Packer
This repo contains reference code from the blog post Up and Running with Lacework and Packer,
and can be used to test the Lacework Host Vulnerability feature that scans a package manifest 
from a host. 


There are two Packer templates in the base directory that build a Amazon Machine Images for 
Ubuntu 18.04 and Red Hat Enterprise Linux 8. The Ubuntu 18.04 image intentionally does not patch
vulnerabilites at build time in order to display the results when vulnerabilities are found. The 
RHEL 8 image does patch for vulnerabilities and _should_ return results that no vulnerabilities 
were found.

## Prereqs
- Lacework Account - You will need a Lacework account to authenticate against the Lacework API
- Lacework API Key / Secret - A Lacework API Key is required to make this integration work
- Packer - HashiCorp Packer is required

## Setup
Before beginning export the following environment variables:

```bash
$ export AWS_REGION='us-east-1'
$ export AWS_PROFILE='<YOUR AWS PROFILE>' # ~/.aws/credentials
$ export LW_API_KEY='<YOUR LACEWORK API KEY>'
$ export LW_API_SECRET='<YOUR LACEWORK API SECRET>'
```

## Build
```bash
$ packer build ubuntu1804-base.json
$ packer build rhel8-base.json
```