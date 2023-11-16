# PMG

the official, lightweight, and fast package manager for delux written in C99.

## Installation
comes with delux linux for other distros install `gcc` and
- Clone the repository
- Run `make` then `sudo cp ./pmg /bin/` finally `cd / && cd etc && sudo mkdir pmg && cd pmg && sudo touch pkgls.txt` 
- Now just type `pmg install {package-name}`


## What can it do?

At the moment it can perform an efficient clean install of a package which is cached. And then uses the cache when a module is downloaded twice. [See here](#whats-missing) for features that are missing.

## Why is it fast?

- Efficient version resolution which minimizes the HTTP throughput by using `{registry}/{package}/{version}` instead of `{registry}/{package}` which has a significantly larger body size
- Use of [reqwest](https://docs.rs/reqwest/latest/reqwest/) to create a HTTP connection pool
- Parallel and asyncronous HTTP requests to the [PMG Host Downloading API](https://github.com/mrGoodwolf)
- Use of the `Accept: application/vnd.npm.install-v1+json; q=1.0, application/json; q=0.8, */*` header which results in smaller HTTP body sizes
- Duplicate avoidance by storing pre-installed versions in a HashMap for clean installs
- A global cache that symlinks point to, avoiding any file copies
- Package locks generated for each cached package, to avoid re-retrievel of the required dependencies

## What's missing?

These are the primary functioning features required for this to pass as a "NodeJS package manager". There are plenty more quality of life and utlility features that will be neccessary:

- Expiry times for the cached packages
- Creation and maintainence of a `package.json` in the working directory
- Creation and maintainence of a `package-lock.json` in the project directory 
- An `uninstall` command
- An `update` command
- There is also an off case where some packages contain an operator at the end of their version like this `< version@2.2.3 > 1.1.2` which is not tolerated by [semver](https://docs.rs/semver/latest/semver/)
- Use checksums to verify file downloads
- Proper error handling everywhere
