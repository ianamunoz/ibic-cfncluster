# IBIC CfnCluster

There are two programs in this project, written in Python and R respectively, designed to come up with the cheapest way for you to run neuroimaging  projects on Amazon Web Services' Elastic Compute Cloud ("in the cloud").

Both do the same thing, but the Python version uses the boto interface, and the R version uses the aws command line interface. You can use whichever you prefer and whichever works on your system.

Each takes (at minimum) two arguments and a switch: 

 + The length of time (in hours) you expect your job to take on the appropriate instance class (m4/c4 vs. g2).
 + The number of jobs you have).
 + Whether to use GPU instances or not.

Both programs rely on the same set of flags and are called identically.

Both programs use the environment variable `IBICCFNCLUSTERHOME` to locate a file with the number of VCPUs in each instance type (`instance_concurrency`). `IBICCFNCLUSTERHOME` should be set to the name of the directory in which this project is installed (e.g., `~/ibic-cfncluster`, if you have cloned the repository to your home directory). The `instance_concurrency` file is located by default in `$IBICCFNCLUSTERHOME/lib/`.

## Setting `IBICCFNCLUSTERHOME`

To run either the Python or the R script, the environment variable `IBICCFNCLUSTERHOME` needs to be set. 

You can set it at runtime with the command below, but this only works in that terminal for as long as you leave it open. Assuming you are running `bash`:

    export IBICCFNCLUSTERHOME=~/ibic-cfncluster

To have `IBICCFNCLUSTERHOME` persist across sessions, add the export line to your `.bashrc` file (or another file like `.bash_profile`.) Any new session will have `IBICCFNCLUSTERHOME` set.

To set it for the current session, source your `.bashrc`:

    source ~/.bashrc

## Python
(Trevor K. McAllister-Day)

**You will need to install the python package `boto3` if you do not have it.**
See `https://github.com/boto/boto3`.

Basic usage:

    ibic-get-spot-estimate [--gpu] --hours HOURS --num NUM

### Example Usage

    $ python/ibic-get-spot-estimate --hours 100 --num 9 

    Here are the results:
    Minimum cost per instance will be EC2 instance-type c4.large in region ap-south-1b at price $0.0167/hr.
    
    Minimum total cost estimate is $8.35 (5 instances).

## R

Usage:

    ibic-get-spot-estimate  [--gpu] --hours HOURS --num NUM

The R version was originally written to download information about instance configuration from the web. However, this is long and time consuming (about 20 minutes, depending on how many configurations and regions you query). We changed it to use the same configuration file as the python version.

+ `-d/--download`   Forces R to download data from the internet.

### Example Usage

    $ R/ibic-get-spot-estimate --hours 100 --num 9 

    Here are the results:
    Minimum cost per instance will be EC2 instance-type c4.large at price $ 0.01643459 /hr
    
    Minimum total cost estimate is $ 8.217297 ( 5 instances)

## Common Flags

Both short and long flags are available for both programs.

### Required Arguments

 + `-H/--hours HOURS`      Number of hours you expect your job to take (on average) on the appropriate instance. Capital "H" to avoid conflict with default `-h` help flag.
 + `-n/--num NUM`     How many jobs (brains) you have to execute.

### Optional flags

 + `-g/--gpu`       Get cost estimates for GPU-enabled instances.
 + `-t/--total`     Display the total cost for the cheapest configuration only.

### Other flags

 + `-v/--verbose`   Be more verbose.
 + `-h/--help`      Display respective help menus.

