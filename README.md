# SPFaaS

This is the **official implementation** of *SPFaaS*. With *SPFaaS*, Developers can leverage its prediction module to accurately predict the invocation patterns of each function, including invocation concurrency and arriving time. Based on the prediction results, *SPFaaS* can implement more precise pre-warming and keep-alive strategies to alleviate function cold starts and enhance user satisfaction.

The project consists of two key components: the Prediction Module and the Decision Module. The Prediction Module is responsible for preprocessing workload data, probabilistic imputation, multiple rounds of sampling, and data prediction. The Decision Module has two parts. The first part involves modifications to the system framework, including container management and resource scheduling. The other part focuses on modifying the internal mechanisms of OpenFaaS.


---

## Environment

- docker: 18.09.9
- k8s: 1.18.4-00
- tomcat: 8.5.95
- Python: 3.10

---

## Workflow
- Install the OpenFaaS cluster.
- Obtain workload data.
  - Azure trace
  - Huawei Public Cloud trace
- Modify the internal mechanisms of OpenFaaS
- Train prediction model and execute data prediction
- Modify system framework of OpenFaaS and run the target system


---

## Key Components

---

### Modify the internal mechanisms of OpenFaaS: [internal_mechanisms](./decision_module/internal_mechanisms/)

```
$ cd faas-netes
$ kubectl apply -f namespaces.yml
$ kubectl -n openfaas create secret generic basic-auth \
    --from-literal=basic-auth-user=admin \
    --from-literal=basic-auth-password=admin
$ kubectl apply -f ./yaml/
$ curl -sL https://cli.openfaas.com | sh
$ echo export OPENFAAS_URL=<IP>:31112 >> ~/.bashrc
$ source ~/.bashrc
```

### Train prediction model and execute data prediction: [prediction_module](./prediction_module/)
```
$ bash ./exec.sh
```

### Modify system framework of OpenFaaS and run the target system: [system_framework](./decision_module/system_framework/)
```
$ git clone https://github.com/zyy010930/SPFaaS.git
$ /usr/share/maven/bin/mvn package -Dmaven.skip.test=true
$ mv /home/yourname/SPFaaS/target/loadGen.war /usr/local/tomcat/apache-tomcat-8.5.95/webapps
$ /usr/local/tomcat/apache-tomcat-8.5.95/bin/shutdown.sh
$ rm -rf /usr/local/tomcat/apache-tomcat-8.5.95/logs/catalina.out
$ /usr/local/tomcat/apache-tomcat-8.5.95/bin/startup.sh
$ vi /usr/local/tomcat/apache-tomcat-8.5.95/logs/catalina.out
```

---

### Collaborator: [Yuyang Zhu](https://github.com/zyy010930)