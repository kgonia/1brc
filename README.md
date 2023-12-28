# 1️⃣🐝🏎️ The One Billion Row Challenge

The One Billion Row Challenge (1BRC) is a fun exploration of how far modern Java can be pushed for aggregating one billion rows from a text file.

The text file contains temperature values for a range of weather stations.
Each row is one measurement in the format `<string: station id>;<double: measurement>`.
The following shows ten rows as an example:

```
hamburg;11.0
hammerfest;-12.7
auckland;5.3
lima;6.6
concordia;-42.5
hammerfest;-6.7
hamburg;9.0
hammerfest;-11.9
hamburg;-0.7
concordia;-48.5
```

The task is to write a Java program which reads the file, calculates the average temperature value per weather station, and emits the result on stdout like this:

```
{auckland=5.3, concordia=-45.5, hamburg=6.4, hammerfest=-10.4, lima=6.6}
```

## Results

| # | Result (sec) | Implementation     | Submitter     |
|---|--------------|--------------------|---------------|
| 1.|          tbd.| [CalculateAverage.java](https://github.com/gunnarmorling/onebrc/blob/main/src/main/java/dev/morling/onebrc/CalculateAverage.java) (baseline)| Gunnar Morling|

See [below](#entering-the-challenge) for instructions how to enter the challenge with your own implementation.

## Prerequisites

[Java 21](https://openjdk.org/projects/jdk/21/) must be installed on your system.

## Running the Challenge

This repository contains two programs:

* `dev.morling.onebrc.CreateMeasurements` (invoked via _create\_measurements.sh_): Creates the file _measurements.txt_ in the root directory of this project with a configurable number of random measurement values
* `dev.morling.onebrc.CalculateAverage` (invoked via _calculate\_average.sh_): Calculates the average values for the file _measurements.txt_

Execute the following steps to run the challenge:

1. Build the project using Apache Maven:

    ```
    ./mvnw clean verify
    ```

2. Create the measurements file with 1B rows (just once):

    ```
    ./create_measurements.sh 1000000000
    ```

    This will take a few minutes.
    **Attention:** the generated file has a size of approx. **12 GB**, so make sure to have enough diskspace.

3. Calculate the average measurement values:

    ```
    ./calculate_average.sh
    ```

    The provided naive example implementation uses the Java streams API for processing the file and completes the task in ~3 min on an Apple Mac Mini M1.
    It serves as the base line for comparing your own implementation.

4. Optimize the heck out of it:

    Adjust the `CalculateAverage` program to speed it up, in any way you see fit (just sticking to a few rules described below).
    Options include parallelizing the computation, using the (incubating) Vector API, memory-mapping different sections of the file concurrently, using AppCDS, GraalVM, CRaC, etc. for speeding up the application start-up, choosing and tuning the garbage collector, and much more. 

The following rules and limits apply:

* Any Java distribution provided by [SDKMan](https://sdkman.io/jdks) as well as early access builds available on openjdk.net may be used (including EA builds for OpenJDK projects like Valhalla).
If you want to use a build not available via these channels, reach out to discuss whether it can be considered.
* No external library dependencies may be used
* Implementations must be provided as a single source file

## Entering the Challenge

To submit your own implementation to 1BRC, follow these steps:

* Create a fork of the [onebrc](https://github.com/gunnarmorling/onebrc/) GitHub repository.
* Create a copy of _CalculateAverage.java_, named _CalculateAverage\_<your_GH_user>.java_, e.g. _CalculateAverage\_doloreswilson.java_.
* Make that implementation fast. Really fast.
* Create a copy of _calculate_average.sh_, named _calculate\_average\_<your_GH_user>.sh_, e.g. _calculate\_average\_doloreswilson.sh_.
* Adjust that script so that it references your implementation class name. If needed, provide any JVM arguments via the `JAVA_OPTS` variable in that script.
* (Optional) If you'd like to use native binaries (GraalVM), adjust the _pom.xml_ file so that it builds that binary.
* Create a pull request against the upstream repository, clearly stating
  * The name of your implementation class.
  * The JDK build to use (of not specified, the latest OpenJDK 21 upstream build will be used).
* I will run the program and determine its performance as described in the next section, and enter the result to the scoreboard.

There is no pre-defined end date or maximum number of results for this challenge at this point,
but I may do a cut-off at some point.
To keep overhead low, please refrain from submitting entries if they wouldn't make it to the top half of the score board.

## Evaluating Results

Results are determined by running the program on a [Hetzner Cloud CCX33 instance](https://www.hetzner.com/cloud) (8 dedicated vCPU, 32 GB RAM).
The `time` program is used for measuring execution times, i.e. end-to-end times are measured.
Each contender will be run five times in a row.
The slowest and the fastest runs are discarded.
The mean value of the remaining three runs is the result for that contender and will be added to the results table above.

If you'd like to spin up your own box for testing on Hetzner Cloud, you may find these [set-up scripts](https://github.com/gunnarmorling/cloud-boxes/) (based on Terraform and Ansible) useful.
Note this will incur cost you are responsible for, I am not going to pay your cloud bill :)

## Price

If you enter this challenge, you may learn something new, get to inspire others, and see your name listed in the scoreboard above.

## FAQ

_Q: Can I use Kotlin or other JVM languages other than Java?_\
A: No, this challenge is focussed on Java only. Feel free to inofficially share implementations significantly outperforming any listed results, though.

_Q: Can I use non-JVM languages and/or tools?_\
A: No, this challenge is focussed on Java only. Feel free to inofficially share interesting implementations and results though. For instance it would be interesting to see how DuckDB fares with this task.

_Q: Why_ 1️⃣🐝🏎️ _?_\
A: It's the abbreviation of the project name: **One** **B**illion **R**ow **C**hallenge.

## License

This code base is available under the Apache License, version 2.