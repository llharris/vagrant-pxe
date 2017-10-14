# vagrant-pxe

A simple Vagrantfile which builds a CentOS 7 based PXE Server and configures it to be able to PXEboot clients and install further CentOS 7 images using the CentOS mirror.

#### Usage

To use, generate an SSH keypair and modify the path to the private key file. Also modify the public_network IP for the pxe VM to match your local network.
