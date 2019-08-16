## Setting up Minishift 
### on Mac OSX Mojave 10.14.5

### Prerequisites
Installed Oracle Virtual Box on MacOS
Docker for Mac



OKD - Origin Community Distribution of Kubernetes that powers Red Hat OpenShift *

To run the VM a hypervisor is required.

There are a few options here

1. xhyve
1. hyperkit
1. VirtualBox




### Step I - download latest Minishift binary
downloaded latest version of Minishift
https://github.com/minishift/minishift/releases
at this time that is version 1.34.1
https://github.com/minishift/minishift/releases/download/v1.34.1/minishift-1.34.1-darwin-amd64.tgz

expanded file then:
cp bin/minishift to /usr/local/bin/minishift

Then ran:
```sh
minishift config set vm-driver virtualbox
minishift start

```

if all goes well

```sh
Login to server ...
Creating initial project "myproject" ...
Server Information ...
OpenShift server started.

The server is accessible via web console at:
    https://192.168.99.100:8443/console

You are logged in as:
    User:     developer
    Password: <any value>

To login as administrator:
    oc login -u system:admin

```
To check that everything is working and view the OpenShift console use the command:

$ minishift console

This launches the URL listed above (for example: https://192.168.42.60:8443) in your default browser.  You will likely get a security warning from the browser about the certificate being from an unknown certificate authority. This is expected. You need to accept the certificate to proceed.

`I used firefox and it did not work so used Chromium instead.`

### Add the directory containing oc to your PATH

After the minishift VM has been started, 

#### add oc to your path:

$ eval $(minishift oc-env)

$ minishift status
Minishift:  Running
Profile:    minishift
OpenShift:  Running (openshift v3.11.0+82a43f6-231)
DiskUsage:  15% of 19G (Mounted On: /mnt/sda1)
CacheUsage: 2.243 GB (used by oc binary, ISO or cached images)


# setup env vars to docker
$ minishift docker-env

eval $(minishift docker-env)

# to oc
minishift oc-env

eval $(minishift oc-env)

# installed docker app on mac


* alternative to OKD is
CDK includes a prebuilt Red Hat Enterprise Linux virtual machine with OpenShift and other container tools pre-installed. 
