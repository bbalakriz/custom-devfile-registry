# custom-devfile-registry

Take a look at the definition of `DevWorkspace` and `DevWorkSpaceTemplate` files contained in this repo under the loction `TP__indigo__dotnet-web-simple`.


#### In shell 1: 

```
$ podman run -it --rm --entrypoint=bash --user 1000 quay.io/devspaces/devfileregistry-rhel8:3.6
bash-4.4$ cd devfiles/
bash-4.4$ rm -rf TP__c* TP__g* TP__p*
```

#### In shell 2:

```
$ git clone <current_repo>
$ cd custom-devfile-registry
$ podman cp ./TP__indigo__dotnet-web-simple/ $(podman ps -q):/var/www/html/devfiles/
$ podman cp ./index.json $(podman ps -q):/var/www/html/devfiles/index.json 
$ podman commit --change='ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]' -c 'CMD ["/usr/local/bin/rhel.entrypoint.sh"]' $(podman ps -q) quay.io/balki404/devfileregistry:6.0
```
#### Use this custom devfile image in the CheCluster:

```
apiVersion: org.eclipse.che/v2
kind: CheCluster
metadata:
  name: devspaces
spec:
  components:
  ....
    devfileRegistry:
      deployment:
        containers:
          - image: '${NEW_REG_IMG}'
  ....
  ```
  
  #### Use the new custom stack in the user dashboard
  
  Log into Dev Spaces dashboard and launch a new workspace using the newly defined stack added (as specified in the steps on shell 2) into the devfile registry
