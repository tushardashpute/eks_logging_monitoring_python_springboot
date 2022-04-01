# eks_logging_monitoring_python_springboot
eks_logging_monitoring_python_springboot

Steps:
======
**1. Create EFK stack [ Elastiksearch/Fluentd/Kibana] for logging**

      git clone https://github.com/tushardashpute/efk_multiline_logs.git

      kubectl apply -f k8s/efk-stack.yaml

      # kubectl apply -f k8s/efk-stack.yaml
      namespace/kube-logging created
      service/elasticsearch created
      statefulset.apps/es-cluster created
      service/kibana created
      deployment.apps/kibana created
      serviceaccount/fluentd created
      clusterrole.rbac.authorization.k8s.io/fluentd created
      clusterrolebinding.rbac.authorization.k8s.io/fluentd created
      configmap/fluentd-config created
      daemonset.apps/fluentd created


      # kubectl get pods -n kube-logging
      NAME                      READY   STATUS    RESTARTS   AGE
      es-cluster-0              1/1     Running   0          4m11s
      fluentd-45ltd             1/1     Running   0          4m11s
      fluentd-kl6z5             1/1     Running   0          4m11s
      fluentd-wdm86             1/1     Running   0          4m11s
      kibana-84cf7f59c-jt6v4    1/1     Running   0          4m11s

      # kubectl get svc -n kube-logging
      NAME            TYPE           CLUSTER-IP      EXTERNAL-IP                                                               PORT(S)             AGE
      elasticsearch   ClusterIP      None            <none>                                                                    9200/TCP,9300/TCP   4m35s
      kibana          LoadBalancer   10.100.130.94   a69255d4d42ac4be3a9b2c972081315d-1189758535.us-east-2.elb.amazonaws.com   5601:31917/TCP      4m34s

      # kubectl get pv,pvc -n kube-logging
      NAME                                                        CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                            STORAGECLASS   REASON   AGE
      persistentvolume/pvc-61d3f28f-51a3-4847-9417-9e561072f16a   10Gi       RWO            Delete           Bound    kube-logging/data-es-cluster-0   gp2                     10s

      NAME                                      STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
      persistentvolumeclaim/data-es-cluster-0   Bound    pvc-61d3f28f-51a3-4847-9417-9e561072f16a   10Gi       RWO            gp2            15s


**2. Deploy Python App**

    git clone https://github.com/cloudtechmasters/sidecar-kubernetes-python-app.git
    kubectl apply -f deployment.yml
    kubectl apply -f service.yml

**3. Deploy Springboot App**

        git clone https://github.com/tushardashpute/eks_jenkins_pipiline.git

        k apply -f deployement.yml 
        k apply -f service.yml 

        # kubectl get pods
        NAME                                 READY   STATUS    RESTARTS   AGE
        spring-boot-hello-5c5fbbc5c4-6nv5m   1/1     Running   0          10m

        # kubectl get svc
        NAME                TYPE           CLUSTER-IP       EXTERNAL-IP                                                              PORT(S)          AGE
        kubernetes          ClusterIP      10.100.0.1       <none>                                                                   443/TCP          70m
        spring-boot-hello   LoadBalancer   10.100.126.158   a3cf606a63c2747b99b7b596a28adafa-601853560.us-east-2.elb.amazonaws.com   8080:30236/TCP   4s

    
**4. Created Prom/Grafana setup for monitiring**

     git clone https://github.com/tushardashpute/postgres-prom-integration-using-helm.git
     
     And follow the instrucation.
