# mvn

> Apache Maven.
> Tool for building and managing Java-based projects.
> More information: <https://maven.apache.org>.

- Compile a project:

`mvn compile`

- Compile and package the compiled code in its distributable format, such as a `jar`:

`mvn package`

- Compile and package, skipping unit tests:

`mvn package -DskipTests`

- Install the built package in local maven repository. (This will invoke the compile and package commands too):

`mvn install`

- Delete build artifacts from the target directory:

`mvn clean`

- Do a clean and then invoke the package phase:

`mvn clean package`

- Clean and then package the code with a given build profile:

`mvn clean -P{{profile}} package`

- Run a class with a main method:

`mvn exec:java -Dexec.mainClass="{{com.example.Main}}" -Dexec.args="{{arg1 arg2}}"`



# Custom ...........................................................................................
```bash
# run individual test
mvn test -Dtest=MyTestClass#myTestMethod

# clean/build: When phase given, every phase in the sequence up to one is processed
mvn -Pbmw clean package  # Connected Drive: BMW profile
mvn package -Dclassifier=aws

# run it
java -cp target/my-app-1.0-SNAPSHOT.jar com.mycompany.app.App

# show deps
mvn dependency:tree
# Donwload Docs
mvn dependency:resolve -Dclassifier=javadoc
# Donwload Sources
mvn dependency:sources dependency:resolve -Dclassifier=javadoc

# check configuration correctness
mvn help:effective-settings
# print effective-pom
mvn help:effective-pom

# Show javadoc plugin goals:
mvn javadoc:help
# detail about the javadoc goal of the javadoc plugin :
mvn javadoc:help -Ddetail -Dgoal=javadoc
```

## Troubleshooting, Resetting
```bash
mvn clean install -U  # update snaphsots (no release)
# purge local repo and reinstall
mvn dependency:purge-local-repository clean install
```

## Create project
```bash
# example project (outdated)
mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4 -DinteractiveMode=false

# simple project (outdated)
mvn archetype:generate -DgroupId=de.sysid -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-simple

# https://www.morling.dev/blog/introducing-oss-quickstart-archetype/
mvn archetype:generate -B -DarchetypeGroupId=org.moditect.ossquickstart -DarchetypeArtifactId=oss-quickstart-simple-archetype -DarchetypeVersion=1.0.0.Alpha1 -DgroupId=de.sysid.demos -DartifactId=app -Dversion=1.0.0-SNAPSHOT -DmoduleName=None
mvn archetype:generate -DarchetypeGroupId=org.moditect.ossquickstart -DarchetypeArtifactId=oss-quickstart-simple-archetype -DarchetypeVersion=1.0.0.Alpha1

# https://github.com/oliviercailloux/Java-Archetype
mvn archetype:generate -DarchetypeGroupId=io.github.oliviercailloux -DarchetypeArtifactId=java-archetype

```
