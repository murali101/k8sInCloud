## (Kubernetes)K8s Setup in cloud

Launch multiple instances on AWS/any cloud by selecting any linux OS  
   
    --> give proper number cores based on size of the instance 
     
        --> make sure select default group or communication between the instances having no issues
     
        --> Login into the instances from the terminal
        
            --> install Docker (all instances) : (https://docs.docker.com/engine/install/ubuntu/)
                  1. curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
                     or
                     curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
                  2. sudo apt-get update
                  3. sudo apt-get install docker-ce
            --> Install K8s (all instances) : https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/
                  1. sudo apt-get install -y apt-transport-https ca-certificates curl
                  2. sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
                     or 
                     sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
                  3. echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
                  4. sudo apt-get update
                  5. sudo apt-get install -y kubelet kubeadm kubectl
                  6. sudo apt-mark hold kubelet kubeadm kubectl // Optional
                  
                update network tables: (all instances)
                  1. echo "net.bridge.bridge-nf-call-iptables=1" | sudo -a /etc/sysctl.conf
                  2. sudo sysctl -p
                  
          --> Creating Cluster
                on master node:
                    1. sudo kubeadm init --pod-network-cidr=10.244.0.0/16 
                        ==> gives the kubeadm command --> save this for future to run on child instances

                        a. mkdir -p $HOME/.kube
                        b. sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
                        c. sudo chown $(id -u):$(id -g) $HOME/.kube/config
                    2. kubectl apply -f https://github.com/coreos/flannel/raw/master/Documentation/kube-flannel.yml 
                    3. kubectl get nodes
                on worker nodes:
                    1. run the kubeadm command from the master node console printed one
                    
                    
                    
        Validation:
            --> go to master node and execute the command "kubectl get nodes"
            --> deploy any pod with multiple replicate set and test it.
                  
