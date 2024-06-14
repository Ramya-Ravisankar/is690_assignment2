
4. **How does Alembic manage database migrations, and why is this important for maintaining database schemas?**
   - Illustrate with an example of a migration script from the project.

### Answer :

## What is Alembic  and how does alembic migration work :
Alembic is a database schema migration tool that uses SQLAlchemy as the underlying engine. Whenever there is a need to modify the existing schema of the database, we have to create an Alembic script that the Alembic can invoke to update the original database schema. It uses ALTER statements to do the same.Alembic has full support for transactional DDL, ensuring no change in database schema upon failure. The most important point to remember is that Alembic executes the scripts sequentially.

## What is Database Schema:
A database schema is an architectural framework that defines the structure, relationships, and constraints of data within a database management system (DBMS). It represents the logical configuration of the entire database, detailing how data is organized and how relational database entities are interrelated. Schemas group objects, including tables, views, indexes, and procedures, into a single logical unit for easier database change management and access.
The four key components of database schema include tables, columns, relationships, and constraints.

## What is Database schema migration :
Database schema migration is the process of managing and applying changes to a database’s structural framework or schema. Sometimes referred to as simply “database migrations,” they allow the schema to evolve alongside the application.

## Step-by-step: Database schema migration :
With strategy in place and approved by all stakeholders, we can get to the business of migrating database schema. Each step in the process as below

1) Pre-migration
Evaluate the current schema and outline the migration plan.
2) Back up data
Prevent potential data loss in case of migration failure with comprehensive data backups (as part of your broader IT backup/recovery process).
3) Schema changes
Prepare scripts for necessary schema modifications.
4) Modify tables/columns
Update the database structure to add or alter tables and columns.
5) Define relationships
Establish how different tables interrelate.
6) Apply constraints
Implement constraints for data integrity.
7) Data migration/transformation
Move and transform data to fit the new schema.
8) Version control
Manage schema changes over time using database version control systems.
9) Testing and validation
Conduct extensive testing to ensure correct migration implementation.
10) Unit testing
Verify individual migration components.
11) Integration testing
Test the overall system functionality post-migration.
12) Release migration
Carefully deploy changes to the production environment.
13) Post-migration
Update downstream data systems and application code.
14) Monitor performance
Keep track of system performance after migration.
15) Rollback plan
Have a contingency plan for reverting changes if necessary.
16) Monitor drift
Watch for out-of-process changes (drift) to catch them quickly and avoid downstream problems.

## Screenshots - Step by Step on how Alembic Script is being executed in our project:

1) After Pytest run and Before Alembic Migration Script is being executed, the below shows the DB Screenshot where we can observe that no tables are available

[DB_Screenshot_afterPytestRun_beforeAlembicMigrationScriptRun](/screenshots/Question4/DB_Screenshot_afterPytestRun_beforeAlembicMigrationScriptRun.png)

2) Alembic Script Migration Run command - Screenshot taken from the terminal :

[Alembic_Migration_Script_Execution_Screenshot](/screenshots/Question4/Alembic_Migration_Script_Execution_Screenshot.png)

3) After Alembic Migration Script run, the below shows the DB Screenshot where tables are available - Users and Events Table.

[DB_Screenshot_afterAlembicMigrationMigrationScriptRun](/screenshots/Question4/DB_Screenshot_afterAlembicMigrationMigrationScriptRun.png)

## Code Snippet:
Below is the Alembic Migration Script command thats executed in the terminal and its output.
Screenshots are already attached as above.

```
docker compose exec fastapi alembic upgrade head

```

   [Return back to answer.md](/answer.md)