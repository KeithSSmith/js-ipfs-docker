# Javascript IPFS Docker Image

This is more a less a temporary solution to the Dockerfile in the js-ipfs library and is my note taking on how to reproduce.

## Pull the repo

```sh
git clone https://github.com/ipfs/js-ipfs.git
```

## Update the Dockerfile

Replace the Dockerfile that is part of the cloned repository with the Dockerfile included in this repo. The only changes are an updated version of Node and adding the following at the end of the `RUN` statement:

```Dockerfile
  && node src/cli/bin.js init
```

## Update init_and_daemon.sh

Replace the similarly named file with the file in this repo. This moves IPFS initialization into the Dockerfile and ensures the Docker instance does not stop once done starting the daemon.

## Build the Docker image

Inside the base directory of the `js-ipfs` repo we should now build the Docker image based on the changed files.

```sh
docker build -t js-ipfs-file-node-10 .
```

## Execute the Docker Container

Running the container detached with the necessary ports exposed required to interact with the rest of the IPFS network.

```sh
docker run -dt \
 --name js_ipfs_host \
 -p 4002:4002 \
 -p 4003:4003 \
 -p 5002:5002 \
 -p 9090:9090 \
 js-ipfs-file-node-10 /bin/bash
```

## jsipfs Alias to execute command inside Docker container

This step is included so that the `jsipfs` command acts like it would if this were a local installation and not inside a docker container. Create or add the function to the `~/.bash_aliases` file and reload the terminal settings. Once properly installed it is possible to interact with `jsipfs` as a local command.

```sh
jsipfs cat /ipfs/QmfGBRT6BbWJd7yUc2uYdaUZJBbnEFvTqehPFoSMQ6wgdr/readme
Hello and Welcome to IPFS!

██╗██████╗ ███████╗███████╗
██║██╔══██╗██╔════╝██╔════╝
██║██████╔╝█████╗  ███████╗
██║██╔═══╝ ██╔══╝  ╚════██║
██║██║     ██║     ███████║
╚═╝╚═╝     ╚═╝     ╚══════╝

If you're seeing this, you have successfully installed
IPFS and are now interfacing with the ipfs merkledag!

 -------------------------------------------------------
| Warning:                                              |
|   This is alpha software. Use at your own discretion! |
|   Much is missing or lacking polish. There are bugs.  |
|   Not yet secure. Read the security notes for more.   |
 -------------------------------------------------------

Check out some of the other files in this directory:

  ./about
  ./help
  ./quick-start     <-- usage examples
  ./readme          <-- this file
  ./security-notes
```
