## install
    git clone https://github.com/gfandada/tekton-demo.git
    cd ~/go/src/github.com/gfandada/tekton-demo
    kubectl apply -f tekton/init.yaml
    kubectl apply -f tekton/resources/
    ## 先配置your-docker-registry
    kubectl apply -f tekton/task/
    kubectl apply -f tekton/pipeline/
    ## 先配置镜像仓库的认证信息
    kubectl apply -f tekton/image-secret.yaml
    kubectl apply -f tekton/pipeline-sa.yaml
    ## 拉镜像的认证
    kubectl -n gfandada create secret docker-registry image-pull --docker-server=<your-docker-registry> --docker-username=<your-name> --docker-password=<your-pword>
    ## 执行整个cicd
    kubectl create -f tekton/run/
 
## result
   [root]# kubectl get pods -n gfandada -o wide -w
   NAME                                         READY   STATUS    RESTARTS   AGE  
   tekton-demo-7zwdj-ci-task-gw5vs-pod-07b014   0/2     Pending   0          0s    
   tekton-demo-7zwdj-ci-task-gw5vs-pod-07b014   0/2     Pending   0          0s    
   tekton-demo-7zwdj-ci-task-gw5vs-pod-07b014   0/2     Init:0/2   0          0s    
   tekton-demo-7zwdj-ci-task-gw5vs-pod-07b014   0/2     Init:1/2   0          1s    
   tekton-demo-7zwdj-ci-task-gw5vs-pod-07b014   0/2     PodInitializing   0          2s    
   tekton-demo-7zwdj-ci-task-gw5vs-pod-07b014   2/2     Running           0          3s    
   tekton-demo-7zwdj-ci-task-gw5vs-pod-07b014   2/2     Running           0          3s 
   tekton-demo-7zwdj-ci-task-gw5vs-pod-07b014   1/2     Running           0          7s  
   tekton-demo-7zwdj-ci-task-gw5vs-pod-07b014   0/2     Completed         0          2m21s   
   tekton-demo-7zwdj-cd-task-s82l6-pod-afaee3   0/3     Pending           0          0s      
   tekton-demo-7zwdj-cd-task-s82l6-pod-afaee3   0/3     Pending           0          0s     
   tekton-demo-7zwdj-cd-task-s82l6-pod-afaee3   0/3     Init:0/2          0          0s      
   tekton-demo-7zwdj-cd-task-s82l6-pod-afaee3   0/3     Init:1/2          0          1s     
   tekton-demo-7zwdj-cd-task-s82l6-pod-afaee3   0/3     PodInitializing   0          2s   
   tekton-demo-7zwdj-cd-task-s82l6-pod-afaee3   3/3     Running           0          10s    
   tekton-demo-7zwdj-cd-task-s82l6-pod-afaee3   3/3     Running           0          10s   
   tekton-demo-7zwdj-cd-task-s82l6-pod-afaee3   2/3     Running           0          13s     
   tekton-demo-7zwdj-cd-task-s82l6-pod-afaee3   0/3     Completed         0          14s     
   tekton-helloworld-go-fc72r-deployment-7fbff49755-4b6v7   0/2     Pending           0          0s     
   tekton-helloworld-go-fc72r-deployment-7fbff49755-4b6v7   0/2     Pending           0          0s      
   tekton-helloworld-go-fc72r-deployment-7fbff49755-4b6v7   0/2     ContainerCreating   0          0s      
   tekton-helloworld-go-fc72r-deployment-7fbff49755-4b6v7   1/2     Running             0          4s      
   tekton-helloworld-go-fc72r-deployment-7fbff49755-4b6v7   2/2     Running             0          5s      
    
   call app by my mac：
   $ time curl -H "Host: tekton-helloworld-go.gfandada.example.com" http://XXXXX
   hello world by gfandada@gmail.com!
   
   real	0m0.125s
   user	0m0.005s
   sys	0m0.009s
   
   