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

### Using Triggers

You can add a GitHub webhook that will POST a JSON event your Webhook listener:
```
$ echo "URL: $(oc  get route webhook-listener --template='https://{{.spec.host}}')"
```

More information is available in the references.

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

[About triggers - Creating Applications With CICD pipelines](https://docs.openshift.com/container-platform/4.4/pipelines/creating-applications-with-cicd-pipelines.html#about-triggers_creating-applications-with-cicd-pipelines)