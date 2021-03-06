## Uninstallation

During the beta period the preferred route of testing new beta code is to
redeploy the host machines.  We understand that is not always an automated
process so if you simply want to prepare a previously used host to test another
beta release these are the steps that can be followed on all machines:

~~~
yum erase "*openshift*"

# Erase all knowledge of the openshift units
systemctl reset-failed
systemctl daemon-reload

# Remove data
rm -rf /var/lib/openshift/
rm /etc/sysconfig/*openshift*
rm -rf /root/.kube
rm -rf /root/.config/openshift

# Remove all docker processes and images.  This is not strictly required but
# it's best to start from a clean slate.
docker rm -f $(docker ps -a -q)
docker rmi -f $(docker images -q)
yum erase "docker"
rm /etc/sysconfig/docker*

# Make sure you're running the latest RHEL 7.1 release
yum update

# This will clear the various virtual NICs
reboot
~~~
