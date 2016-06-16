# mongo-replica-set

This image runs mongodb server in replica set mode.

## Build:
    docker build -t mongo-replica-set

## Push:
    docker push user/mongo-replica-set:tag
If you want to push the image to one of your repositories at https://hub.docker.com , replace user with your own username and set the correct tag


## Usage:

    docker run -d \
        --env AUTH=no \
        --env REPL_SET=rs0 \
        -v /host/path:/data/db
        mongo-replica-set

Replace `/host/path` with a local folder on your machine, this allows the image to use a host OS folder for the mongo files, thus making them persistent. Otherwise you may loose data when closing and opening the container 

Replace `rs0` to whatever you want your replica set to be named

Alternatively you can set it up via [cloud.docker](https://cloud.docker.com/, "cloud.docker.com") once you have pushed the image in one of your repositories

Once you have a few instances running, you need to connect them in the replica set. 
Get the IP or hostname for all the docker instances running mongo-replica-set and set them in variables for easier access

    export MONGO1="meteor-mongo-1"
    export MONGO2="meteor-mongo-2"
    export MONGO3="meteor-mongo-3"

### Note: you have to add -u admin -p $MONGODB_PATH if you are using authentication.

    mongo $MONGO1 --eval "rs.initiate(); myconf = rs.conf(); myconf.members[0].host = '$MONGO1'; rs.reconfig(myconf,{force:true}); rs.add('$MONGO2'); rs.add('$MONGO3');"


## Parameters

### Minimum requirements

    AUTH            `yes` or `no`, used to identify the need of setting up a password, for yes values, see MONGODB_PASS 
    REPL_SET        a replica set name, to be later used when connecting to the mongodb

### Optional parameters
    MONGODB_PASS    password to be set for the mongo `admin` user, set AUTH to yes for this to work 
    OPLOG_SIZE      oplog size to be set at mongodb server startup 
    JOURNALING      `yes` or `no`, if set to no, mongodb will run with `--nojournal` flag
    STORAGE_ENGINE  storage engine to be used by mongodb server, defaults to `wiredTiger`

