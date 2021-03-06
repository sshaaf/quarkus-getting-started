# quarkus-getting-started project

This project uses Quarkus, the Supersonic Subatomic Java Framework.

If you want to learn more about Quarkus, please visit its website: https://quarkus.io/ .

## Running the application in dev mode


The default application.properties expects you to have a postgresql database running. Hopefully you have docker installed then you can run the script in the repo home

```
./start-database.sh
```

You can run your application in dev mode that enables live coding using:
```
./mvnw quarkus:dev
```

also notice the %dev in the application.properties which means that the %.dev is only when you run in dev mode. the other properties will be used in openshift.


## Packaging and running the application

The application can be packaged using `./mvnw package`.
It produces the `quarkus-getting-started-1.0.0-SNAPSHOT-runner.jar` file in the `/target` directory.
Be aware that it’s not an _über-jar_ as the dependencies are copied into the `target/lib` directory.

The application is now runnable using `java -jar target/quarkus-getting-started-1.0.0-SNAPSHOT-runner.jar`.

## Creating a native executable

You can create a native executable using: `./mvnw package -Pnative`.

Or, if you don't have GraalVM installed, you can run the native executable build in a container using: `./mvnw package -Pnative -Dquarkus.native.container-build=true`.

You can then execute your native executable with: `./target/quarkus-getting-started-1.0.0-SNAPSHOT-runner`

If you want to learn more about building native executables, please consult https://quarkus.io/guides/building-native-image.


## Running this on Openshift with Postgresql
```
mvn quarkus:add-extension -Dextensions="openshift"
```

Login to openshift with the oc command

```
oc new-project quarkus-todo
```

Use the following command to create a postgresql database in your openshift namespace

```
oc new-app \
 -e POSTGRESQL_USER=restcrud \
 -e POSTGRESQL_PASSWORD=restcrud \
 -e POSTGRESQL_DATABASE=restcrud \
 -e POSTGRESQL_MAX_CONNECTIONS=200 \
 --name=postgres-database \
 openshift/postgresql
```

And finally to deploy the application into Openshift
```
mvn clean package -Dquarkus.kubernetes.deploy=true
```

now point to /todo.html or /api for json and you would see the app response




