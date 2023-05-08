# Docker File Tidbits

## CMD

If you launch the Ubuntu container it immediately exits since its command is 'bash'.
A shell is not a long running process that will keep a container alive.  
If you want to instead keep Ubutu alive longer you can create a new image based on it.

```dockerfile
FROM Ubuntu

CMD sleep 5
```

Or as JSON array format:

_Careful on this one because you need to have the exe/bin and its flags/params comma separated!_

- Good ✅ `["sleep","5"]`
- Bad ❌ `["sleep 5"]`

```dockerfile
FROM Ubuntu

CMD ["sleep", "5"]
```

You can now build this fancy new image that does nothing except sleep 5 seconds before exiting.

```bash
docker built -t ubuntu-sleeper .
docker run ubuntu-sleeper
```

Parameters on a dockerfile can be used via `ENTRYPOINT`

Using an entrypoint is WAY less hardcoded.  Take the following:

```dockerfile
FROM Ubuntu

ENTRYPOINT ["sleep"]
```

Now we can do the following:

```bash
docker built -t ubuntu-sleeper.
docker run ubuntu-sleeper 10
```

We are passing the 10 seconds in as a param to the ENTRYPOINT.
This can be problematic if a parameter isn't passed.  Fix it by using `ENTRYPOINT` with `CMD`!

```dockerfile
FROM Ubuntu

ENTRYPOINT ["sleep"]

CMD ["5"]
```

Now if nothing is passed in, the default will be 5 seconds.

Recap:

`CMD` = Hardcoded command to always run, can only be changed in the container
`ENTRYPOINT` = Pass flags into the command defined in the ENTRYPOINT
`ENTRYPOINT` + `CMD` = Pass flags in but have a default Cmd to run if nothing is passed to Entrypoint

Finally you can overrule the `ENTRYPOINT` at runtime

```bash
docker run --entrypoint bettersleep ubuntu-sleeper 10
```

## Dockerfile Speak to Pod-Definition Speak

In a pod definition file the `command:` key will override the Dockerfiles `ENTRYPOINT`
In addition the `args:` key will override the `CMD`
