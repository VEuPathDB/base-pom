# Base POM

Provides base project dependencies and build setup for VEuPathDB maven-based projects.  The released POM should be used as a parent POM across all Maven-based VEuPathDB projects in an effort to standardize the libraries we use across the project and help ensure e.g. that transitive dependency conflicts are more easily managed.  To that end, this POM contains set version numbers for (hopefully) all libraries used by said projects.

## Usage

To use this in a new Maven-based Java projects, specify it as the parent POM:
```
  <parent>
    <groupId>org.gusdb</groupId>
    <artifactId>base-pom</artifactId>
    <version>2.7</version>
  </parent>
```

If you aren't already using other VEuPathDB libraries, you'll also need to add the VEuPathDB Maven repository:
```
  <repositories>
    <repository>
      <id>veupathdb</id>
      <url>https://maven.pkg.github.com/VEuPathDB/maven-packages</url>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
    </repository>
  </repositories>
```

## Release Procedure

This project is set up to use the maven release plugin.  The latest version on HEAD should be kept as a SNAPSHOT version of the next release (e.g. 1.0-SNAPSHOT).  The `release.sh` script will streamline the release procedure, given the following prerequisites:

1. You can currently push changes/tags to this repo (via SSH key or other credentials)
2. Your github credentials are stored in the current environment ($GITHUB_USERNAME, $GITHUB_TOKEN)
3. You have permission to deploy artifacts to Github Packages in the [maven-packages repo](https://github.com/VEuPathDB/maven-packages)

If so, then clone this repo and run `./release.sh`.  It will ask you three questions:

1. What version should be released?
    1. Probably the SNAPSHOT version without SNAPSHOT, e.g. 1.0, but may jump to next major version
1. What tag name should be used to tag this commit?
    1. Convention is 'v' + release, e.g. v1.0
1. What version should remain for the next development cycle?
    1. This will be the next snapshot, e.g. 1.1-SNAPSHOT

After answering the questions, the release will proceed.  Afterward, the following should have occurred:

1. The version of the POM is updated to the release version and tagged with the release tag
1. The artifact is deployed to maven packages
1. The version of the POM is updated to the new snapshot version for the next development cycle
