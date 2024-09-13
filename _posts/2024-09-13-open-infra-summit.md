---
title: "OpenInfra Summit을 다녀온 후기"
excerpt: "infra"

categories:
  - infra
tags:
  - infra
classes: wide
last_modified_at: 2023-09-13T07:35:00-10:00
sidebar:
  nav: "infra"
---

다녀온 이후로 바로 후기를 작성하고 싶었지만, 잠시 일이 바빠서 후기를 작성하지 못했습니다.  

![Open Infra Summit](https://keepinmindsh.github.io/lines/assets/img/open_infra_conference.jpeg)

- 세미나 주제 : OpenInfra Summit Asia 2024 * OCP APAC Summit 2024 
- 일자 : 2024년 9월 3일 ~ 4일, 수원 

제가 Infra 관련 전문가는 아니기 때문에 좀더 흥미로운 주제가 많았다고 느낄 수도 있겠지만, 그래도 각 세션의 내용 자체는 고민해볼 만한 내용이 많았던것 같아요. 

제가 들었던 주제는 아래와 같았습니다. 

### 세미나 주제 

- Towards Kata Containers 4.0: Full Lifecycle GPU Management for AI/ML Workloads
- OpenStack Networking as scale with OpenSDN (Tungsten Fabric) for multi-AZ
- 100% Automation GitOps-Based Business Applications Using Sphinx, Zuul, and OpenStack
- The first exploration of "Beyonding Kubernetes"
- Implementing a Kubernetes Engine with Cluster-API and OpenStack / Kubernetes engine implemented with Cluster-api and Openstack 
- Migrating from VMware to OpenStack 
- How to not fail running OpenStack on Kubernetes at scale 
- New World : Cloud Paradigm Shift
- Battle at Edge with AI 
- OpenStack, GPUs, and Cost Optimizing for AI Workloads

내용들 중에서 생소한 개념들도 많았고, 새롭게 듣는 기술용어들도 많았습니다. 

### 해당 내용들 중에서 틈틈히 봐야겠다고 생각했던 부분들은, 

- OpenRan 
- OpenSDN
- OpenComputeProject 
- LoadBlancer base on EBPF 
- Autonomic Computing
- [KATA Containers](https://katacontainers.io/) 
- PCI passthrough 
- Sphinx : Open Source Search Server 
- Glue Code 
- EDSL: AI-Powered Research 
  - https://pypi.org/project/edsl/ 
- [Modular](https://www.modular.com/mojo)
- [AnyScale:LLM Running on Cloud](https://www.anyscale.com/pricing)
- OpenStack에서 주로 사용되는 것들 
  - Nova
  - Neutron 
  - Chider 
  - Swift
  - Glance
  - Keystone  
- [OpenSource MQTT Broker](https://mosquitto.org/)
- [Mirantis](https://www.mirantis.com/)

### 그리고 현재 사용하고 있는 쿠버네티스 관련해서는, 

- [WebAssembly 기반의 서버리스 쿠버네티스](https://www.spinkube.dev/)
- [WebAssembly 기반의 쿠버네티스](https://krustlet.dev/)
- [K3S](https://k3s.io/)
- [Kubernetes Cluster API](https://cluster-api.sigs.k8s.io/)
- [KubeEdge](https://kubeedge.io/)

### Openstack helm의 경우, 

openstack을 kubernetes환경에서 helm chart를 이용하여 배포할 수 있는 프로젝트입니다. Kubernetes의 기본 기능인  Self-Healing 을 비롯하여 Kubernetes의 많은 장점들을 이용하여 Openstack을 관리 하기 때문에 확장등의 라이프 사이클 관리에 도움이 되며 을 구축, 업그레이드, 확장등의 관리를 손쉽게 할 수 있습니다. 대부분 AT&T를 주도적으로 99cloud , Suse등과 함께 국내에서는 SK Telecom이 많은 기여 하고 있는 프로젝트입니다.

> [OpenStack Helm을 이용한 Kubernetes환경에서 OpenStack 배포](https://tech.osci.kr/openstack-helm%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-kubernetes%ED%99%98%EA%B2%BD%EC%97%90%EC%84%9C-openstack-%EB%B0%B0%ED%8F%AC/)  
> [What is the best way to deploy Openstack on Kubernetes?](https://www.reddit.com/r/openstack/comments/t4pil1/what_is_the_best_way_to_deploy_openstack_on/?rdt=58245)

### 그리고 흥미로웠던 것은 

VMWare/Windows 등을 Cloud 로 이전해줄 수 있는 기술을 제공하는 곳이였습니다.  

- [Coriolis](https://cloudbase.it/coriolis/)

### 현재 제공되는 Cloud Native Landscape를 보면서 트렌드를 파악하는 것도 좋구요 

- https://landscape.cncf.io/ 

잘 모르는 분야에 대해서 듣고, 그곳에서 인사이트를 얻는 것들은 도움이 많이 되는 것 같습니다. 위의 주제들은 앞으로 계속 공부해보려고 합니다.  