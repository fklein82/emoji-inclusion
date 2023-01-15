allow_k8s_contexts('tap-demo1-3')

SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='https://registry.fklein.me/myproject/workloads/inclusion-backend-api-dev:b5.20230115.225942')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='dev')

k8s_custom_deploy(
    'inclusion-backend-api',
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --live-update" +
               " --local-path " + LOCAL_PATH +
               " --source-image " + SOURCE_IMAGE +
               " --namespace " + NAMESPACE +
               " --yes >/dev/null" +
               " && kubectl get workload inclusion-backend-api --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes",
    deps=['pom.xml', './target/classes'],
    container_selector='workload',
    live_update=[
      sync('./target/classes', '/workspace/BOOT-INF/classes')
    ]
)

k8s_resource('inclusion-backend-api', port_forwards=["8080:8080"],
            extra_pod_selectors=[{'serving.knative.dev/service': 'inclusion-backend-api'}])