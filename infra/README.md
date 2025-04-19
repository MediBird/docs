# CaringNote Infra README

## CaringNote Infra 환경

* kakao cloud intance 1대 내에 단일 node로 k8s로 구성함

### instance 정보

* t1i.xlarge
* CPU
  * v4
* RAM
  * 16GB
* SSD
  * 1024GB

### Infra 디렉터리 구성

```sh
📦 caring-note-deployment
├── common
│   ├── cluster-issuer.yaml         # cert‑manager ClusterIssuer 설정
│   ├── get_helm.sh                 # Helm Chart 설치 스크립트
│   ├── helm_list.sh                # 클러스터 기본 환경 설치 스크립트 ⭐️⭐️
│   ├── ingress-values.yaml         # nginx‑ingress 관련 매니페스트
│   ├── keycloak-values.yaml        # Keycloak Helm 매니페스트
│   └── network-policy.yaml         # (미사용) NetworkPolicy 예시
│
├── postgresql                      # PostgreSQL Helm 차트
│   ├── .helmignore
│   ├── Chart.lock
│   ├── Chart.yaml
│   ├── README.md
│   ├── charts/common
│   ├── templates
│   ├── values.schema.json
│   └── values.yaml                # PostgreSQL 매니페스트
│
├── pvc
│   └── postgresql-pv.yaml          # PV/PVC 정의 (PostgreSQL 데이터용)
│
├── prod                       # 프로덕션 배포용 매니페스트 ⭐️⭐️⭐️
│   ├── api.yaml               # caring-note-api Deployment/Service
│   ├── ingress.yaml           # caringnote-ingress 설정
│   └── web.yaml               # caring-note-web Deployment/Service
│
└── staging                    # 스테이징 배포용 매니페스트 ⭐️⭐️⭐️
    ├── api.yaml
    ├── ingress.yaml
    └── web.yaml
```

### K8S cluster 구성 방법

```sh
## k8s는 각 환경에 맞게 사전 설치 되어 있다고 가정한다.

git clone https://github.com/MediBird/caring-note-deployment.git
cd ./caring-note-deployment

## helm chart 설치
./get_helm.sh

## k8s caring-note 기본 환경 설정
./helm_list.sh
```

### K8S cluster 배포 구성도

* caringNote 는 SingleNode로 구성되어 있음.

![deployment](./assets/deployment.png)



### K8S cluster 네트워크 구성도

![network](./assets/network.png)

### 주의사항

* 현재 caring note는 단일 인스턴스 사용하고 있어
  load balancer(ex elb) 사용하고 있지 않음.

  * 이경우 ingress-value.yaml에 아래와 같이 NodePort로 설정함.

    * 만약 load balancer 사용하는 경우 type을 LoadBalancer로 설정해야함.

    ```yaml
        labels: {}
        # -- Type of the external controller service.
        # Ref: https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types
        type: NodePort ## 507 line
        # -- Pre-defined cluster internal IP address of the external controller service. Take care of collisions with existing services.
        # This value is immutable. Set once, it can not be changed without deleting and re-creating the service.
        # Ref: https://kubernetes.io/docs/concepts/services-networking/service/#choosing-your-own-ip-address
        
    ```
