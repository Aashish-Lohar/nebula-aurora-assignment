## Steps to install helm chart and access in local cluster
1. build docker image and push it to your repository or load docker image in your local cluster, like for minkube
    `minikube image load my-image:latest`
2. In root location of folder nebula-arora-assignment, run 
    `helm dependency update wiki-chart`
3. Install helm chart
    `helm install wiki wiki-chart/`
4. Below is the ingress to refer the paths
```
{{- if .Values.ingress.enabled }}
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: {{ .Release.Name }}-ingress
  labels:
    {{- include "wiki-chart.labels" . | nindent 4 }}
  {{- with .Values.ingress.annotations }}
  annotations:
    {{- toYaml . | nindent 4 }}
  {{- end }}
spec:
  {{- if .Values.ingress.className }}
  ingressClassName: {{ .Values.ingress.className }}
  {{- end }}
  rules:
    - http:
        paths:
          - path: /grafana/
            pathType: Prefix
            backend:
              service:
                name: {{ .Release.Name }}-grafana
                port:
                  number: 80
                  
          - path: /user/
            pathType: Prefix
            backend:
              service:
                name: {{ .Release.Name }}-fastapi-service 
                port:
                  number: 8000
          - path: /posts/
            pathType: Prefix
            backend:
              service:
                name: {{ .Release.Name }}-fastapi-service 
                port:
                  number: 8000
          - path: /
            pathType: Prefix
            backend:
              service:
                name: {{ .Release.Name }}-fastapi-service 
                port:
                  number: 8000
                  
{{- end }}
```