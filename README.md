Using the standalone jOOQ code generator with Gradle and VertabeloXML
======================================================================

Usage
-----

Create a build.gradle script as follows:

```groovy

import javax.xml.bind.JAXB 
// Configure the Java plugin and the dependencies 
// ---------------------------------------------- 
apply plugin: 'java' 

repositories { 
    mavenLocal() 
    mavenCentral() 
} 

dependencies { 
    compile 'org.jooq:jooq:3.6.1' 
} 

buildscript { 
    repositories { 
        mavenLocal() 
        mavenCentral() 
    } 

    dependencies { 
        classpath 'org.jooq:jooq-codegen:3.6.1' 
        classpath 'org.jooq:jooq-meta-extensions:3.6.1' 
    } 
} 

// Use your favourite XML builder to construct the code generation configuration file 
// ---------------------------------------------------------------------------------- 
def writer = new StringWriter() 
def xml = new groovy.xml.MarkupBuilder(writer) 
        .configuration('xmlns': 'http://www.jooq.org/xsd/jooq-codegen-3.6.0.xsd') { 

    generator() { 

        database() { 
            name('org.jooq.util.vertabelo.VertabeloXMLDatabase') 
                properties() { 
                    property() { 
                        key('dialect') 
                        value('POSTGRES') 
                } 
                    property() { 
                        key('xml-file') 
                        value('library.xml') 
                    } 
                } 
            } 
        generate() {} 
        target() { 
            packageName('com.example.postgres.db') 
            directory('src/main/java') 
        } 
    } 
} 


// Run the code generator 
// ---------------------- 
org.jooq.util.GenerationTool.generate( 
        JAXB.unmarshal(new StringReader(writer.toString()), org.jooq.util.jaxb.Configuration.class) 
)

```

## Type:

	gradle build

The generated code is located under the path provided in target section in build.gradle.

More information about using standalone jOOQ code generator with Gradle here: (http://www.jooq.org/doc/3.4/manual/code-generation/codegen-gradle/)







