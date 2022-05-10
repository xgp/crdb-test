# repo

let's start with the `xgp/crdb-legacy` branch in my fork of the keycloak repo: https://github.com/xgp/keycloak/tree/xgp/crdb-legacy

# testing keycloak(quarkus) startup

in the keycloak repo:
```
mvn clean install -DskipTests
```
(once you have done a full build, you can just rebuild what you are working on and then run a `mvn clean install -DskipTests` in quarkus/dist/)

in another work directory:
```
rm -rf keycloak-999-SNAPSHOT/
cp $KEYCLOAK_REPO/quarkus/dist/target/keycloak-999-SNAPSHOT.zip .
unzip keycloak-999-SNAPSHOT.zip
cd keycloak-999-SNAPSHOT/
./bin/kc.sh --verbose start-dev --db=cockroach --db-username=user --db-password=xxxx --db-url=jdbc:postgresql://free-tier13.aws-eu-central-1.cockroachlabs.cloud:26257/defaultdb?options=--cluster%3Dkeycloak-test-658 --transaction-xa-enabled=false --db-pool-min-size=0
```

because of the failure, the database gets in a bad state, you need to run `delete-from-kc-tables-17.0.1.sql` in your database between runs, and optionally `drop-all-kc-tables-17.0.1.sql` if you want to clear the schema (to force keycloak bootstrap to run liquibase migration from scratch).



# other notes

## how to do a sql dump from postgres in a docker container

docker exec 9be8a523659e pg_dump --column-inserts --data-only -U keycloak keycloak >backup

