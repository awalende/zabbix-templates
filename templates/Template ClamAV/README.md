# Template ClamAV

## Introduction

This is a template for the Anti-Virus-Software [ClamAV](https://clamav.net).
It gathers the output of `clamscan` and analyzes it. The follwing fields are read by Zabbix:

- Entire `clamscan`-output
- Date of last scan
- Total scanned size
- Scan time
- Number of infected files
- Name of found virus if any

## Installation

### Install ClamAV

ClamAV is usually available via your default package manager.

For Ubuntu you can use:

```
sudo apt-get install clamav clamav-freshclam 
```

### Setup Template

This template uses the following Macros:

| **Macro**        | **Description**                                                                                | **Default**                   |   |   |
|------------------|------------------------------------------------------------------------------------------------|-------------------------------|---|---|
| {$CLAM.LOG.PATH} | Path to the clamscan output file                                                               | /var/log/clamav/last_scan.log |   |   |
| {$CLAM.TTL.SCAN} | Trigger an alert after n-seconds when a scan has not been done recently. 72 hours are default. | 259200                        |   |   |

Per default, `clamscan` is not running automaticaly in a defined interval.
You may use a cronjob, which pipes the output to `/var/log/clamav/last_scan.log`. For example:

`0 18 * * * clamscan --recursive --infected --exclude-dir='^/dev|^/proc|^/sys' / > /var/log/clamav/last_scan.log`

Make sure that `--infected` is set, so that `clamscan` only outputs files which have been infected.

## Issues

- Zabbix is only reporting the first infected file. Other infected files in the output are discarded.
- Output `clamscan` to a real log-file, containing the history of all scans and not only the last one.

## Coverage

This Template is tested on:

- Zabbix 6.0