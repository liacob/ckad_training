# Entendiendo el contexto de seguridad

- Define privilegios y AC settings para un pod o container, incluye:
    - Permisos para accerder a un objeto
    - Security Enhanced Linus (SELinux), donde se aplican etiquetas de securidad
    - Diferencias privilegiado de no privilegiado
    - AppArmor (similar a SELinux)
    - Privilege escalation

- Va a nivel de spec en el yaml (materials/securitycontextdemo.yaml por ejemplo)

- Tras crearlo tenemos que:

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get pods
NAME          READY   STATUS                       RESTARTS        AGE
multipod      2/2     Running                      4 (3m59s ago)   24m
nginx         1/1     Running                      0               24m
nginxsecure   0/1     CreateContainerConfigError   0               8s

- Veamoslo con describe aunque haya fallado:

Warning  Failed     11s (x4 over 42s)  kubelet            Error: container has runAsNonRoot and image will run as root (pod: "nginxsecure_default(cb50c2ee-0564-4c34-a992-4c354059650b)", container: nginx)

