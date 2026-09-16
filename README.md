[Skills Network Labs.pdf](https://github.com/user-attachments/files/32285189/Skills.Network.Labs.pdf)
# KubernetesProject
[Skills Network Labs2.pdf](https://github.com/user-attachments/files/32285255/Skills.Network.Labs2.pdf)
# KubernetesProject

In this lab, you will:

- Scale an application with a ReplicaSet
- Apply rolling updates to an application
- Use a ConfigMap to store application configuration
- Autoscale the application using Horizontal Pod Autoscaler

theia@theiadocker-a422789255:/home/project $cd /home/project<br />theia@theiadocker-a422789255:/home/project$ [ ! -d 'CC201' ] && git clone https://github.com/ibm-developer-skills-network/CC201.git
Cloning into 'CC201'...
remote: Enumerating objects: 30, done.
remote: Counting objects: 100% (22/22), done.
remote: Compressing objects: 100% (16/16), done.
remote: Total 30 (delta 12), reused 6 (delta 6), pack-reused 8 (from 1)
Receiving objects: 100% (30/30), 8.71 KiB | 8.71 MiB/s, done.
Resolving deltas: 100% (13/13), done.
theia@theiadocker-a422789255:/home/project
theia@theiadocker-a422789255:/home/project
theia@theiadocker-a422789255:/home/project $cd CC201/labs/3_K8sScaleAndUpdate/<br />theia@theiadocker-a422789255:/home/project/CC201/labs/<br />3_K8sScaleAndUpdate$ ls
Dockerfile                         deployment.yaml
app.js                             package.json
deployment-configmap-env-var.yaml
theia@theiadocker-a422789255:/home/project/CC201/labs/3_K8sScaleAndUpdate $export MY_NAMESPACE=sn-labs-a422789255<br />theia@theiadocker-a422789255:/home/project/CC201/labs/<br />3_K8sScaleAndUpdate$ theia@theiadocker-a422789255:/hom
theia@theiadocker-a422789255:/home/project $cd /home/project<br />theia@theiadocker-a422789255:/home/project$ [ ! -d 'CC201' ] && git clone https://github.com/ibm-developer-skills-network/CC201.git
Cloning into 'CC201'...
remote: Enumerating objects: 30, done.
remote: Counting objects: 100% (22/22), done.
remote: Compressing objects: 100% (16/16), done.
remote: Total 30 (delta 12), reused 6 (delta 6), pack-reused 8 (from 1)
Receiving objects: 100% (30/30), 8.71 KiB | 8.71 MiB/s, done.
Resolving deltas: 100% (13/13), done.
theia@theiadocker-a422789255:/home/project $cd CC201/labs/3_K8sScaleAndUpdate/<br />theia@theiadocker-a422789255:/home/project/CC201/labs/<br />3_K8sScaleAndUpdate$ ls
Dockerfile                         deployment.yaml
app.js                             package.json
deployment-configmap-env-var.yaml
theia@theiadocker-a422789255:/home/project/CC201/labs/3_K8sScaleAndUpdate $export MY_NAMESPACE=sn-labs-a422789255<br />theia@theiadocker-a422789255:/home/project/CC201/labs/<br />3_K8sScaleAndUpdate$ docker build -t us.icr.io/$MY_NAMESPACE/hello-world:1 . && docker push us.icr.io/$MY_NAMESPACE/hello-world:1
[+] Building 12.9s (9/9) FINISHED      docker:default
=> [internal] load build definition from Docke  0.1s
=> => transferring dockerfile: 180B             0.0s
=> [internal] load metadata for docker.io/libr  1.1s
=> [internal] load .dockerignore                0.0s
=> => transferring context: 2B                  0.0s
=> [1/4] FROM docker.io/library/node:9.4.0-alp  6.2s
=> => resolve docker.io/library/node:9.4.0-alp  0.0s
=> => sha256:359a2efa481b9edeff9ca 951B / 951B  0.0s
=> => sha256:b5f94997f35f4d1ba 4.94kB / 4.94kB  0.0s
=> => sha256:605ce1bd3f3164f29 1.99MB / 1.99MB  1.1s
=> => sha256:fe58b30348fe37c 19.70MB / 19.70MB  3.2s
=> => sha256:46ef8987ccbdd5d2e 1.02MB / 1.02MB  0.8s
=> => extracting sha256:605ce1bd3f3164f2949a30  0.1s
=> => extracting sha256:fe58b30348fe37cda551e7  2.6s
=> => extracting sha256:46ef8987ccbdd5d2e0127b  0.1s
=> [internal] load build context                0.0s
=> => transferring context: 574B                0.0s
=> [2/4] COPY app.js .                          0.9s
=> [3/4] COPY package.json .                    0.0s
=> [4/4] RUN npm install &&    apk update &&    4.1s
=> exporting to image                           0.4s
=> => exporting layers                          0.4s
=> => writing image sha256:39e30ffef16ed6e913e  0.0s
=> => naming to us.icr.io/sn-labs-a422789255/h  0.0s

1 warning found (use docker --debug to expand):

- JSONArgsRecommended: JSON arguments recommended for CMD to prevent unintended behavior related to OS signals (line 8)
The push refers to repository [us.icr.io/sn-labs-a422789255/hello-world]
ac1984bfbdea: Pushed
fcba03b68e50: Pushed
96406a5099c1: Pushed
0804854a4553: Pushed
6bd4a62f5178: Pushed
9dfa40a0da3b: Pushed
1: digest: sha256:e0ecadbeaf6eefc984d486c0f7e0d7d2954baa20ccd731028c9cd4a6fbc9c803 size: 1576
theia@theiadocker-a422789255:/home/project/CC201/labs/
3_K8sScaleAndUpdate$ echo $MY_NAMESPACE
sn-labs-a422789255
theia@theiadocker-a422789255:/home/project/Ctheia@theiadoctheia@theiadocker-a422789255:/home/project/CC201/labs/
3_K8sScaleAndUpdate $kubectl apply -f deployment.yaml<br />deployment.apps/hello-world created<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ kubectl get pods<br />NAME READY STATUS RESTARTS AGE<br />hello-world-648b4c74cf-f69pt 1/1 Running 0 15s<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ kubectl expose deployment/hello-world<br />service/hello-world exposed<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ curl curl -L localhost:8001/api/v1/namespaces/sn-labs-a422789255/services/hello-world/proxy<br />Hello world from hello-world-648b4c74cf-f69pt! Your app is up and running!<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ kubectl scale deployment hello-world --replicas=3<br />deployment.apps/hello-world scaled<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ kubectl get pods<br />NAME READY STATUS RESTARTS AGE<br />hello-world-648b4c74cf-b257v 1/1 Running 0 7s<br />hello-world-648b4c74cf-bzgtw 1/1 Running 0 7s<br />hello-world-648b4c74cf-f69pt 1/1 Running 0 3m33s<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ for i in <code>seq 10</code>; do curl -L localhost:8001/api/v1/namespaces/sn-labs-a422789255/services/hello-world/proxy; done<br />Hello world from hello-world-648b4c74cf-f69pt! Your app is up and running!<br />Hello world from hello-world-648b4c74cf-f69pt! Your app is up and running!<br />Hello world from hello-world-648b4c74cf-f69pt! Your app is up and running!<br />Hello world from hello-world-648b4c74cf-bzgtw! Your app is up and running!<br />Hello world from hello-world-648b4c74cf-f69pt! Your app is up and running!<br />Hello world from hello-world-648b4c74cf-bzgtw! Your app is up and running!<br />Hello world from hello-world-648b4c74cf-b257v! Your app is up and running!<br />Hello world from hello-world-648b4c74cf-f69pt! Your app is up and running!<br />Hello world from hello-world-648b4c74cf-f69pt! Your app is up and running!<br />Hello world from hello-world-648b4c74cf-b257v! Your app is up and running!<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ kubectl scale deployment hello-world --replicas=1<br />deployment.apps/hello-world scaled<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ kubectl get pods<br />NAME READY STATUS RESTARTS AGE<br />hello-world-648b4c74cf-b257v 1/1 Terminating 0 85s<br />hello-world-648b4c74cf-bzgtw 1/1 Terminating 0 85s<br />hello-world-648b4c74cf-f69pt 1/1 Running 0 4m51s<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ kubectl get pods<br />NAME READY STATUS RESTARTS AGE<br />hello-world-648b4c74cf-f69pt 1/1 Running 0 5m18s<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />theia@theiadocker-a422789255:/home/project\$ \[ ! -d 'CC201' \] && git clone https://github.com/ibm-developer-skills-network/CC201.gitC201.gitps://github.com/ibm-developer-sk<br />Cloning into 'CC201'...<br />remote: Enumerating objects: 30, done.<br />remote: Counting objects: 100% (22/22), done.<br />remote: Compressing objects: 100% (16/16), done.<br />remote: Total 30 (delta 12), reused 6 (delta 6), pack-reused 8 (from 1)<br />Receiving objects: 100% (30/30), 8.71 KiB \| 8.71 MiB/s, done.<br />theia@theiadocker-a422789255:/home/project\$ cd CC201/labs/3\_K8sScaleAndUpdate/ate/9255:/home/project\$ cd CC201/la<br />theia@theiadocker-a422789255:/home/project/CC201/labs/<br />3\_K8sScaleAndUpdate\$ ls<br />DockeScaleAndUpdate\$ export MY\_NAMESPACE=sn-labs-a422789253\_K8sScaleAndUpdate\$ docker build -t us.icr.io/\$MY\_NAMESPACE/hello-world:1 . && docker push us.icr.io/$MY_NAMESPACE/hello-world:1-world:1. && docker push us.icr.io/$MY_NAMAME
[+] Building 12.9s (9/9) FINISHED docker:default
=> [internal] load build definition from Docke 0.1s
=> => transferring dockerfile: 180B 0.0s
=> [internal] load metadata for docker.io/libr 1.1s
=> [internal] load .dockerignore 0.0s
=> => transferring context: 2B 0.0s
=> [1/4] FROM docker.io/library/node:9.4.0-alp 6.2s
=> => resolve docker.io/library/node:9.4.0-alp 0.0s
=> => sha256:359a2efa481b9edeff9ca 951B / 951B 0.0s
=> => sha256:b5f94997f35f4d1ba 4.94kB / 4.94kB 0.0s
=> => sha256:605ce1bd3f3164f29 1.99MB / 1.99MB 1.1s
=> => sha256:fe58b30348fe37c 19.70MB / 19.70MB 3.2s
=> => sha256:46ef8987ccbdd5d2e 1.02MB / 1.02MB 0.8s
=> => extracting sha256:605ce1bd3f3164f2949a30 0.1s
=> => extracting sha256:fe58b30348fe37cda551e7 2.6s
=> => extracting sha256:46ef8987ccbdd5d2e0127b 0.1s
=> [internal] load build context 0.0s
=> => transferring context: 574B 0.0s
=> [2/4] COPY app.js . 0.9s
=> [3/4] COPY package.json . 0.0s
=> [4/4] RUN npm install && apk update && 4.1s
=> exporting to image 0.4s
=> => exporting layers 0.4s
=> => writing image sha256:39e30ffef16ed6e913e 0.0s
=> => naming to us.icr.io/sn-labs-a422789255/h 0.0s

1 warning found (use docker --debug to expand):

- JSONArgsRecommended: JSON arguments recommended for CMD to prevent unintended behavior related to OS signals (line 8)
The push refers to repository [us.icr.io/sn-labs-a422789255/hello-world]
ac1984bfbdea: Pushed
fcba03b68e50: Pushed
96406a5099c1: Pushed
0804854a4553: Pushed
6bd4a62f5178: Pushed
9dfa40a0da3b: Pushed
1: digest: sha256:e0ecadbeaf6eefc984d486c0f7e0d7d2954baa20ccd731028c9cd4a6fbc9c803 size: 1576
theia@theiadocker-a422789255:/home/project/CC201/labs/
3_K8sScaleAndUpdate$ echo $MY_NAMESPACE
sn-labs-a422789255
theia@theiadocker-a422789255:/home/project/CC201/labs/
3_K8sScaleAndUpdate $kubectl apply -f deployment.yaml<br />deployment.apps/hello-world created<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ kubectl get pods<br />NAME READY STATUS RESTARTS AGE<br />hello-world-648b4c74cf-f69pt 1/1 Running 0 15s<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ kubectl expose deployment/hello-world<br />service/hello-world exposed<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ curl curl -L localhost:8001/api/v1/namespaces/sn-labs-a422789255/services/hello-world/proxy<br />Hello world from hello-world-648b4c74cf-f69pt! Your app is up and running!<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ kubectl scale deployment hello-world --replicas=3<br />deployment.apps/hello-world scaled<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ kubectl get pods<br />NAME READY STATUS RESTARTS AGE<br />hello-world-648b4c74cf-b257v 1/1 Running 0 7s<br />hello-world-648b4c74cf-bzgtw 1/1 Running 0 7s<br />hello-world-648b4c74cf-f69pt 1/1 Running 0 3m33s<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ for i in <code>seq 10</code>; do curl -L localhost:8001/api/v1/namespaces/sn-labs-a422789255/services/hello-world/proxy; done<br />Hello world from hello-world-648b4c74cf-f69pt! Your app is up and running!<br />Hello world from hello-world-648b4c74cf-f69pt! Your app is up and running!<br />Hello world from hello-world-648b4c74cf-f69pt! Your app is up and running!<br />Hello world from hello-world-648b4c74cf-bzgtw! Your app is up and running!<br />Hello world from hello-world-648b4c74cf-f69pt! Your app is up and running!<br />Hello world from hello-world-648b4c74cf-bzgtw! Your app is up and running!<br />Hello world from hello-world-648b4c74cf-b257v! Your app is up and running!<br />Hello world from hello-world-648b4c74cf-f69pt! Your app is up and running!<br />Hello world from hello-world-648b4c74cf-f69pt! Your app is up and running!<br />Hello world from hello-world-648b4c74cf-b257v! Your app is up and running!<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ kubectl scale deployment hello-world --replicas=1<br />deployment.apps/hello-world scaled<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ kubectl get pods<br />NAME READY STATUS RESTARTS AGE<br />hello-world-648b4c74cf-b257v 1/1 Terminating 0 85s<br />hello-world-648b4c74cf-bzgtw 1/1 Terminating 0 85s<br />hello-world-648b4c74cf-f69pt 1/1 Running 0 4m51s<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ kubectl get pods<br />NAME READY STATUS RESTARTS AGE<br />hello-world-648b4c74cf-f69pt 1/1 Running 0 5m18s<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ docker build -t us.icr.io/\$MY\_NAMESPACE/hello-world:2 . && docker push us.icr.io/$MY_NAMESPACE/hello-world:2
[+] Building 4.3s (9/9) FINISHED docker:default
=> [internal] load build definition from Dockerfil 0.0s
=> => transferring dockerfile: 180B 0.0s
=> [internal] load metadata for docker.io/library/ 0.3s
=> [internal] load .dockerignore 0.0s
=> => transferring context: 2B 0.0s
=> CACHED [1/4] FROM docker.io/library/node:9.4.0- 0.0s
=> [internal] load build context 0.0s
=> => transferring context: 369B 0.0s
=> [2/4] COPY app.js . 0.0s
=> [3/4] COPY package.json . 0.0s
=> [4/4] RUN npm install && apk update && ap 3.4s
=> exporting to image 0.3s
=> => exporting layers 0.3s
=> => writing image sha256:e8e4d6d7238b5da21f2eb8b 0.0s
=> => naming to us.icr.io/sn-labs-a422789255/hello 0.0s

1 warning found (use docker --debug to expand):

- JSONArgsRecommended: JSON arguments recommended for CMD to prevent unintended behavior related to OS signals (line 8)
The push refers to repository [us.icr.io/sn-labs-a422789255/hello-world]
fcfe3a95c95d: Pushed
fcba03b68e50: Layer already exists
51d9f8ac8fbd: Pushed
0804854a4553: Layer already exists
6bd4a62f5178: Layer already exists
9dfa40a0da3b: Layer already exists
2: digest: sha256:a69a7ae814a23a7cb0ecf3000ef1572adc0ecb05cf5c9ce38e306ad2846b4283 size: 1576
theia@theiadocker-a422789255:/home/project/CC201/labs/3_K8
sScaleAndUpdate$ ibmcloud cr images
Listing images...

Repository                                                               Tag                                                                           Digest         Namespace            Created          Size     Security status
us.icr.io/sn-labs-a422789255/hello-world                                 1                                                                             e0ecadbeaf6e   sn-labs-a422789255   9 minutes ago    28 MB    -
us.icr.io/sn-labs-a422789255/hello-world                                 2                                                                             a69a7ae814a2   sn-labs-a422789255   22 seconds ago   28 MB    -
us.icr.io/sn-labsassets/categories-watson-nlp-runtime                    latest                                                                        6b01b1e5527b   sn-labsassets        3 years ago      3.1 GB   -
us.icr.io/sn-labsassets/classification-watson-nlp-runtime                latest                                                                        dbd407898549   sn-labsassets        3 years ago      4.0 GB   -
us.icr.io/sn-labsassets/concepts-watson-nlp-runtime                      latest                                                                        1e4741f10569   sn-labsassets        3 years ago      3.2 GB   -
us.icr.io/sn-labsassets/custom-watson-nlp-runtime                        latest                                                                        f6513e19a33d   sn-labsassets        3 years ago      6.5 GB   -
us.icr.io/sn-labsassets/detag-watson-nlp-runtime                         latest                                                                        38916c2119fc   sn-labsassets        3 years ago      2.7 GB   -
us.icr.io/sn-labsassets/emotion-watson-nlp-runtime                       latest                                                                        1c9de1d27318   sn-labsassets        3 years ago      4.0 GB   -
us.icr.io/sn-labsassets/entity-mentions-bert-watson-nlp-runtime          latest                                                                        57d92957214f   sn-labsassets        3 years ago      3.8 GB   -
us.icr.io/sn-labsassets/entity-mentions-bilstm-watson-nlp-runtime        latest                                                                        76dbd3bdb12b   sn-labsassets        3 years ago      2.9 GB   -
us.icr.io/sn-labsassets/entity-mentions-rbr-multi-watson-nlp-runtime     latest                                                                        577399d7b4e7   sn-labsassets        3 years ago      2.7 GB   -
us.icr.io/sn-labsassets/entity-mentions-rbr-watson-nlp-runtime           latest                                                                        506cc92ecd3f   sn-labsassets        3 years ago      2.7 GB   -
us.icr.io/sn-labsassets/entity-mentions-sire-watson-nlp-runtime          latest                                                                        cd4e48efd3f6   sn-labsassets        3 years ago      2.8 GB   -
us.icr.io/sn-labsassets/entity-mentions-transformer-watson-nlp-runtime   latest                                                                        0584c56563ce   sn-labsassets        3 years ago      3.8 GB   -
us.icr.io/sn-labsassets/instructions-splitter                            latest                                                                        2af122cfe4ee   sn-labsassets        5 years ago      21 MB    -
us.icr.io/sn-labsassets/istio-examples-bookinfo-details-v1               1.17.0                                                                        85819dbebb6d   sn-labsassets        4 years ago      62 MB    -
us.icr.io/sn-labsassets/istio-examples-bookinfo-productpage-v1           1.17.0                                                                        4c58e1fbe731   sn-labsassets        4 years ago      68 MB    -
us.icr.io/sn-labsassets/istio-examples-bookinfo-ratings-v1               1.17.0                                                                        f529526d2807   sn-labsassets        4 years ago      57 MB    -
us.icr.io/sn-labsassets/istio-examples-bookinfo-reviews-v3               1.17.0                                                                        0b37c5a396f7   sn-labsassets        4 years ago      415 MB   -
us.icr.io/sn-labsassets/keywords-watson-nlp-runtime                      latest                                                                        e2b9dc471ae0   sn-labsassets        3 years ago      2.7 GB   -
us.icr.io/sn-labsassets/lang-detect-watson-nlp-runtime                   latest                                                                        4d3b44e72af0   sn-labsassets        3 years ago      2.7 GB   -
us.icr.io/sn-labsassets/llmchatbot-demo                                  latest                                                                        b0c45851acbe   sn-labsassets        3 years ago      5.1 GB   -
us.icr.io/sn-labsassets/nginx                                            1.25.1                                                                        73e957703f12   sn-labsassets        3 years ago      71 MB    -
us.icr.io/sn-labsassets/nginx                                            latest                                                                        73e957703f12   sn-labsassets        3 years ago      71 MB    -
us.icr.io/sn-labsassets/nginx                                            sha256-85eabf2757cb5b5b84248d7feb019079501dfd8691fc79b8b1d0ff1591a6270b.sig   9a38ca67e16d   sn-labsassets        -                308 B    -
us.icr.io/sn-labsassets/nginx                                            signed                                                                        85eabf2757cb   sn-labsassets        3 years ago      67 MB    -
us.icr.io/sn-labsassets/nginx                                            unsigned                                                                      73e957703f12   sn-labsassets        3 years ago      71 MB    -
us.icr.io/sn-labsassets/noun-phrases-watson-nlp-runtime                  latest                                                                        c696f6af9797   sn-labsassets        3 years ago      2.7 GB   -
us.icr.io/sn-labsassets/openshift-console-auth-proxy                     0.0.1                                                                         f3cd15bd3584   sn-labsassets        6 years ago      355 MB   -
us.icr.io/sn-labsassets/pgadmin-theia                                    latest                                                                        0adf67ad81a3   sn-labsassets        5 years ago      101 MB   -
us.icr.io/sn-labsassets/phpmyadmin                                       latest                                                                        b66c30786353   sn-labsassets        5 years ago      163 MB   -
us.icr.io/sn-labsassets/relations-sire-watson-nlp-runtime                latest                                                                        65c2e74995d5   sn-labsassets        3 years ago      2.8 GB   -
us.icr.io/sn-labsassets/relations-transformer-watson-nlp-runtime         latest                                                                        18ffd6c35726   sn-labsassets        3 years ago      3.6 GB   -
us.icr.io/sn-labsassets/relations-watson-nlp-runtime                     latest                                                                        3547dcc15c43   sn-labsassets        3 years ago      3.6 GB   -
us.icr.io/sn-labsassets/sentiment-bert-watson-nlp-runtime                latest                                                                        b7f6814ca014   sn-labsassets        3 years ago      3.7 GB   -
us.icr.io/sn-labsassets/sentiment-cnn-watson-nlp-runtime                 latest                                                                        d89e9fbfccf1   sn-labsassets        3 years ago      2.8 GB   -
us.icr.io/sn-labsassets/sentiment-watson-nlp-runtime                     latest                                                                        a0b76bd2dad7   sn-labsassets        3 years ago      4.2 GB   -
us.icr.io/sn-labsassets/speech-standalone                                latest                                                                        635f94bc2c1c   sn-labsassets        3 years ago      3.7 GB   -
us.icr.io/sn-labsassets/syntax-watson-nlp-runtime                        latest                                                                        332c147eb437   sn-labsassets        3 years ago      2.7 GB   -
us.icr.io/sn-labsassets/tts-standalone                                   latest                                                                        56ce85280714   sn-labsassets        3 years ago      3.7 GB   -
us.icr.io/sn-labsassets/tts-standalone-full                              latest                                                                        0285cd4b20bd   sn-labsassets        3 years ago      5.4 GB   -
us.icr.io/sn-labsassets/watson-nlp-runtime                               1.0.18                                                                        0cbcbd5bde0e   sn-labsassets        3 years ago      2.7 GB   -
us.icr.io/sn-labsassets/watson-nlp_syntax_izumo_lang_en_stock            1.0.7                                                                         362ba4b6ef00   sn-labsassets        3 years ago      14 MB    -
us.icr.io/sn-labsassets/watson-nlp_syntax_izumo_lang_fr_stock            1.0.7                                                                         2d47c44882c7   sn-labsassets        3 years ago      14 MB    -

OK
theia@theiadocker-a422789255:/home/project/CC201/labs/3_K8
sScaleAndUpdate$ kubectl set image deployment/hello-world hello-world=us.icr.io/$MY_NAMESPACE/hello-world:2
deployment.apps/hello-world image updated
theia@theiadocker-a422789255:/home/project/CC201/labs/3_K8
sScaleAndUpdate $kubectl rollout status deployment/hello-world<br />deployment "hello-world" successfully rolled out<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ kubectl get deployments -o wide<br />NAME          READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS    IMAGES                                       SELECTOR<br />hello-world   1/1     1            1           10m   hello-world   us.icr.io/sn-labs-a422789255/hello-world:2   run=hello-world<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ \^C<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ curl curl -L localhost:8001/api/v1/namespaces/sn-labs-a422789255/services/hello-world/proxy<br />Welcome hello-world-f68c644b6-d2v97! Your app is up and running!<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ kubectl rollout undo deployment/hello-world<br />deployment.apps/hello-world rolled back<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />theia@theiadocker-a422789255:/home/project\$ \[ ! -d 'CC201' \] && git clone https://github.com/ibm-developer-skills-network/CC201.gitC201.gitps://github.com/ibm-developer-sk<br />Cloning into 'CC201'...<br />remote: Enumerating objects: 30, done.<br />remote: Counting objects: 100% (22/22), done.<br />remote: Compressing objects: 100% (16/16), done.<br />remote: Total 30 (delta 12), reused 6 (delta 6), pack-reused 8 (from 1)<br />Receiving objects: 100% (30/30), 8.71 KiB \| 8.71 MiB/s, done.<br />theia@theiadocker-a422789255:/home/project\$ cd CC201/labs/3\_K8sScaleAndUpdate/ate/9255:/home/project\$ cd CC201/la<br />theia@theiadocker-a422789255:/home/project/CC201/labs/<br />3\_K8sScaleAndUpdate\$ ls<br />DockeScaleAndUpdate\$ export MY\_NAMESPACE=sn-labs-a422789253\_K8sScaleAndUpdate\$ docker build -t us.icr.io/\$MY\_NAMESPACE/hello-world:1 . && docker push us.icr.io/$MY_NAMESPACE/hello-world:1-world:1. && docker push us.icr.io/$MY_NAMAME
[+] Building 12.9s (9/9) FINISHED      docker:default
=> [internal] load build definition from Docke  0.1s
=> => transferring dockerfile: 180B             0.0s
=> [internal] load metadata for docker.io/libr  1.1s
=> [internal] load .dockerignore                0.0s
=> => transferring context: 2B                  0.0s
=> [1/4] FROM docker.io/library/node:9.4.0-alp  6.2s
=> => resolve docker.io/library/node:9.4.0-alp  0.0s
=> => sha256:359a2efa481b9edeff9ca 951B / 951B  0.0s
=> => sha256:b5f94997f35f4d1ba 4.94kB / 4.94kB  0.0s
=> => sha256:605ce1bd3f3164f29 1.99MB / 1.99MB  1.1s
=> => sha256:fe58b30348fe37c 19.70MB / 19.70MB  3.2s
=> => sha256:46ef8987ccbdd5d2e 1.02MB / 1.02MB  0.8s
=> => extracting sha256:605ce1bd3f3164f2949a30  0.1s
=> => extracting sha256:fe58b30348fe37cda551e7  2.6s
=> => extracting sha256:46ef8987ccbdd5d2e0127b  0.1s
=> [internal] load build context                0.0s
=> => transferring context: 574B                0.0s
=> [2/4] COPY app.js .                          0.9s
=> [3/4] COPY package.json .                    0.0s
=> [4/4] RUN npm install &&    apk update &&    4.1s
=> exporting to image                           0.4s
=> => exporting layers                          0.4s
=> => writing image sha256:39e30ffef16ed6e913e  0.0s
=> => naming to us.icr.io/sn-labs-a422789255/h  0.0s

1 warning found (use docker --debug to expand):

- JSONArgsRecommended: JSON arguments recommended for CMD to prevent unintended behavior related to OS signals (line 8)
The push refers to repository [us.icr.io/sn-labs-a422789255/hello-world]
ac1984bfbdea: Pushed
fcba03b68e50: Pushed
96406a5099c1: Pushed
0804854a4553: Pushed
6bd4a62f5178: Pushed
9dfa40a0da3b: Pushed
1: digest: sha256:e0ecadbeaf6eefc984d486c0f7e0d7d2954baa20ccd731028c9cd4a6fbc9c803 size: 1576
theia@theiadocker-a422789255:/home/project/CC201/labs/
3_K8sScaleAndUpdate$ echo $MY_NAMESPACE
sn-labs-a422789255
theia@theiadocker-a422789255:/home/project/CC201/labs/
3_K8sScaleAndUpdate $kubectl apply -f deployment.yaml<br />deployment.apps/hello-world created<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ kubectl get pods<br />NAME READY STATUS RESTARTS AGE<br />hello-world-648b4c74cf-f69pt 1/1 Running 0 15s<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ kubectl expose deployment/hello-world<br />service/hello-world exposed<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ curl curl -L localhost:8001/api/v1/namespaces/sn-labs-a422789255/services/hello-world/proxy<br />Hello world from hello-world-648b4c74cf-f69pt! Your app is up and running!<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ kubectl scale deployment hello-world --replicas=3<br />deployment.apps/hello-world scaled<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ kubectl get pods<br />NAME READY STATUS RESTARTS AGE<br />hello-world-648b4c74cf-b257v 1/1 Running 0 7s<br />hello-world-648b4c74cf-bzgtw 1/1 Running 0 7s<br />hello-world-648b4c74cf-f69pt 1/1 Running 0 3m33s<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ for i in <code>seq 10</code>; do curl -L localhost:8001/api/v1/namespaces/sn-labs-a422789255/services/hello-world/proxy; done<br />Hello world from hello-world-648b4c74cf-f69pt! Your app is up and running!<br />Hello world from hello-world-648b4c74cf-f69pt! Your app is up and running!<br />Hello world from hello-world-648b4c74cf-f69pt! Your app is up and running!<br />Hello world from hello-world-648b4c74cf-bzgtw! Your app is up and running!<br />Hello world from hello-world-648b4c74cf-f69pt! Your app is up and running!<br />Hello world from hello-world-648b4c74cf-bzgtw! Your app is up and running!<br />Hello world from hello-world-648b4c74cf-b257v! Your app is up and running!<br />Hello world from hello-world-648b4c74cf-f69pt! Your app is up and running!<br />Hello world from hello-world-648b4c74cf-f69pt! Your app is up and running!<br />Hello world from hello-world-648b4c74cf-b257v! Your app is up and running!<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ kubectl scale deployment hello-world --replicas=1<br />deployment.apps/hello-world scaled<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ kubectl get pods<br />NAME READY STATUS RESTARTS AGE<br />hello-world-648b4c74cf-b257v 1/1 Terminating 0 85s<br />hello-world-648b4c74cf-bzgtw 1/1 Terminating 0 85s<br />hello-world-648b4c74cf-f69pt 1/1 Running 0 4m51s<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ kubectl get pods<br />NAME READY STATUS RESTARTS AGE<br />hello-world-648b4c74cf-f69pt 1/1 Running 0 5m18s<br />theia@theiadocker-a422789255:/home/project/CC201/labs/3\_K8<br />sScaleAndUpdate\$ docker build -t us.icr.io/\$MY\_NAMESPACE/hello-world:2 . && docker push us.icr.io/$MY_NAMESPACE/hello-world:2
[+] Building 4.3s (9/9) FINISHED docker:default
=> [internal] load build definition from Dockerfil 0.0s
=> => transferring dockerfile: 180B 0.0s
=> [internal] load metadata for docker.io/library/ 0.3s
=> [internal] load .dockerignore 0.0s
=> => transferring context: 2B 0.0s
=> CACHED [1/4] FROM docker.io/library/node:9.4.0- 0.0s
=> [internal] load build context 0.0s
=> => transferring context: 369B 0.0s
=> [2/4] COPY app.js . 0.0s
=> [3/4] COPY package.json . 0.0s
=> [4/4] RUN npm install && apk update && ap 3.4s
=> exporting to image 0.3s
=> => exporting layers 0.3s
=> => writing image sha256:e8e4d6d7238b5da21f2eb8b 0.0s
=> => naming to us.icr.io/sn-labs-a422789255/hello 0.0s

1 warning found (use docker --debug to expand):

- JSONArgsRecommended: JSON arguments recommended for CMD to prevent unintended behavior related to OS signals (line 8)
The push refers to repository [us.icr.io/sn-labs-a422789255/hello-world]
fcfe3a95c95d: Pushed
fcba03b68e50: Layer already exists
51d9f8ac8fbd: Pushed
0804854a4553: Layer already exists
6bd4a62f5178: Layer already exists
9dfa40a0da3b: Layer already exists
2: digest: sha256:a69a7ae814a23a7cb0ecf3000ef1572adc0ecb05cf5c9ce38e306ad2846b4283 size: 1576
theia@theiadocker-a422789255:/home/project/CC201/labs/3_K8
sScaleAndUpdate$ ibmcloud cr images
Listing images...

Repository                                                               Tag                                                                           Digest         Namespace            Created          Size     Security status
us.icr.io/sn-labs-a422789255/hello-world                                 1                                                                             e0ecadbeaf6e   sn-labs-a422789255   9 minutes ago    28 MB    -
us.icr.io/sn-labs-a422789255/hello-world                                 2                                                                             a69a7ae814a2   sn-labs-a422789255   22 seconds ago   28 MB    -
us.icr.io/sn-labsassets/categories-watson-nlp-runtime                    latest                                                                        6b01b1e5527b   sn-labsassets        3 years ago      3.1 GB   -
us.icr.io/sn-labsassets/classification-watson-nlp-runtime                latest                                                                        dbd407898549   sn-labsassets        3 years ago      4.0 GB   -
us.icr.io/sn-labsassets/concepts-watson-nlp-runtime                      latest                                                                        1e4741f10569   sn-labsassets        3 years ago      3.2 GB   -
us.icr.io/sn-labsassets/custom-watson-nlp-runtime                        latest                                                                        f6513e19a33d   sn-labsassets        3 years ago      6.5 GB   -
us.icr.io/sn-labsassets/detag-watson-nlp-runtime                         latest                                                                        38916c2119fc   sn-labsassets        3 years ago      2.7 GB   -
us.icr.io/sn-labsassets/emotion-watson-nlp-runtime                       latest                                                                        1c9de1d27318   sn-labsassets        3 years ago      4.0 GB   -
us.icr.io/sn-labsassets/entity-mentions-bert-watson-nlp-runtime          latest                                                                        57d92957214f   sn-labsassets        3 years ago      3.8 GB   -
us.icr.io/sn-labsassets/entity-mentions-bilstm-watson-nlp-runtime        latest                                                                        76dbd3bdb12b   sn-labsassets        3 years ago      2.9 GB   -
us.icr.io/sn-labsassets/entity-mentions-rbr-multi-watson-nlp-runtime     latest                                                                        577399d7b4e7   sn-labsassets        3 years ago      2.7 GB   -
us.icr.io/sn-labsassets/entity-mentions-rbr-watson-nlp-runtime           latest                                                                        506cc92ecd3f   sn-labsassets        3 years ago      2.7 GB   -
us.icr.io/sn-labsassets/entity-mentions-sire-watson-nlp-runtime          latest                                                                        cd4e48efd3f6   sn-labsassets        3 years ago      2.8 GB   -
us.icr.io/sn-labsassets/entity-mentions-transformer-watson-nlp-runtime   latest                                                                        0584c56563ce   sn-labsassets        3 years ago      3.8 GB   -
us.icr.io/sn-labsassets/instructions-splitter                            latest                                                                        2af122cfe4ee   sn-labsassets        5 years ago      21 MB    -
us.icr.io/sn-labsassets/istio-examples-bookinfo-details-v1               1.17.0                                                                        85819dbebb6d   sn-labsassets        4 years ago      62 MB    -
us.icr.io/sn-labsassets/istio-examples-bookinfo-productpage-v1           1.17.0                                                                        4c58e1fbe731   sn-labsassets        4 years ago      68 MB    -
us.icr.io/sn-labsassets/istio-examples-bookinfo-ratings-v1               1.17.0                                                                        f529526d2807   sn-labsassets        4 years ago      57 MB    -
us.icr.io/sn-labsassets/istio-examples-bookinfo-reviews-v3               1.17.0                                                                        0b37c5a396f7   sn-labsassets        4 years ago      415 MB   -
us.icr.io/sn-labsassets/keywords-watson-nlp-runtime                      latest                                                                        e2b9dc471ae0   sn-labsassets        3 years ago      2.7 GB   -
us.icr.io/sn-labsassets/lang-detect-watson-nlp-runtime                   latest                                                                        4d3b44e72af0   sn-labsassets        3 years ago      2.7 GB   -
us.icr.io/sn-labsassets/llmchatbot-demo                                  latest                                                                        b0c45851acbe   sn-labsassets        3 years ago      5.1 GB   -
us.icr.io/sn-labsassets/nginx                                            1.25.1                                                                        73e957703f12   sn-labsassets        3 years ago      71 MB    -
us.icr.io/sn-labsassets/nginx                                            latest                                                                        73e957703f12   sn-labsassets        3 years ago      71 MB    -
us.icr.io/sn-labsassets/nginx                                            sha256-85eabf2757cb5b5b84248d7feb019079501dfd8691fc79b8b1d0ff1591a6270b.sig   9a38ca67e16d   sn-labsassets        -                308 B    -
us.icr.io/sn-labsassets/nginx                                            signed                                                                        85eabf2757cb   sn-labsassets        3 years ago      67 MB    -
us.icr.io/sn-labsassets/nginx                                            unsigned                                                                      73e957703f12   sn-labsassets        3 years ago      71 MB    -
us.icr.io/sn-labsassets/noun-phrases-watson-nlp-runtime                  latest                                                                        c696f6af9797   sn-labsassets        3 years ago      2.7 GB   -
us.icr.io/sn-labsassets/openshift-console-auth-proxy                     0.0.1                                                                         f3cd15bd3584   sn-labsassets        6 years ago      355 MB   -
us.icr.io/sn-labsassets/pgadmin-theia                                    latest                                                                        0adf67ad81a3   sn-labsassets        5 years ago      101 MB   -
us.icr.io/sn-labsassets/phpmyadmin                                       latest                                                                        b66c30786353   sn-labsassets        5 years ago      163 MB   -
us.icr.io/sn-labsassets/relations-sire-watson-nlp-runtime                latest                                                                        65c2e74995d5   sn-labsassets        3 years ago      2.8 GB   -
us.icr.io/sn-labsassets/relations-transformer-watson-nlp-runtime         latest                                                                        18ffd6c35726   sn-labsassets        3 years ago      3.6 GB   -
us.icr.io/sn-labsassets/relations-watson-nlp-runtime                     latest                                                                        3547dcc15c43   sn-labsassets        3 years ago      3.6 GB   -
us.icr.io/sn-labsassets/sentiment-bert-watson-nlp-runtime                latest                                                                        b7f6814ca014   sn-labsassets        3 years ago      3.7 GB   -
us.icr.io/sn-labsassets/sentiment-cnn-watson-nlp-runtime                 latest                                                                        d89e9fbfccf1   sn-labsassets        3 years ago      2.8 GB   -
us.icr.io/sn-labsassets/sentiment-watson-nlp-runtime                     latest                                                                        a0b76bd2dad7   sn-labsassets        3 years ago      4.2 GB   -
us.icr.io/sn-labsassets/speech-standalone                                latest                                                                        635f94bc2c1c   sn-labsassets        3 years ago      3.7 GB   -
us.icr.io/sn-labsassets/syntax-watson-nlp-runtime                        latest                                                                        332c147eb437   sn-labsassets        3 years ago      2.7 GB   -
us.icr.io/sn-labsassets/tts-standalone                                   latest                                                                        56ce85280714   sn-labsassets        3 years ago      3.7 GB   -
us.icr.io/sn-labsassets/tts-standalone-full                              latest                                                                        0285cd4b20bd   sn-labsassets        3 years ago      5.4 GB   -
us.icr.io/sn-labsassets/watson-nlp-runtime                               1.0.18                                                                        0cbcbd5bde0e   sn-labsassets        3 years ago      2.7 GB   -
us.icr.io/sn-labsassets/watson-nlp_syntax_izumo_lang_en_stock            1.0.7                                                                         362ba4b6ef00   sn-labsassets        3 years ago      14 MB    -
us.icr.io/sn-labsassets/watson-nlp_syntax_izumo_lang_fr_stock            1.0.7                                                                         2d47c44882c7   sn-labsassets        3 years ago      14 MB    -

OK
theia@theiadocker-a422789255:/home/project/CC201/labs/3_K8
sScaleAndUpdate$ kubectl set image deployment/hello-world hello-world=us.icr.io/$MY_NAMESPACE/hello-world:2
deployment.apps/hello-world image updated
theia@theiadocker-a422789255:/home/project/CC201/labs/3_K8
sScaleAndUpdate$ kubectl rollout status deployment/hello-world
deployment "hello-world" successfully rolled out
theia@theiadocker-a422789255:/home/project/CC201/labs/3_K8
sScaleAndUpdate$ kubectl get deployments -o wide
NAME          READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS    IMAGES                                       SELECTOR
hello-world   1/1     1            1           10m   hello-world   us.icr.io/sn-labs-a422789255/hello-world:2   run=hello-world
theia@theiadocker-a422789255:/home/project/CC201/labs/3_K8
sScaleAndUpdate$ ^C
theia@theiadocker-a422789255:/home/project/CC201/labs/3_K8
sScaleAndUpdate$ curl curl -L localhost:8001/api/v1/namespaces/sn-labs-a422789255/services/hello-world/proxy
Welcome hello-world-f68c644b6-d2v97! Your app is up and running!
theia@theiadocker-a422789255:/home/project/CC201/labs/3_K8
sScaleAndUpdate$ kubectl rollout undo deployment/hello-world
deployment.apps/hello-world rolled back
theia@theiadocker-a422789255:/home/project/CC201/labs/3_K8
sScaleAndUpdate$ kubectl rollout status deployment/hello-world
deployment "hello-world" successfully rolled out
theia@theiadocker-a422789255:/home/project/CC201/labs/3_K8
sScaleAndUpdate$ kubectl get deployments -o wide
NAME          READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS    IMAGES                                       SELECTOR
hello-world   1/1     1            1           12m   hello-world   us.icr.io/sn-labs-a422789255/hello-world:1   run=hello-world
theia@theiadocker-a422789255:/home/project/CC201/labs/3_K8
sScaleAndUpdate$

---

In this practice lab, you will:

- Build and deploy an application to Kubernetes
- Implement Vertical Pod Autoscaler (VPA) to adjust pod resource requests/limits
- Implement Horizontal Pod Autoscaler (HPA) to scale the number of pod replicas based on resource utilization
- Create a Secret and update the deployment for using it
