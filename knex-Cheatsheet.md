[back to overview](/../..)

# knex Cheatsheet<!-- omit in toc -->

- [Database](#database)
  - [Migrations](#migrations)
  - [Seed](#seed)

# Database

## Migrations
- `knex migrate:list` - list all completed and pending
- `knex migrate:up` - up one
- `knex migrate:up migrationname.js` - up only the specified
- `knex migrate:latest` - up all to latest
- `knex migrate:down` - down last migration
- `knex migrate:down migrationname.js` - down only the specified
- `knex migrate:rollback` - rollback last batch
- `knex migrate:rollback --all` - rollback all
- `knex migrate:make migration_name` - create migration with name migration_name.js

## Seed
- `knex seed:run --esm` - run all seed files in alpha order
- `knex seed:run --esm --specific=seedname.js` - run only specific seed file
