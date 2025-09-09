# the-sound-mind

<details>
  <summary>System Specific Notes</summary>

  <details>
    <summary>Colima on MacOS</summary>
    - In order to avoid running Docker Desktop we've used Colima.  It can be installed with [Homebrew](https://brew.sh/).
    
    <code>
    brew install docker-compose
    </code>

    - The colima VM needs to be started with

    colima start virtio --mount-type=virtiofs

    - Something similar to this
    
    {
            "currentContext": "colima-virtio",
            "cliPluginsExtraDirs": [
                    "/opt/homebrew/lib/docker/cli-plugins"
            ]
    ]
    
    may need to be added to your Docker config.  This allows Docker to find the docker-compose plugin which will allow commands like

    docker compose up  # without the dash (Docker executing compose as a plugin)

    as opposed to commands like

    docker-compose up  # with the dash (directly running the docker-compose binary)

    We've found this to be nessesary, as bringing up the containers with docker-compose instead of docker compose will result in connectivity issues between containers.

    The following may need to be added to your `~/.docker/config.json` in order to avoid permissions issues with certain volumes.
  </details>

  <details>
  <summary>Docker Engine on Linux</summary>
  We recommend installing Docker with the instructions at 
  [Install Docker Engine](https://docs.docker.com/engine/install/)
  </details>

</details>

## Running the app

### Starting the supabase containers

This will start the containers providing the supabase services, local on your machine.

Open your docker desktop to start the docker service, then open a shell in the root folder.

```shell
# Go to the docker folder
cd supabase/docker

# Copy the fake env vars
cp .env.example .env

# Pull the latest images
docker compose pull

# Start the services (in detached mode)
docker compose up -d

# If you're on a mac pull the storage container requirements
docker compose -f docker-compose.yml -f docker-compose.s3.yml up -d
```

Wait for the containers to start up, and confirm they are running in docker desktop.

### Running the app for development

When making changes to the app, this will be the most efficient way to view your changes in the app. 

Open a shell in the root folder.

```shell
# Change to root directory
cd ../../

# (re)Install npm packages
npm i

# Copy the fake env vars
cp .env.dev .env

# Start the app using npm
npm run dev
```

You should be able to access the app server at

`http://localhost:3000`

### Ensure requirements for storage bucket are configured

# Log into supabase UI (this will eventually be in code)
localhost:8000
user: supabase
password: this_password_is_insecure_and_should_be_updated

On the left side column select `Storage`

Under `All Buckets` select `song-files`

Create a new folder called `listen`
The UI is wonky and will stay for the moment, but will look like it deleted.

Under `Configuration` select `Policies`

Click Create `New policy`

Select `Get Started Quickly`

Select `Give users access to a folder only to authenticated users`

Select `Use this template`

Select all permissions `SELECT INSERT UPDATE DELETE`

In the `Policy Definition` change `private` to `listen`
Which should look like this
```bucket_id = 'song-files' AND (storage.foldername(name))[1] = 'listen' AND auth.role() = 'authenticated'```

Select `Review`

Select `Save Policy`

Occasionally we've seen an error
`Error adding policy: API error happened while trying to communicate with the server.`
Ignore this, but ensure in the `Configuration` `Policies` tab that there are 4 policies in place.

# Create a user and update with Admin permissions
Go to `http://localhost:3000/auth/login`

Select `Create Account` and create an account

Go to `localhost:8000`

Select `Database` and then select `Tables`

Under the `Users` table select the 3 dots and choose `View in table editor`

Set the `is_site_admin` value to `TRUE` by typing `T` then Enter

Go back to `localhost:3000` refresh the page, and now you're an admin!

### Running the app in docker

To test the app in a production-similar environment, with the app only accessible through the nginx reverse proxy.

Open a shell in the root folder.

```shell
# (re)Install npm packages
npm i

# (re)Build and start the services (in detached mode)
docker compose up --build -d
```

Wait for the containers to start up, and confirm they are running in docker desktop. You should be able to access the app through nginx at 

`http://localhost:3003`

You will not be able to access the app server directly as it is not publicly exposed, and is only accessible to the reverse proxy within the container at port `3000`.

## Running unit tests

You will need to have the supabase docker container(s) up and running.

Then just use

`npm test`

and you should see results with coverage.
