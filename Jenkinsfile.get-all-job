def label = "worker-${UUID.randomUUID().toString()}"

podTemplate(label: label, containers: [
  containerTemplate(name: 'kubectl', image: 'seldonio/k8s-deployer:k8s_v1.10.0', command: 'cat', ttyEnabled: true),
],
volumes: [
  hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock')
]) {
  node(label) {
    stage('Run kubectl') {
      container('kubectl') {
        sh "kubectl version"
        sh "kubectl -n default get pods"
      }
    }
  }
}
