  #https://github.com/wardviaene/kubernetes-course/blob/master/autoscaling/hpa-example.yml
  #https://github.com/kubernetes/website/issues/8173
  #kubectl run -i --tty load-generator --image=busybox /bin/sh
  # while true; do wget -q -O- http://hpa-example.default.svc.cluster.local:31001; done
  #kubectl get hpa
  #https://github.com/kubernetes-incubator/metrics-server
  #https://github.com/kubernetes-incubator/metrics-server/issues/41
  #kubectl describe hpa