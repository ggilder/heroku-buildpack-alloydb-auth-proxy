# Google Cloud AlloyDB Auth proxy buildpack

This Heroku buildpack adds the Google Cloud AlloyDB Auth proxy to your app to enable
connections to AlloyDB instances in GCP.

## Prerequisite

- Google service account with the [required permissions](https://cloud.google.com/alloydb/docs/auth-proxy/connect#required-iam-permissions)
- JSON key for the service account
- Google Cloud AlloyDB instance set up with a postgresql user able to connect
to the database

## Install

Add the proxy to your buildpacks. It's important that this buildpack should
not be the last one in the list, as that's used by Heroku to determine your
startup processes. (--index=1)

     heroku buildpacks:add --index=1 https://github.com/ggilder/heroku-buildpack-alloydb-auth-proxy

Add the GCP JSON credentials as `GOOGLE_CREDENTIALS_JSON` env variable to your app.

Set the instance the proxy should connect to with the `ALLOYDB_INSTANCES` env
variable. Format: `<instance connection name>=tcp:<local port>`. You can get the
**instance connection name** from the Cloud SQL console overview.

Set the connection string for your DB library to
`postgres://<username>:<password>@localhost:<local port>/<database-name>`

Start the proxy before your app tries to connect to the database by e.g. adding
`bin/run_alloydb_auth_proxy` to the `.profile` in the root of your project.