# Directus REST API Helper

A small [fish](https://fishshell.com/) script that makes it easy to interact with a [Directus REST API](https://directus.io/docs/api). Handles logging in and refreshing tokens, and wraps [curlie](https://github.com/rs/curlie) to give you a concise query language in your shell, and pairs nicely with tools like [jq](https://github.com/jqlang/jq).

Depends on fish, curlie and jq.

## Usage

``` fish
# login

# expects login credentials to be set like this
set -x DIRECTUS_USER user
set -x DIRECTUS_PASS password
set -x DIRECTUS_URL https://directus.example.com

# first login
source ~/path/to/directus-login

# will set up some environment variables and a `dirr` alias

# subsequent calls to `source ~/path/to/directus-login` will refresh your access token.
# there is no persistence, the enviroment variables will disappear with your shell session.

# example usage

# list all users
dirr $DIRECTUS_URL/users

# filter users by last name and sort by last login date in descending order
dirr $DIRECTUS_URL/users filter[last_name][_contains]=="A%" sort==-last_access

# create new item in a collection
jq -n '{data: {title: "My new blogpost", content: "I have not come up with that yet", status: "draft"}}' | \
    dirr POST $DIRECTUS_URL/items/posts

# update a single item in a collection
jq -n '{status: "preview"}' | \
    dirr PATCH $DIRECTUS_URL/items/posts/123

# update many items in a collection
jq -n '{data: {status: "published"}, query: {filter: {status: "preview"}}}' | \
    dirr PATCH $DIRECTUS_URL/items/posts
```
