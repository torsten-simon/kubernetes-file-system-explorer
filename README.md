# kubernetes-file-system-explorer-editor README

View & Edit files from Kubernetes Pods
Add favorite files per pod for quicker access.

Important: This is a fork of the kubernetes-file-system-explorer https://github.com/sandipchitale/kubernetes-file-system-explorer


Get the extension for VSCode: https://marketplace.visualstudio.com/items?itemName=tsimones.kubernetes-file-system-explorer-editor

## Features

It supports the following commands:

| Command | Treenode Type| Description |
|---------|----------|-------------|
|`k8s.node.terminal` (Terminal)| Node |Start a shell into Node using nsenter. User must set the `kubernetes-file-system-explorer.nsenter-image` preference value to point to a `nsenter` image of choice e.g. `jpetazzo/nsenter:latest`.|
|`k8s.pod.container.terminal` (Terminal)| Container |Starts a shell in the Container. It basically runs:<br/>`kubectl exec -it pod-name -c containername -- sh`|
|`k8s.pod.container.folder.find` (Find)| Container Folder Node |Show the output of `find folderpath` in editor. It basically runs:<br/>`kubectl exec -it pod-name -c containername -- find /path/to/folder`|
|`k8s.pod.container.folder.ls-al` (ls -al)| Container Folder Node |Show the output of `ls -al folderpath` in editor. It basically runs:<br/>`kubectl exec -it pod-name -c containername -- ls -al /path/to/folder`|
|`k8s.pod.container.folder.cp-from` (kubectl cp from)| Container Folder Node |kubectl cp from FolderNode to a local folder|
|`k8s.pod.container.folder.cp-to-from-folder` (kubectl cp to from folder)| Container Folder Node |kubectl cp from a local folder to the FolderNode|
|`k8s.pod.container.folder.cp-to-from-file` (ls -al)| Container Folder Node |kubectl cp from a local file to the FolderNode|
|`k8s.pod.container.file.view` (View file)| Container File Node |Show the contents of the file in editor. It basically runs:<br/>`kubectl exec -it pod-name -c containername -- cat /path/to/file`|
|`k8s.pod.container.file.tail-f` (tail -f)| Container File Node |Tail -f the contents of the file in terminal. It basically runs:<br/>`kubectl exec -it pod-name -c containername -- tail -f /path/to/file`.<br/>That way you can tail arbitrary files.|
|`k8s.pod.container.file.cp-from` (kubectl cp from)| Container File Node |kubectl cp from FileNode to a local folder|

This extension adds tree nodes for the Kubernetes Init Containers, Containers and filesystem of the Kubernetes Containers under the Pod node in the Kubernetes Explorer View. Simply expands the treenode for a Pod to see it's Init Containers, Containers and Container filesystem. For example, the following screenshot shows the:

- the `haveged-445n7` pod
- the `haveged` container
- the `haveged` container's file system
- `View file` command in the context menu of `/etc/bash.bashrc` file
- `/etc/bash.bashrc` file content loaded in a editor tab

![Pod's filesystem](images/filesystem.png)


### How it works

It basically starts at `/` by running the following Kubectl command in the Pod to get the file listing:

`kubectl exec -it pod-name -c containername -- ls -f /`

As the tree nodes for directories are expanded e.g. `/etc/`, it basically runs:

`kubectl exec -it pod-name -c containername -- ls -f /etc/`

to get listing of files and created tree nodes for them.

## Requirements

This extension works with Microsoft Kubernetes extension.
