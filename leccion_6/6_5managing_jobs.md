# Los trabajos son diferentes a los pods

- Se puede ver como pods de duración limitada en vez de para siempre.

- Útiles para cosas como backups, cálculos, procesamiento en bloque.

- Un pod lanzado en un job necesita tener especificada la ```restartPolicy``` en:
    - OnFailure: Relanzar el contenedor en el mismo pod.
    - Never: Relanzar el contenedor en un nuevo pod.

- Hay tres tipos de jobs basados en los parámetros de ```completions```  y ```paralelism```.
    - Non parallel: Se lanza un pod, a menos que falle:
        - completions=1 / parallelism=1
    - Parallel con un número de veces que ha cumplirse:
        - jobs.specs.completions = n / parallelism = m
    - Parallel con cola de trabajo. Con terminar el primero ya está:
        - completions = 1 / parallelism = m
    
- Veamos el ejemplo de simplejob.yaml (Tiene el parámetro ```template``` que es una especificación de pods)

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl create -f materials/simplejob.yaml 
job.batch/simple-job created

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get jobs
NAME         COMPLETIONS   DURATION   AGE
simple-job   0/1           3s         3s

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get pods
NAME               READY   STATUS      RESTARTS   AGE
simple-job-ms8fq   0/1     Completed   0          28s

base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get jobs -o yaml
apiVersion: v1
items:
- apiVersion: batch/v1
  kind: Job
    .
    .
  spec:
    backoffLimit: 6
    completionMode: NonIndexed
    **completions: 1**
    **parallelism: 1**

- Ahora lo borramos y editamos:

apiVersion: batch/v1
kind: Job
metadata:
  name: simple-job
spec:
  **completions: 3**
  template:
    spec:

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get pods 
NAME               READY   STATUS      RESTARTS   AGE
simple-job-5k6z8   1/1     Running     0          7s
simple-job-hbb9q   0/1     Completed   0          15s
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get pods 
NAME               READY   STATUS      RESTARTS   AGE
simple-job-5k6z8   0/1     Completed   0          12s
simple-job-hbb9q   0/1     Completed   0          20s
simple-job-n246p   1/1     Running     0          4s
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get pods
NAME               READY   STATUS      RESTARTS   AGE
simple-job-5k6z8   0/1     Completed   0          24s
simple-job-hbb9q   0/1     Completed   0          32s
simple-job-n246p   0/1     Completed   0          16s
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get jobs
NAME         COMPLETIONS   DURATION   AGE
simple-job   3/3           24s        54s