## Setting up Minishift 
### on Mac OSX Mojave 10.14.5

### Step I - download latest Minishift binary
I downloaded [minishift-1.17.0-darwin-amd64.tgz](https://github.com/minishift/minishift/releases/download/v1.17.0/minishift-1.17.0-darwin-amd64.tgz)
There may be newer versions on the [Minishift Github release page](https://github.com/minishift/minishift/releases) 

once downloaded expand the tar file and copy `minishift` to the `/usr/local/bin` folder

```shell
# untar the downloaded file
tar -xvf minishift-1.17.0-darwin-amd64.tgz
sudo mv ~/Downloads/minishift-1.17.0-darwin-amd64/minishift /usr/local/bin
sudo chmod +x /usr/local/bin/minishift
```


### Step II - Install dependencies / Virtualization Environment
Minishift requires a hypervisor to start the virtual machine on which the OpenShift cluster is provisioned. 

Before Minishift can run, we need setup the virualization environment which supports Openshift running under MacOS environment.

There are a few options here

1. xhyve
1. hyperkit
1. VirtualBox

I chose `xhyve`

Minishift is currently tested against `docker-machine-driver-xhyve version 0.3.3`

When doing this step, I assume you have [brew](https://brew.sh/) installed /configured correctly in your computer.

You can simply copy and paste code below and run in a terminal.

```
# Update brew first
brew update
# install docker-machine-driver-xhyve
brew install docker-machine-driver-xhyve

# docker-machine-driver-xhyve need root owner and uid
$ sudo chown root:wheel $(brew --prefix)/opt/docker-machine-driver-xhyve/bin/docker-machine-driver-xhyve

# Set the owner User ID (SUID) for the binary 
$ sudo chmod u+s $(brew --prefix)/opt/docker-machine-driver-xhyve/bin/docker-machine-driver-xhyve
```

( Note:  I had docker app installed with Minikube installed - which I stopped running first
as I felt it may have caused issues)

( Note: brew installed a later version of docker-machine-driver-xhyve )
- so I had to follow steps in troubleshooting below to downgrade to v0.3.3

You can verify the existing version of the xhyve driver on your system using brew info as follows:
``` sh
brew info docker-machine-driver-xhyve
docker-machine-driver-xhyve: stable 0.3.3 (bottled)
```


### Step III - run minishift

```sh
minishift start
```




## Trouble shooting 
So I had some issues with the version v0.4.0 of `docker-machine-driver-xhyve`
that brew installed.  there are compatibility issues with minishift version 1.17.0

The solution for me was to uninstall the docker-machine-driver-xhyve 0.4.0 driver and install the old and tested v0.3.3 of `docker-machine-driver-xhyve`

```sh
brew uninstall docker-machine-driver-xhyve
brew edit docker-machine-driver-xhyve
```
I now edited the brew file in VI

Change the `tag:` AND `revision:` references to:

```java 
class DockerMachineDriverXhyve < Formula
  desc "Docker Machine driver for xhyve"
  homepage "https://github.com/machine-drivers/docker-machine-driver-xhyve"
  url "https://github.com/machine-drivers/docker-machine-driver-xhyve.git",
      :tag      => "v0.3.3",
      :revision => "7d92f74a8b9825e55ee5088b8bfa93b042badc47"
  revision 2
```
now run brew as before it will use the older version

```sh
# Update brew first
brew update
# install docker-machine-driver-xhyve
brew install docker-machine-driver-xhyve

# docker-machine-driver-xhyve need root owner and uid
$ sudo chown root:wheel $(brew --prefix)/opt/docker-machine-driver-xhyve/bin/docker-machine-driver-xhyve

# Set the owner User ID (SUID) for the binary 
$ sudo chmod u+s $(brew --prefix)/opt/docker-machine-driver-xhyve/bin/docker-machine-driver-xhyve
```

## References:
I followed mostly the instructions laid out here:
http://keyangxiang.com/2018/05/18/Openshift/how-to-run-minishift-on-macos/

https://stackoverflow.com/questions/56358247/error-creating-new-host-json-cannot-unmarshal-bool-into-go-struct-field-driver

https://github.com/machine-drivers/docker-machine-driver-xhyve#install

Official docs
https://docs.okd.io/latest/minishift/getting-started/setting-up-virtualization-environment.html

