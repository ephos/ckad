# Security Concepts

Docker hosts and containers share the same kernel, in that way:

- Docker containers can only see their own processes
- Docker hosts see all of their processes as well as the child containers

Effectively you get process isolation.

Now in terms of users there is the root user and the non-root users.

Docker runs the container processes as root user by default.
You can set the user for the container with the `--user=1000` of the docker command.

Better yet, you can set the user in the **DockerFile** by specifying `USER 1000`

When a container is running as the root user it doesn't mean it has full root to the host.
The root user in the container is contained and cannot for example reboot the host machine or access other containers.
If you want to play with fire you can use `--cap-add <NAME>` to add the missing capabilities.
You can also drop them with `--cap-drop <NAME`.  Or run with `--privileged` to give all.
