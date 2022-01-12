# epoch2iso

Unix Epoch times are not human readable. This script can be used in a pipeline to convert any likely epoch numbers to the ISO8601 date and time format.

There is no consideration for the meaning of the numbers, so any number in a sensible range will be converted. This makes it handy for debugging and examining logs, but not suitable for use in a data loading pipeline.

## Requires

Perl

## Usage

usage: ./epoch2iso

Converts any epoch timestamps found in STDIN to ISO8601 times. Only times within a range of n years from now are converted. Default of 10 years.

Milliseconds are used if a sensible conversion can be made with milliseconds.

Warning: Some numbers which are not timestamps might be converted accidentally, use -y to limit window range if needed.

       -h    Print help.
       -q    Quote output with "
       -q='  Quote output with '
       -u    Print a UTC time'
       -y    Only convert times within 1 year from now.
       -y=3  Only convert times within 3 years from now.

Examples run in 2019 in London (+1 timezone):

         echo 1434567890 | ./epoch2iso -y20 -u
         2015-06-17T19:04:50Z

         echo 1434567890 | ./epoch2iso -y20
         2015-06-17T20:04:50+01

         echo '{"my_doc_in": "json", "made-at": 1434567890}' | ./epoch2iso -q
         {"my_doc_in": "json", "made-at": "2015-06-17T20:04:50+01"}


Note that the default is to display the reference date in the local timezone in effect at time of the date being converted, not the current time. 

e.g. In London the following is observed even if run in winter as the time converted is using summer time. 

```
echo 1620604809 | ./epoch2iso 
2021-05-10T01:00:09+01
```
