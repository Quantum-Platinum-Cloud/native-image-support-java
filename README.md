# Native Image Support for Google Cloud Libraries

**Please Note**: If you are using version 25.4.0 or later of the `libraries-bom`, the Cloud Client Libraries for Java come with the native image configurations built-in. This means that the Cloud Client libraries can be compiled into native images without the need for adding the `native-image-support` dependency. 

[![experimental](http://badges.github.io/stability-badges/dist/experimental.svg)](http://github.com/badges/stability-badges)

This repository provides support for applications using the [Google Java Client Libraries](https://github.com/googleapis/google-cloud-java#supported-apis) to be built as [GraalVM Native Images](https://www.graalvm.org/reference-manual/native-image/).

## Setup

Add the `native-image-support` artifact to your project to take advantage of native image support.

For example, in Maven:

```
<dependency>
    <groupId>com.google.cloud</groupId>
    <artifactId>native-image-support</artifactId>
    <version>0.10.0</version>
</dependency>
```

This dependency contains the GraalVM configurations to provide out-of-the-box support for native-image compilation of applications using Google Java Client Libraries.

### Client Library Versions

To compile with native image, ensure the client library version in your project is supported by `native-image-support`.

| Native Image Support version | GraalVM Compiler Version | *`libraries-bom` version | `grpc-netty-shaded` version |
|-------------------------|--------------------------|:-------------------------|-------------------------------|
| `0.10.0`                | `>= 21.3.0`              | `24.0.0` or later        | `1.42.0` or later             |
| `0.9.0`                 | `>= 21.3.0`              | `23.x.x - 24.0.0`        | `1.41.0`                      |
| `0.8.0`                 | `>= 21.2.0`              | `23.x.x - 24.0.0`        | `1.41.0`                      |

Typically, you can just depend on the latest versions of the client libraries to get something working if you are not sure about what versions of (transitive) dependencies are being used by your project.

**NOTE:** Most users typically manage their client library versions using the [Cloud Libraries Bill of Materials](https://github.com/GoogleCloudPlatform/cloud-opensource-java/wiki/The-Google-Cloud-Platform-Libraries-BOM) (`libraries-bom`).
This is an easy way to ensure that the versions of all the client libraries in your project are compatible with each other and up-to-date.
The `libraries-bom` also manages the version of `grpc-netty-shaded` as well and ensures that it is at the latest compatible version.

### GraalVM Features

This project offers [GraalVM features](https://www.graalvm.org/sdk/javadoc/index.html?org/graalvm/nativeimage/hosted/Feature.html) which provides optional reflection configurations for specific use-cases.
These features should only be used when necessary because they increase the size of the container image and increase compilation times.

* `ProtobufMessageFeature`: Adds reflection metadata for request objects of the Google Cloud APIs that your application uses.
This is typically only needed if you need to print/log requests objects or access them reflectively.

Add the features as an additional build argument to the native-image compiler via the `--features` flag.

Command line Example:

```
native-image -cp <other settings> --features=ProtobufMessageFeature
```

The [Cloud Tasks code sample](native-image-samples/native-image-samples-client-library/tasks-sample/pom.xml) demonstrates how to use this setting.

## Supported Libraries

Most of the Java Google Client Libraries [listed here](https://github.com/googleapis/google-cloud-java#supported-apis) are supported for GraalVM compilation using this dependency.
These libraries are all listed under the `com.google.cloud` group ID.

If you find an unsupported library, please make a feature request via our [Github Issue Tracker](https://github.com/GoogleCloudPlatform/native-image-support-java/issues).

GraalVM-compatible sample code using various Google Cloud libraries can be found below:

| Google Cloud Service Library  | Sample Link              | 
|-------------------------|--------------------------|
| [Cloud BigQuery](https://github.com/googleapis/java-bigquery) | [bigquery-sample](./native-image-samples/native-image-samples-client-library/bigquery-sample) |
| [Cloud BigTable](https://github.com/googleapis/java-bigtable) | [bigtable-sample](./native-image-samples/native-image-samples-client-library/bigtable-sample) |
| [Cloud Datastore](https://github.com/googleapis/java-datastore) | [datastore-sample](./native-image-samples/native-image-samples-client-library/datastore-sample) |
| [Cloud Firestore](https://github.com/googleapis/java-firestore) | [firestore-sample](./native-image-samples/native-image-samples-client-library/firestore-sample) |
| [Cloud Logging](https://github.com/googleapis/java-logging) | [logging-sample](./native-image-samples/native-image-samples-client-library/logging-sample) |
| [Cloud Pub/Sub](https://github.com/googleapis/java-pubsub) | [pubsub-sample](./native-image-samples/native-image-samples-client-library/pubsub-sample) |
| [Cloud Secret Manager](https://github.com/googleapis/java-secretmanager) | [secretmanager-sample](./native-image-samples/native-image-samples-client-library/secretmanager-sample) |
| [Cloud SQL (w/ MySQL)](https://github.com/GoogleCloudPlatform/cloud-sql-jdbc-socket-factory) | [cloud-sql-sample](./native-image-samples/native-image-samples-client-library/cloud-sql-sample) |
| [Cloud Spanner](https://github.com/googleapis/java-spanner) | [spanner-sample](./native-image-samples/native-image-samples-client-library/spanner-sample) |
| [Cloud Storage](https://github.com/googleapis/java-storage) | [storage-sample](./native-image-samples/native-image-samples-client-library/storage-sample) |
| [Cloud Tasks](https://github.com/googleapis/java-tasks) | [tasks-sample](./native-image-samples/native-image-samples-client-library/tasks-sample) |

Additional API compatibility is in active development.

Please also consult the project [samples applications directory](./native-image-samples) for the full range of code samples.

### Additional Frameworks

Our project targets compatibility for native image frameworks as well, such as for Quarkus, Micronaut, and Spring.
We are in the early stages of research for these frameworks and maintain some [code samples](./native-image-samples).

We are also interested in collaborating with other open source projects to improve framework-level compatibility.

Related projects:

*  [Quarkus Extension for Google Cloud Services](https://github.com/quarkiverse/quarkiverse-google-cloud-services) - Enables usage of Google Cloud libraries in Quarkus applications.

Please let us know if you are interested in collaborating by contacting us via our [Issue Tracker](https://github.com/GoogleCloudPlatform/native-image-support-java/issues).

## Questions

Please report any issues and questions to our [Github Issue Tracker](https://github.com/GoogleCloudPlatform/native-image-support-java/issues).
