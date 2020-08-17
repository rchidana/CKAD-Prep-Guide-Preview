# CKAD-Prep-Guide-Preview

### An excerpt from my CKAD-Prep-Guide targeting Kubernetes Version 1.18.


# Pod Related Commands

### Kubernetes version 1.18:

	>alias k=kubectl
	>k cluster-info
	>k version

### Pod creation:

	>k create -f file-name
	>k apply -f file-name 
	>k apply -f file1 -f file2 -f file3
	>k apply -f ./dir

### Pod creating using Imperative Command

	>k run my-pod --image=busybox --dry-run=client --restart=Never -o yaml 
	-- sh -c 'echo "hello world";sleep 1d'

	--dry-run options -> client, server
	--restart options -> Never, OnFailure, Always
	-o yaml or -o json
	--port -> To expose any container port eg: --port=80
	--env="build-number=25" --env="env=stage"  --> multiple environment variables can be 
	specified with multiple --env options (comma seperated does not work!!)

	--labels="app=ui,env=prod" --> comma seperated values work

### start Pod with default command but using custom arguments
	>k run nginx --image=nginx -- <arg1> <arg2> <arg3>....

### start Pod with different (not default) command but using custom arguments
	>k run nginx --image=nginx --command -- <cmd> <arg1> <arg2> <arg3>

### Resource Requests
	--requests='cpu=100m,memory=256Mi'

### Resource Limits
	--limits='cpu=200m,memory=512Mi'

### Pod timeout (time to wait till at least one of the Pod gets running)
	--pod-running-timeout=1m5s or 10s


### Labels
Get pod with labels
	>k get po --show-labels
	>k get po -L <label-key1>,<label-key2>

### Getting Pods with specific label

	>k get po -l env=prod

### Adding a label to a running pod
	>k label pod my-pod <new-label>=<value1> <new-label2>=<value2>

### Overwriting a label of a running pod
	>k label pod my-pod <key>=<value> --overwrite

### Deleting a label on a running Pod
	>k label po my-pod <new-label-key>-

### Labeling nodes is very similar to Labeling Pods
	>k label node minikube env=prod
	>k get node --show-labels
	>k label node minikube env-
	>k label node minikube env=stage --overwrite

### Updating image on a running Pod
	>k run my-nginx --image=nginx:1.17
	>k set image po/my-nginx my-nginx=nginx:latest

### Replacing a running pod from a yaml file-name forcibly (deletes running pod)
	>k replace --force -f new-pod.yml

### Setting current context to modify default namespace to <name-space-of-choice>
	>k config set-context --current --namespace=<name-space-of-choice>


### Deleting all Objects in a Namespace
	> k delete all --all --namespace=<name-space-of-choice>

### Delete pods and services with same names "baz" and "foo"
	kubectl delete pod,service baz foo

### Delete pods and services with label name=myLabel.
	kubectl delete pods,services -l name=myLabel

### Delete a pod with minimal delay
	kubectl delete pod foo --now

### Force delete a pod on a dead node
	kubectl delete pod foo --force

### Delete all pods
  kubectl delete pods --all
  
### Automatic cleaning of Pods (upon exit)
Hint : similiar to Docker, there is --rm option that can be used to delete a 'foreground' pod!
But you need to use -it option to run the pod in the foreground

	kubectl run busybox -it --rm --image=busybox --restart=Never -- sh -c "echo 'hello world, I will be removed upon exit!!'"

### Max waiting period for a Pod to come up live, if not, terminate it automatically
Hint : activeDeadlineSeconds

### Get detailed explanation of any K8s Manifest file using explain command

	k explain pod --recursive
	#To get to know the apiVersion
	k explain pod --recursive | grep VERSION
	#To know about ports in a Pod
	k explain pod --recursive | grep -C5 ports
	#To know about any time related settings
	k explain pod --recursive | grep -i seconds

# Recommended .vimrc file setting so as to quickly edit your yaml file

  autocmd FileType yaml setlocal ts=2 sts=2 sw=2 expandtab
  
AutoIndent (ai) setting in the .vimrc file can create issues while copying & pasting contents from other sources onto vi editor. In such case, just type in "set paste" in the vi editor mode before pasting contents.

# Tmux Commands
In case you need to split your teminal into multiple sections, you can install tmux (on Ubuntu) using the command
 
  >sudo apt-get update
  >sudo apt-get install tmux
  
  #Bring up tmux using the command
  >tmux
  
To split the Screen Horizontally: Press CTLR+B and then hit "  
To split the Screen Vertically: Press CTLR+B and then hit %  
To Navigate between Screens : Press CTRL+B and then <- or -> or Up/Down Keys  
To exit tmux  
  >exit
