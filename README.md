# Developement workflow

### Dockerfile Note
Note when creating the Dockerfile I needed to configure locale evnironment as most base os images are minimal and require the user to customize it. See the Dockerfile as the following lines were added for locale support:

```
ENV OS_LOCALE="en_US.UTF-8"
RUN locale-gen ${OS_LOCALE}
ENV LANG=${OS_LOCALE} \
    LANGUAGE=en_US:en \
        LC_ALL=${OS_LOCALE}
```

When creating a new project bash into the container from the host:

```
docker run -it -v $PWD:/app -p 3000:3000 meteor:v1.0.1 bash
```

Now create the meteor app

```
cd /app
meteor create simple-todos
```

If you're using a VM there is currently an issue with mongo and placing the database on a shared mount with the host.  You need to symlink the db directory that meteor would normally create if it does not already exist. 

```
# mkdir /db # (or whatever path you want as long as its not mapped back to the host.  In this example anything under /app is mapped back to the host so we need to create the db directory somewhere else)
# cd simple-todos/.meteor/local
# ln -s /db
```

Now start the development server

```
# cd /app/simple-todos
# meteor npm install
# meteor
```

Now the initial project files will be created on your host and you can initialize the /app/simple-todos directory as a git repo.  meteor automatically creates default .gitignore files.
