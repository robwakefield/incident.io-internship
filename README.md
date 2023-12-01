# Running the Code
To run the code, ensure you have python (v3.10+) installed and run the command:

`./render-schedule [--schedule, --overrides, --until, --from, --testfile]`.

Where:
- `--schedule` JSON file containing a definition of a schedule
- `--overrides` JSON file containing an array of overrides
- `--from` the time from which to start listing entries
- `--until` the time until which to listing entries
- `--testfile` JSON file containing a schedule and override combination to test (see test/ for examples)

There are no extra python libraries required to be installed.

# Code Structure / Implementation

The command line arguments are first parsed along with the input files. This gives us a clean JSON structure to perform operations on. The first thing that the code does is calculate the schedule as a list of shifts that fall closely around the from and until dates. We then apply the overrides on top of this schedule by peforming operations on a case-wise basis in a merge sort style by comparing the next shift and override that is not yet in the final (overriden) schedule. After this we have a correctly overwritten list of shifts that we need to truncate in order to fit the from/until constraint. Finally this is printed to the output.

## Testing

In order to help when adding new features and to ensure that the script is working as expeced, a test file containing the JSON attributes 'schedule', 'overrides', 'from', 'until' and 'expected' can be passed using the flag `--testfile`. The necessary atrributes are passed to the calc_shifts function and the output is compared with the the expected ouput defined in the JSON test file.

This was useful when implementing the different relations between shifts and overrides as there were many different ways that the overrides could interact with a schedule and using test cases for each permutation meant that it was easy to see if the implementation was correct.

A single test can be run as `render-schedule --testfile=test/truncate.json`

A helper bash script is provided that will run all of the test cases in the test/ directory `./runtests`

# Future Product Features

As it stands, the program outputting a JSON list is not very useful. I would want to implement a GUI to display the schedule in a clear way. This would probably be best done using a web app such as a React app as it can be quickly and easily set up. The JSON output would make it easy to parse the schedule and plot it on a timeline. It would be a good idea to make this available to each user in the schedule so they could check when they are next scheduled to work. There could also be a way to add an override in the web app and update the schedule to reflect this.

Another feature that would be useful would be to implement some logic that automatically notified the correct person (based on the schedule) when there is a problem that they need to address. A database of on-call staff's names/usernames linked to their emails could be used as a lookup table that forwarded the problem notification to their inbox. A priority system implemented at the problem notification level could make use of phone calls if the priority of the problem is high enough. Either way we would use the schedule as a way to find out who was on-call at the time.

