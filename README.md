# Falcosecurity kernel-crawler
 
[![Latest](https://img.shields.io/github/v/release/falcosecurity/kernel-crawler?style=for-the-badge)](https://github.com/falcosecurity/kernel-crawler/releases/latest)
![Architectures](https://img.shields.io/badge/ARCHS-x86__64%7Caarch64-blueviolet?style=for-the-badge)
 
It is a tool used to crawl supported kernels by multiple distros, and generate a [driverkit](https://github.com/falcosecurity/driverkit)-like config json.  
Output json can be found, for each supported architecture, under [kernels](https://github.com/falcosecurity/kernel-crawler/tree/kernels) branch and on gh pages: https://falcosecurity.github.io/kernel-crawler/.  

A weekly [prow job](https://github.com/falcosecurity/test-infra/blob/master/config/jobs/update-kernels/update-kernels.yaml) will open a PR on this repo to update the json.  
As soon as the PR is merged and the json updated, another [prow job](https://github.com/falcosecurity/test-infra/blob/master/config/jobs/update-dbg/update-dbg.yaml) will create a PR on [test-infra](https://github.com/falcosecurity/test-infra) to generate the new Driverkit configs from the updated json.

Helper text and options:

Main:
```commandline
Usage: kernel-crawler [OPTIONS] COMMAND [ARGS]...

Options:
    --debug / --no-debug
    --help                Show this message and exit.

Commands:
    crawl
```

Crawl command:
```commandline
Usage: kernel-crawler crawl [OPTIONS]

Options:
    --distro [AmazonLinux|AmazonLinux2|AmazonLinux2022|CentOS|Debian|Fedora|Flatcar|Minikube|Oracle6|Oracle7|Oracle8|PhotonOS|Redhat|Ubuntu|*]
    --version TEXT
    --arch [x86_64|aarch64]
    --out_fmt [plain|json|driverkit]
    --image TEXT                    Option is required when distro is Redhat.
    --help                          Show this message and exit.
```

## Docker image

A docker image is provided for releases, by a circleCI job: `falcosecurity/kernel-crawler:latest`.
You can also build it yourself, by issuing:
```commandline
docker build -t falcosecurity/kernel_crawler -f docker/Dockerfile .
```
from project root.

## Install

To install the project, a simple `pip3 install .` from project root is enough.  

## Examples

* Crawl amazonlinux2 kernels, with no-formatted output:
```commandline
kernel-crawler crawl --distro=AmazonLinux2
```

* Crawl ubuntu kernels, with [driverkit](https://github.com/falcosecurity/driverkit) config-like output:
```commandline
kernel-crawler crawl --distro=Ubuntu --out_fmt=driverkit
```

* Crawl all supported distros kernels, with json output:
```commandline
kernel-crawler crawl --distro=* --out_fmt=json
```
| :exclamation: **Note**: Passing ```--image``` argument is supported with ```--distro=*``` |
|-------------------------------------------------------------------------------------------|

* Crawl Redhat kernels (specific to the container supplied), with no-formatted output:
```commandline
kernel-crawler crawl --distro=Redhat --image=redhat/ubi8:registered
```
