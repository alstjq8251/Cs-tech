## PersistentVolume

- 퍼시스턴트볼륨 서브시스템은 사용자 및 관리자에게 스토리지 사용 방법에서부터 스토리지가 제공되는 방법에 대한 세부 사항을 추상화하는 API를 제공한다.

### 프로비저닝

- PV를 프로비저닝 할 수 있는 두 가지 방법이 있다: 정적(static) 프로비저닝과 동적(dynamic) 프로비저닝

`동적 프로비저닝`

`정적 프로비저닝`

- 관리자가 생성한 정적 PV가 사용자의 퍼시스턴트볼륨클레임과 일치하지 않으면 클러스터는 PVC를 위해 특별히 볼륨을 동적으로 프로비저닝 하려고 시도할 수 있다.
- 이 프로비저닝은 스토리지클래스를 기반으로 한다.

### 바인딩

- 사용자는 원하는 특정 용량의 스토리지와 특정 접근 모드로 퍼시스턴트볼륨클레임을 생성하거나 동적 프로비저닝의 경우 이미 생성한 상태다

### PersistentVolume 종류

**임시**
- `emptyDir`

**로컬**
- `hostPath`
- `local`

**원격**
- `persistentVolumeClaim`, `cephfs`, `cinder`, `csi`, `fc(fibre channel)`, `flexVolume`, `flocker`, `glusterfs`, `iscsi`, `nfs`, `portworxVolume`, `quobyte`, `rbd`, `scaleIO`, `storageos`, `vsphereVolume` 

#### PV 보존정책

`Delete`

`Retain`

`Recycle`

### 볼륨 용량 제한하는 방법

`resourceQuota`

`limitRange`

