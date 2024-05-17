# PDChecker

An automated security analysis framework for identifying the three vulnerabilities in chaincode applying PDC. This tool is an extension of [revive](https://github.com/mgechev/revive), and is one of the main outcomes of the paper "Understanding and Detecting Privacy Leakage Vulnerabilities in Hyperledger Fabric Chaincodes".

## Installation

1. Go installstion. (Recommend Go 1.21.1)
2. build from the source code

```bash
git clone git@github.com:pdchecker/PDChecker.git
cd PDChecker
make install
make build
```

## Usage

```bash
revive -h
```

- defaults.toml: default config for revive.
- chaincode.toml: config for go chaincodes.
- chaincodePrivacy.toml: config for privacy leakage vulnerability detection in go chaincodes.

### Command Line Flags

PDChecker accepts the following command line parameters:

- `-config [PATH]` - path to config file in TOML format.
- `-exclude [PATTERN]` - pattern for files/directories/packages to be excluded for linting. You can specify the files you want to exclude for linting either as package name, list them as individual files, directories, or any combination of the three.
- `-formatter [NAME]` - formatter to be used for the output. The currently available formatters are:

  - `default` - will output the failures the same way that `golint` does.
  - `json` - outputs the failures in JSON format.
  - `ndjson` - outputs the failures as stream in newline delimited JSON (NDJSON) format.
  - `friendly` - outputs the failures when found. Shows summary of all the failures.
  - `stylish` - formats the failures in a table. Keep in mind that it doesn't stream the output so it might be perceived as slower compared to others.
  - `checkstyle` - outputs the failures in XML format compatible with that of Java's [Checkstyle](https://checkstyle.org/).
- `-max_open_files` -  maximum number of open files at the same time. Defaults to unlimited.
- `-set_exit_status` - set exit status to 1 if any issues are found, overwrites `errorCode` and `warningCode` in config.
- `-version` - get revive version.

### Configuration

PDChecker can be configured with a TOML file. Here's a sample configuration with explanation for the individual properties:

```toml
# When set to false, ignores files with "GENERATED" header, similar to golint
ignoreGeneratedHeader = true

# Sets the default severity to "warning"
severity = "warning"

# Sets the default failure confidence. This means that linting errors
# with less than 0.8 confidence will be ignored.
confidence = 0.8

# Sets the error code for failures with severity "error"
errorCode = 0

# Sets the error code for failures with severity "warning"
warningCode = 0

# Configuration of the `cyclomatic` rule. Here we specify that
# the rule should fail if it detects code with higher complexity than 10.
[rule.cyclomatic]
  arguments = [10]

# Sets the severity of the `package-comments` rule to "error".
[rule.package-comments]
  severity = "error"
```

By default PDChecker will enable only the linting rules that are named in the configuration file.