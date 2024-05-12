# Jobs and CronJob

## Pod

Take a case where you want a certain operation to be done. Post that we don't need to the pod. `Job` will do that for us. 

If you see a typical pod definition, it would look like this 

[pod-definition.yaml](Pod with Ubuntu image to caluculate sum)

Once the job is done the pod would change it's status to completion. Since, by default the *restartPolicy* of the pod would be `Always`, a new pod will be created and does the same thing and turns to `completion` status. The process would repeat until the max limit exceeds. 

Hence the `restartPoliciy` is modified to `Never`, to make sure to stop the pods from getting created repeatedly.

## Job

In a job you can define the no of pods you need with the value `spec: completions: 3`. With the value set, the job would spin 3 pods one after the other post their completion. 

If you want 3 of the pods to run concurrently, you can add new property `spec: parllelism: 3`. With this, the job would run 3 pods intitially and then stops the process until 3 pods complete their process. 

[job-definition.yaml](Job definition with pod definition)

## CronJob

If you want to run the job at a particular time, then you can define the job inside a cron job. It has a `schedule` field, where you can define the cron, which make sures to run the job on the specified time. 

[cronjob-definition.yaml](Cron Job definition using the job definition)
