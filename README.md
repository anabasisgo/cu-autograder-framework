# Clemson University Autograder Framework 

Python-based generic output checking autograder framework for use with the 
[Gradescope](https://gradescope.com) autograder.

Originally based on the 
[General output-checking autograder example](https://gradescope-autograders.readthedocs.io/en/latest/diff_general/)
given by Gradescope.

# Usage
1. Download the latest release from the [releases page](https://github.com/Elan456/cu-autograder-framework/releases) 
or clone the repository.
2. Place testing drivers in the `tests/drivers` directory.
3. Place any input files that the student's code should read in the `tests/io_files` directory.
4. Put unit tests in the `tests/test.py` file which can use the drivers and io_files.
5. Zip up the autograder: `sh zipper.sh` (The name can be changed in the `zipper.sh` file)
6. Upload the autograder to Gradescope.
7. Upload your sample code to Gradescope and see if everything works as expected.
## Files (and what they do)

Even though many files are mentioned, the two things that must be changed are the 
`test.py` folder and adding drivers to the `drivers` directory. 

* `setup.sh` - Ran when the autograder docker image is built. This is where
  you should install any dependencies that your autograder needs and setup 
  users. You could compile code here, but it's easier to handle errors and 
  display them nicely if you compile in the `test.py` file. 
* `run_autograder` - What Gradescope runs when the autograder is executed.
  This is typically used to start the Python test script. 
* `run_tests.py` - Finds all the unit tests within the `tests` directory and
  runs them. It then logs the results to a `results.json` 
  This shouldn't need to be changed. Typically, it just runs `test.py`
* `requirements.txt` - A list of Python packages that need to be installed
  for the autograder to run.  
* `zipper.sh` - A small script that zips up the autograder for upload to 
  Gradescope.
* `example_sample_code/samplecode.zip` - Sample code which should pass
all the example test cases given in this framework

### Tests directory


| Directory        | Use                      | Function                                                                         |
|------------------|--------------------------|----------------------------------------------------------------------------------|
| `tests/drivers`  | Store non-Python drivers | Copied into the `source` folder **without giving** read access to the "student" user      |
| `tests/io_files` | Store input files        | Copied into the `source` folder and **given** read access to the "student" user      |

* `test.py` - The Python script which compiles, runs, and observes drivers and
  the student's code. Examples are given in the file.
* `utils` - A Python package that contains our custom utilities for the autograder.
  This is where to define functions that are used multiple times in the autograder.

# Security 
This framework wasn't made with security as the main focus; however, precautions 
were taken to prevent simple attacks such as students attempting to Tar the entire root
and capture hidden test cases.

Many of the utility functions in `utils` make use of the "student" user who has 
limited permissions. As long as student's code is not run as root, everything
within the tests directory should be safe, but more testing is still needed.

Be careful running student's makefiles without the "student" user 
as they can be used to run arbitrary commands on the system.

Security becomes important when there are more students because it becomes
more difficult to find malicious submissions. 
[Gradescope's best practices](https://gradescope-autograders.readthedocs.io/en/latest/best_practices/)
mentions a few things to keep in mind. 
For example, if you run the student's code as root while they have something 
like [this](https://www.reddit.com/r/csMajors/comments/rlkf55/if_your_school_uses_gradescope_autograder_hidden/)
set up, they'll find all your hidden test cases. 

# Contributing

## Making Changes
1. Clone the repository
2. Create a new branch
3. Make your changes
4. Commit your changes and modify files to satisfy the pre-commit hooks
5. Create a pull request
6. Wait for approval
7. Merge your branch into main
8. Delete your branch
9. Celebrate!!

## Getting Help 
Feel free to email me at [ema8@clemson.edu](ema8@clemson.edu), or if you are a Clemson student,
message me on Teams to ask specific questions or set up a time to meet.

## Issues
If you find any issues, please report them on the issues page
of the repository. Please include as much information as possible
so that the issue can be reproduced and fixed.

## Feature Requests
If you have any feature requests, also use the issues
page of the repository. Please include as much information as possible
so that the feature can be implemented as requested.

