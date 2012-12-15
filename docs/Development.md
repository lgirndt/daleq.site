---
layout: doc
title: Development
---


The basic guidelines if you want to contribute to Daleq's development.

## Building Daleq from Source

Daleq uses Gradle as a build tool. Hence building Daleq is as simple as

	$ git clone https://github.com/brands4friends/daleq.git
	$ cd daleq
	$ ./gradlew build
	
## Code Quality

We use Gradle's plugins like Checkstyle, PMD and Findbugs  to ensure some level of code quality. The build task will fail if one of their rules is violated. If a build broke due to failing tests or violated coding rules, you can use the ```issues``` task to get a brief summary of all occured problems:

	$ ./gradlew issues

## Code Coverage

We've integrated Clover Code Coverage generation into our Gradle configuration. (and this is no fun as of Gradle 1.2). If you want to run it, you need a Clover license file. As a matter of fact, currently we just support the older Clover version 2.6.7. 

Anyways, running the build to generate a code coverage report is done by:

	$ ./gradlew -PwithClover -Pclover.license=PATH_TO_YOUR_LICENSE_FILE test
	
## Integration Tests

Running the whole project or just the subproject ```intregration``` will run a certain set of tests against an actual database. Currently Daleq supports a few InMemory DBs and some which require a separate database instance. 

Nonetheless a task execution will use a distinct DB. Which DB is taken is controlled with a gradle property ```integration.db```. Some DBs require further properties. If no property is provided, HSQLDB is taken. 

Currently the following databases are supported

| DB | integration.db  | Further properties |
|---:|----------------:|:-------------------|
| HSQLDB | HSQLDB | - |
| H2 | H2 | - |
| Mysql | Mysql | integration.mysql.url=URL, integration.mysql.user=test, integration.mysql.password=test |

Gradle properties can be provided on the command line via the ```-P``` switch, like 

	$ ./gradlew build -Pintegration.db=H2

or with a separate ```gradle.properties``` file.

Have a look at the travis-ci configuration for further insights.

## Releasing a Version

There are a few things to do and consider when releasing a version X.Y.0. 

Just do

1. Update the version from ```X.Y.0-SNAPSHOT``` to ```X.Y.0``` in ```build.gradle```, commit and push.
1. Make sure, the ci server succeeds with this build.
1. Tag the release ```git tag -a release-X.Y.0``` and push the tag to github ```git push --tags```.
1. Upload the artifacts to Maven's Central Repository.
1. Change references to the project version in the README and the wiki.
1. Update the master to the next SNAPSHOT release: Change the version to ```X.Y+1.0-SNAPSHOT``` (or whatever) in ```build.gradle```, commit and push.