# Base POM

Provides base project dependencies and build setup for VEuPathDB maven-based projects.  The released POM should be used as a parent pom across all Maven-based VEuPathDB projects in an effort to standardize the libraries we use across the project and help ensure e.g. that transitive dependency conflicts are more easily managed.  To that end, this POM contains set version numbers for (hopefully) all libraries used by said projects.

## Release Procedure

This project is set up to use the maven release plugin.  The latest version on HEAD should be kept as a SNAPSHOT version of the next release (e.g. 1.0-SNAPSHOT).  After changes are made that require a release, prepare the release:

1. Commit and push all changes
2. Run `mvn -Dusername=git release:prepare`.  This will ask you three questions:
  1. What version should be released?  Probably the SNAPSHOT version without SNAPSHOT, e.g. 1.0, but may jump to next major version
  2. What tag name should be used to tag this commit?  Convention is 'v' + release, e.g. v1.0
  3. What version should remain for the next development cycle?  This will be the next snapshot, e.g. 1.1-SNAPSHOT
3. If your github credentials are set up correctly, this build should succeed.  It will have:
  1. Changed the version of the pom to the release version and committed it under the chosen tag
  2. Changed the version of the pom to the next SNAPSHOT version
  3. Added local uncommitted files needed to perform the release

After a successful 'release:prepare', you can perform the release.  To do so, you will need to declare your github username/token in ~/.m2/settings.xml, e.g.
```
<settings>
  <servers>
    <server>
      <id>github</id>
      <username>[your_github_username]</username>
      <password>[your_github_token]</password>
    </server>
  </servers>
</settings>
```

1. Run `mvn release:perform`.  This should deploy the pom artifact to our Github Packages repository and clean up the directory
2. You may notice some unpushed changes in your local checkout after 'release:perform' completes.  Please push these changes manually.
