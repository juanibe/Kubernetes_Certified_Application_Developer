# DEMO - ENCODING DATA AT REST

[See](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/)

We will focus on how to encryt the data stored in the _etcd_ server.

Using the **etcdctl** command line tool, read that Secret out of etcd. If we don't have the tool installed we need to install it.

`ETCDCTL_API=3 etcdctl --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key get /registry secrets/default/secret1 | hexdump -C`

If we run that command we will see that the data is stored in an unencrypted format. To change that and be able to encrytp the first thing we need to do is to check if whether encryption at rest is enabled or not. To see that we can run the comand `ps -aux | grep kube-api | grep "encryption-provider-config"`. If we don't have a result, means the option is not configured.

### Creating configuration file

The first thing we have to do is to create a configuration file and pass it as an option with the flag `--encryption-provider-config`. (See file _encryption-configurationfile.yaml_). In the _providers_ part the one at the top of the list will be used. In this case is the one the provides no encrytpion at all.
Instead we could use another one at the top. (see file _encryption-aescbc-configurationfile.yaml_).
In each encryption algorithm you can choose a key and a secret. The secret is the one that is going to be used for the encryption

Once we have the file you will need to mount the new encryption config file to the kube-apiserver static pod. Here is an example on how to do that:

- Save the new encryption config file to `/etc/kubernetes/enc/enc.yaml` on the control-plane node.
- Edit the manifest for the kube-apiserver static pod: `/etc/kubernetes/manifests/kube-apiserver.yaml` so that it is similar to the file _manifest.yaml_
- Restart your API server.

We use _volume_ (local directory, the one that is in the local machine) and _volumeMounts_ (where the local directory is mapped to, and put it within the Pod) because we have the file locally. So everything that is avilable in the local in the specified path will be available in the Pod in the specified path.
