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
$ export AWS_PROFILE='<YOUR AWS PROFILE>' # ~/.aws/credentials
$ export AWS_REGION='us-east-1'
$ export LW_ACCOUNT='<YOUR LACEWORK ACCOUNT NAME>'
$ export LW_API_KEY='<YOUR LACEWORK API KEY>'
$ export LW_API_SECRET='<YOUR LACEWORK API SECRET>'
```

## Build
```bash
$ packer build ubuntu1804-base.json
/src/up-and-running-packer(master) » packer build ubuntu1804-base.json
amazon-ebs: output will be in this color.

==> amazon-ebs: Prevalidating any provided VPC information
==> amazon-ebs: Prevalidating AMI Name: ubuntu1804-base-lacework-20200923045147
    amazon-ebs: Found Image ID: ami-06b33ea0e4b6334bc
==> amazon-ebs: Creating temporary keypair: packer_5f6ad464-6549-db4b-017c-7ff5fe50bc24
==> amazon-ebs: Creating temporary security group for this instance: packer_5f6ad468-ea04-2eb3-d38c-5008d8dd0c8a
==> amazon-ebs: Authorizing access to port 22 from [0.0.0.0/0] in the temporary security groups...
==> amazon-ebs: Launching a source AWS instance...
==> amazon-ebs: Adding tags to source instance
    amazon-ebs: Adding tag: "Name": "Packer Builder"
    amazon-ebs: Instance ID: i-06ecb497ce8b599a1
==> amazon-ebs: Waiting for instance (i-06ecb497ce8b599a1) to become ready...
==> amazon-ebs: Using ssh communicator to connect: 34.204.81.129
==> amazon-ebs: Waiting for SSH to become available...
==> amazon-ebs: Connected to SSH!
==> amazon-ebs: Provisioning with shell script: /var/folders/kw/18_3nq_j6gg_ky7d5zdfh_yh0000gn/T/packer-shell952488738
    amazon-ebs:   % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
    amazon-ebs:                                  Dload  Upload   Total   Spent    Left  Speed
    amazon-ebs: 100  5137  100  5137    0     0  84213      0 --:--:-- --:--:-- --:--:-- 84213
    amazon-ebs: --> install: Installing the Lacework CLI
    amazon-ebs: --> install: Downloading via wget: https://github.com/lacework/go-sdk/releases/latest/download/lacework-cli-linux-amd64.tar.gz
    amazon-ebs: --> install: Downloading via wget: https://github.com/lacework/go-sdk/releases/latest/download/lacework-cli-linux-amd64.tar.gz.sha256sum
    amazon-ebs: renamed '/var/tmp/tmp.MDa0Rowxlz/lacework-cli-latest.tar.gz' -> 'lacework-cli-linux-amd64.tar.gz'
    amazon-ebs: renamed '/var/tmp/tmp.MDa0Rowxlz/lacework-cli-latest.tar.gz.sha256sum' -> 'lacework-cli-linux-amd64.tar.gz.sha256sum'
    amazon-ebs: --> install: Verifying the shasum digest matches the downloaded archive
    amazon-ebs: lacework-cli-linux-amd64.tar.gz: OK
    amazon-ebs: --> install: Extracting lacework-cli-linux-amd64.tar.gz
    amazon-ebs: --> install: Installing Lacework CLI into /usr/local/bin
    amazon-ebs: 'lacework-cli-linux-amd64/lacework' -> '/usr/local/bin/lacework'
    amazon-ebs: info: No menu item 'Verifying installed Lacework CLI version' in node '(dir)Top'
    amazon-ebs: lacework v0.2.3 (sha:fed5cf0ca72e0b3aaf0673bd3eb57eb85b42eef1) (time:20200922222426)
--> install: The Lacework CLI has been successfully installed.
    amazon-ebs:           VULNERABILITIES
    amazon-ebs: ------------------------------------
    amazon-ebs:      SEVERITY    COUNT   FIXABLE
    amazon-ebs:   -------------+-------+----------
    amazon-ebs:     Critical         0         0
    amazon-ebs:     High             5         5
    amazon-ebs:     Medium          75        71
    amazon-ebs:     Low             57        31
    amazon-ebs:     Negligible       6         2
    amazon-ebs:
    amazon-ebs:         CVE        |  SEVERITY  | SCORE |     PACKAGE     |         VERSION          |        FIX VERSION
    amazon-ebs: -------------------+------------+-------+-----------------+--------------------------+-----------------------------
    amazon-ebs:   CVE-2018-16864   | High       | 7.8   | systemd         | 237-3ubuntu10.42         | 0:237-3ubuntu10.11
    amazon-ebs:   CVE-2018-11235   | High       | 7.8   | git             | 1:2.17.1-1ubuntu0.7      | 1:2.17.1-1ubuntu0.1
    amazon-ebs:   CVE-2018-16865   | High       | 7.8   | systemd         | 237-3ubuntu10.42         | 0:237-3ubuntu10.11
    amazon-ebs:   CVE-2016-6313    | High       | 5.3   | libgcrypt20     | 1.8.1-4ubuntu1.2         | 0:1.7.2-2ubuntu1
    amazon-ebs:   CVE-2019-7304    | High       | 9.8   | snapd           | 2.45.1+18.04.2           | 0:2.34.2+18.04.1
    amazon-ebs:   CVE-2018-15686   | Medium     | 9.8   | systemd         | 237-3ubuntu10.42         | 0:237-3ubuntu10.6
    amazon-ebs:   CVE-2018-19486   | Medium     | 9.8   | git             | 1:2.17.1-1ubuntu0.7      | 1:2.17.1-1ubuntu0.4
    amazon-ebs:   CVE-2019-1353    | Medium     | 9.8   | git             | 1:2.17.1-1ubuntu0.7      | 1:2.17.1-1ubuntu0.5
    amazon-ebs:   CVE-2018-16229   | Medium     | 7.5   | tcpdump         | 4.9.3-0ubuntu0.18.04.1   | 0:4.9.3-0ubuntu0.18.04.1
    amazon-ebs:   CVE-2018-14404   | Medium     | 7.5   | libxml2         | 2.9.4+dfsg1-6.1ubuntu1.3 | 0:2.9.4+dfsg1-6.1ubuntu1.2n
...
...
```