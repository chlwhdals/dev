apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
  namespace: default  # 네임스페이스를 명시적으로 설정 (필요 시 변경)
  labels:
    app: myapp  # 추가적인 메타데이터를 포함
spec:
  replicas: 1  # Pod 복제본 수 (필요에 따라 조정)
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: 211.183.3.151:8443/project2/repository:donggill2
        resources:
          limits:
            memory: "64Mi"
            cpu: "250m"
          requests:  # 요청 리소스를 명시적으로 추가
            memory: "32Mi"
            cpu: "100m"
        ports:
        - containerPort: 80
      imagePullSecrets:
      - name: harbor-secret  # Secret 연결
---
apiVersion: v1
kind: Service
metadata:
  name: myapp
  namespace: default  # 네임스페이스를 명시적으로 설정 (Deployment와 일치)
spec:
  selector:
    app: myapp
  ports:
  - name: http  # 포트 이름 추가
    port: 80
    targetPort: 80
    nodePort: 30001  # 고정 NodePort
  type: NodePort
