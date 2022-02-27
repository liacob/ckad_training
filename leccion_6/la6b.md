# Formas de afrontarlo

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl create cronjob --help 
Create a cron job with the specified name.

Aliases:
cronjob, cj

Examples:
  # Create a cron job
  kubectl create cronjob my-job --image=busybox --schedule="*/1 * * * *"
  
  # Create a cron job with a command
  kubectl create cronjob my-job --image=busybox --schedule="*/1 * * * *" -- date

>>  kubectl create cronjob sleep-job --image=busybox --schedule="*/2 * * * *"  --dry-run -o yaml > lab6_2.yaml