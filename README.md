# ZenStack Todo Sample With SvelteKit

This project is a collaborative Todo app built with [SvelteKit](https://kit.svelte.dev/) and [ZenStack](https://zenstack.dev).

In this fictitious app, users can be invited to workspaces where they can collaborate on todos. Public todo lists are visible to all members in the workspace.


## Features

-   User signup/signin
-   Creating workspaces and inviting members
-   Data segregation and permission control

## Running the sample

1. Setup a new database

    It use PostgreSQL by default, if you want to use MySQL, simply change the db datasource provider to `mysql` in `schema.zmodel` file.
    
    You can launch a PostgreSQL instance locally, or create one from a hoster like [Supabase](https://supabase.com). Create a new database for this app, and set the connection string in .env file.

1. Install dependencies

    ```bash
    npm install
    ```

1. Generate server and client-side code from model

    ```bash
    npm run generate
    ```

1. Synchronize database schema

    ```bash
    npm run db:push
    ```

1. Start dev server

    ```bash
    npm run dev
    ```



## Feedback and Issues
If you encounter any issue or have any feedback, please create a new issue in our main repository so we could respond to it promptly:

[https://github.com/zenstackhq/zenstack](https://github.com/zenstackhq/zenstack)