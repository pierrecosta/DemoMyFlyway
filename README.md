# Database Management

How to create changelog in order to use Liquibase

## Main principles
A changelog-master will reference all other changelogs to migrate
Changelog will be a list of changeset to update the DB
Each changeset will contain information about the modification and one SQL modification

### Warnings
Due to security, technical user is the schema were migrations will be done.
Currently, 2 sorts of DB : HSQLDB and Oracle


## Table of contents
* changelog-master
* changelog and changeset
* sql tree structure and norms
* sqlplus

# Changelog-master
*What's this file ?* **It's the entry point for each Liquibase migration**
This file contains :
* Init file to create schema in local environment
   This is usefull to update HSQLDB to restart from scratch easily
```
    - include :
        file: initdb/changelog-init.yaml
        relativeToChangelogFile: true
```

* list of changelog to applied to migrate the DB
   Each Changelog need to be releasable separatly
```
    - include :
        file: v1/changelog-JIRA-011.yaml
        relativeToChangelogFile: true
```

* Tag to set version and package used for the deployment
   This is only for tracability and lisibility of database state
   It's stored inside the db
```
    - changeSet:
        id: tagDatabaseV1
        author: MajorVersion
        comment: '${packageInfo}'
        tagDatabase:
            tag: Migration_V1
```
  
Here a full example :
```
databaseChangeLog:
    - include :
        file: initdb/changelog-init.yaml
        relativeToChangelogFile: true
    - include :
        file: v1/changelog-JIRA-011.yaml
        relativeToChangelogFile: true
    - include :
        file: v1/changelog-JIRA-012.yaml
        relativeToChangelogFile: true
    - changeSet:
        id: tagDatabaseV1
        author: MajorVersion
        comment: '${packageInfo}'
        tagDatabase:
            tag: Migration_V1
```
