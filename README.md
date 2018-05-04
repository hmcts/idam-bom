# IdAM BOM

The IdAM BOM project provides Maven BOM file that can be used for managing the versions of the dependencies in IdAM 
projects. This would ensure all projects always use a compatible stack.
                                             
## Usage

There are 2 ways to use the BOM file:

##### Inheriting the BOM file
```xml
<project ...>
    <modelVersion>4.0.0</modelVersion>
    <groupId>uk.gov.hmcts.reform.idam</groupId>
    <artifactId>idam-state-of-the-art-project</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>pom</packaging>
    <name>IdAM State of the Art</name>
    <parent>
        <groupId>uk.gov.hmcts.reform.idam</groupId>
        <artifactId>idam-bom</artifactId>
        <version>1.0.0</version>
    </parent>
</project>
```

##### Importing the BOM file
```xml
<project ...>
    <modelVersion>4.0.0</modelVersion>
    <groupId>uk.gov.hmcts.reform.idam</groupId>
    <artifactId>idam-state-of-the-art-project</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>pom</packaging>
    <name>IdAM State of the Art</name>
         
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>uk.gov.hmcts.reform.idam</groupId>
                <artifactId>idam-bom</artifactId>
                <version>1.0.0</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
</project>
```

## Overriding a dependency defined in the BOM
The order of precedence of the artifact’s version is:

* The version of the artifact’s direct declaration in your project pom
* The version of the artifact in the parent project
* The version in the imported pom, taking into consideration the order of importing files
* Standard Maven dependency mediation (mvn uses "nearest definition" strategy, see [Maven docs](http://people.apache.org/~jvanzyl/maven-3.1.1/guides/introduction/introduction-to-dependency-mechanism.html) for more information)

If you are importing the BOM file or if you are overriding a dependency that's not explicitly defined there (meaning 
it's being imported from a third party BOM file), then you'd need to add an entry in the `dependencyManagement` of your 
project *before* the `idam-bom` entry.
```xml
<dependencyManagement>
    <dependencies>
        <!-- Override Mockito provided by IdAM BOM -->
        <dependency>
            <groupId>org.mockito</groupId>
            <artifactId>mockito-core</artifactId>
            <version>2.12.0</version>
        </dependency>
        <dependency>
            <groupId>uk.gov.hmcts.reform.idam</groupId>
            <artifactId>idam-bom</artifactId>
            <version>1.0.0</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

If you are inheriting the BOM file *and* the dependency is explicitly defined in IdAM BOM (as opposed to being 
imported), then you can override individual dependencies by overriding a property in your own project. 
```xml
<properties>
    <mockito.version>2.12.0</mockito.version>
</properties>
```
