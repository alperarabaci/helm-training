# initials
```console
helm create coupon-service-chart

helm dependency update coupon-service-chart
```

after update files
```console
cd coupon-service-chart
helm install couponservice .
```

# connect to mysql
infos from values.yaml
```yaml
mysql:
  auth:
    rootPassword: test1234
    database: mydb
  primary:
    service: 
      type: NodePort
      nodePorts:
        mysql: 32762
  fullnameOverride: docker-mysql
  initdbScriptsConfigMap: mysql-initdb-config
```   

how to decide set this parameters? check mysql dependency primary/svc.yaml
```yaml
  ports:
    - name: mysql
      port: {{ .Values.primary.service.ports.mysql }}
      protocol: TCP
      targetPort: mysql
      {{- if (and (or (eq .Values.primary.service.type "NodePort") (eq .Values.primary.service.type "LoadBalancer")) .Values.primary.service.nodePorts.mysql) }}
      nodePort: {{ .Values.primary.service.nodePorts.mysql }}
      {{- else if eq .Values.primary.service.type "ClusterIP" }}
      nodePort: null
      {{- end }}
```
  
pass: test1234 //should be in vault.
port: 32762

```sql
INSERT INTO `coupon` (`id`,`code`,`discount`,`exp_date`) VALUES (2,'UDEMY_SALE',1.000,'01.01.2022');
```

# check endpoint
uses values.yaml for nodeport info
```yaml
 service:
    type: NodePort
    port: 9091
    targetPort: 9091
    nodePort: 32761
```

```console
curl http://localhost:32761/couponapi/coupons/UDEMY_SALE
```

# check probes

deployment.yaml
```yaml
    containers:
    - name: {{ .Chart.Name }}
        securityContext:
        {{- toYaml .Values.securityContext | nindent 12 }}
        image: "{{ .Values.image.repository }}:{{ .Values.image.tag | default .Chart.AppVersion }}"
        imagePullPolicy: {{ .Values.image.pullPolicy }}
        ports:
        - name: tomcat
            containerPort: 9091
            protocol: TCP
        livenessProbe:
        httpGet:
            path: /actuator/health/liveness
            port: tomcat
        initialDelaySeconds: 10
        failureThreshold: 5
        readinessProbe:
        httpGet:
            path: /actuator/health/readiness
            port: tomcat
        failureThreshold: 5
        initialDelaySeconds: 10
```

```console
curl http://localhost:32761/actuator/health/readiness
curl http://localhost:32761/actuator/health/liveness
```