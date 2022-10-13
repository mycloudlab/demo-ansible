# demo-ansible

kind: Pod
apiVersion: v1
metadata:
  generateName: automation-job-893-
  annotations:
    openshift.io/scc: restricted
  name: demo2
  namespace: ansible-automation-platform
spec:
  restartPolicy: Never
  serviceAccountName: default
  imagePullSecrets:
    - name: default-dockercfg-xpvrq
  priority: 0
  schedulerName: default-scheduler
  enableServiceLinks: true
  terminationGracePeriodSeconds: 30
  preemptionPolicy: PreemptLowerPriority
  nodeName: ip-10-0-134-32.us-east-2.compute.internal
  securityContext:
    seLinuxOptions:
      level: 's0:c26,c5'
    fsGroup: 1000660000
  containers:
    - resources:
        requests:
          cpu: 250m
          memory: 100Mi
      stdin: true
      terminationMessagePath: /dev/termination-log
      stdinOnce: true
      name: worker
      securityContext:
        capabilities:
          drop:
            - KILL
            - MKNOD
            - SETGID
            - SETUID
        runAsUser: 1000660000
      imagePullPolicy: IfNotPresent
      volumeMounts:
        - name: maven-storage
          mountPath: /maven/repository
      terminationMessagePolicy: File
      image: >-
        quay.io/rhn_support_csantana/ansible-executation-environment-java8-mvn-3:2.0
      args:
        - sleep
        - "80000"
  automountServiceAccountToken: false
  serviceAccount: default
  volumes:
    - name: maven-storage
      persistentVolumeClaim:
        claimName: ee-maven-repos
  