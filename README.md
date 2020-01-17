## install
    git clone https://github.com/gfandada/tekton-demo.git
    cd ~/go/src/github.com/gfandada/tekton-demo
    kubectl apply -f tekton/init.yaml
    kubectl apply -f tekton/resources/
    kubectl apply -f tekton/task/
    kubectl apply -f tekton/pipeline/
    ## 先配置镜像仓库的认证信息
    kubectl apply -f tekton/image-secret.yaml
    kubectl apply -f tekton/pipeline-sa.yaml
    ## 拉镜像的认证
    kubectl -n gfandada create secret docker-registry image-pull --docker-server=registry.cn-chengdu.aliyuncs.com --docker-username=<your-name> --docker-password=<your-pword>
    ## 执行整个cicd
    kubectl create -f tekton/run/
    
