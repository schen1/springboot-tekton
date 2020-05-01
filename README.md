# springboot-tekton

## Prerequisites
Tekton or OpenShift pipelines installed
[Helm](https://helm.sh/)
[Tekton CLI](https://github.com/tektoncd/cli) (Optional)

## Setup projects and rolebindings
```
$ oc create -f https://raw.githubusercontent.com/schen1/container-pipelines/feature/tekton/basic-spring-boot-tekton/.openshift/projects/projects.yml
```

## Install the pipelines using helm

```
$ helm upgrade --install springboot-pipelines . -n basic-spring-boot-build
```

## Run the pipeline
### Either using the oc command

```
$ oc create -f pipelinerun.yaml -n basic-spring-boot-build
```
### Or using tekton cli (version 0.9.0)
```
$ tkn pipeline start basic-spring-boot-pipeline \
    --resource basic-spring-boot-git=basic-spring-boot-git \
    --resource basic-spring-boot-templates=basic-spring-boot-templates \
    --resource basic-spring-boot-image=basic-spring-boot-image \
    --workspace name=local-maven-repo,claimName=maven-repo-pvc
```

## Support
Add PVC for [maven caching](https://developers.redhat.com/blog/2020/02/26/speed-up-maven-builds-in-tekton-pipelines/)

## Cleanup
```
$ oc delete project basic-spring-boot-build \
basic-spring-boot-dev \
basic-spring-boot-stage \
basic-spring-boot-prod
```

## References
[Container pipelines from Red Hat CoP](https://github.com/redhat-cop/container-pipelines/tree/master/basic-spring-boot-tekton)
[Maven caching](https://developers.redhat.com/blog/2020/02/26/speed-up-maven-builds-in-tekton-pipelines/)
[Tekton catalog](https://github.com/tektoncd/catalog)
[Pipeline start command](https://github.com/tektoncd/cli/blob/master/docs/reference/cmd/pipeline_start.md)
