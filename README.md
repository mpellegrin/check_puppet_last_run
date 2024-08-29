# check_puppet_last_run
A command to check the status of Puppet last_run_summary.yaml file, written in Python with the nagiosplugin library.

## Prerequisites

The following packages or libraries are required:
 - python3-nagiosplugin
 - python3-yaml

## Installation

 1. Install the required libraries

 2. Copy the file inside your plugins directory, for instance `/usr/lib/nagios/plugins_bin`

 3. On the Icinga client, create the CheckCommand configuration. Do not forget to adjust the summary file path depending on your installation.
```
object CheckCommand "check_puppet_last_run" {
  command = [ PluginDir + "/check_puppet_last_run" ]
  arguments = {
    "--filepath" = {
      value = "/var/cache/puppet/state/last_run_summary.yaml"
      description = "Path to the last_run_summary.yaml file"
      skip_key = true
      required = true
      order = -1
    }
  }
}
```

 4. On the Icinga server, create the service:
```
apply Service "puppet last run" {
  check_command = "check_puppet_last_run"
  command_endpoint = host.name
  assign where host.name
}
```

## Configuration

You can specify custom ranges for the following metrics:
 - time since last Puppet run
 - number of changes
 - number of reported failures

| Argument | Default | Description |
|---|---|---|
|  `-wlr`<br>`--warning-lastrun-delay` | `0:3600` | return warning if last_run delay is outside RANGE |
|  `-clr`<br>`--critical-lastrun-delay` | `0:14400` | return critical if last_run is outside RANGE |
|  `-wc`<br>`--warning-changes` |  | return warning if changes is outside RANGE |
|  `-cc`<br>`--critical-changes` |  | return critical if changes is outside RANGE |
|  `-wf`<br>`--warning-failures` |  | return warning if failures is outside RANGE |
|  `-cf`<br>`--critical-failures` | `0:0` | return critical if failures is outside RANGE |
