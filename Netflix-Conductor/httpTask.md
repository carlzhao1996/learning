# Netflix http workflow

## Conductor Client
main class
```
@SpringBootApplication
public class ConductorApplication {

    public static void main(String[] args) {
        //SpringApplication.run(ConductorApplication.class, args);
        TaskClient taskClient = new TaskClient();
        taskClient.setRootURI("http://localhost:8080/api/");

        int threadCount = 2;

        Worker worker1 = new MyWorker("mytask1");
        Worker worker2 = new MySecondWorker("mytask2");

        WorkflowTaskCoordinator.Builder builder = new WorkflowTaskCoordinator.Builder();
        WorkflowTaskCoordinator coordinator = builder.withWorkers(worker1,worker2).
        withThreadCount(threadCount).withTaskClient(taskClient).build();

        coordinator.init();

    }

}
```
第一个Worker
```
public class MyWorker implements Worker {
    private String taskDefName;

    public MyWorker(String taskDefName){
        this.taskDefName=taskDefName;
    }

    @Override
    public String getTaskDefName() {
        return taskDefName;
    }

    @Override
    public TaskResult execute(Task task) {
        System.out.printf("Executing %s\n",taskDefName);
        System.out.println("input: "+task.getInputData().get("inputVal"));
        TaskResult result = new TaskResult(task);
        result.setStatus(TaskResult.Status.COMPLETED);
        //Register the output of the task
        result.getOutputData().put("response: ",String.valueOf(task.getInputData().get("response")));
        return result;
    }
}
```
第二个Worker
```
public class MySecondWorker implements Worker{
    private String taskDefName;

    public MySecondWorker(String taskDefName){
        this.taskDefName=taskDefName;
    }

    @Override
    public String getTaskDefName() {
        return taskDefName;
    }

    @Override
    public TaskResult execute(Task task) {
        System.out.printf("Executing %s\n", taskDefName);
        System.out.println("bi:" + task.getInputData().get("bi"));
        TaskResult result = new TaskResult(task);
        result.setStatus(TaskResult.Status.COMPLETED);

        //Register the output of the task
        result.getOutputData().put("bo", String.valueOf(task.getInputData().get("bi")) + " from bo");
        return result;
    }
}
```
## Task 定义
URL POST
```
http://localhost:8080/api/metadata/taskdefs
```
BODY
```
[
    {
        "name": "mytask1",
        "description": "mytask1",
        "retryCount": 3,
        "timeoutSeconds": 1200,
        "inputKeys": [
            "inputVal"
        ],
        "timeoutPolicy": "TIME_OUT_WF",
        "retryLogic": "FIXED",
        "retryDelaySeconds": 600,
        "responseTimeoutSeconds": 800
    },
    {
        "name": "mytask2",
        "retryCount": 3,
        "timeoutSeconds": 1200,
        "inputKeys": [
            "bi"
        ],
        "outputKeys": [
            "bo"
        ],
        "timeoutPolicy": "TIME_OUT_WF",
        "retryLogic": "FIXED",
        "retryDelaySeconds": 600,
        "responseTimeoutSeconds": 800
    }
]
```

## workflow 定义
URL POST 
```
http://localhost:8080/api/metadata/workflow
```
Body
```
{
    "ownerApp": "string",
    "createTime": 0,
    "updateTime": 0,
    "createdBy": "string",
    "updatedBy": "string",
    "name": "myworkflow2",
    "description": "my http workflow for test",
    "version": 1,
    "tasks": [
        {
            "name": "mytask1",
            "taskReferenceName": "node1",
            "inputParameters": {
                "http_request": {
                    "uri": "http://localhost:8081/Cache/getCache",
                    "method": "GET"
                }
            },
            "type": "HTTP"
        },
        {
            "name": "mytask2",
            "taskReferenceName": "node2",
            "type": "SIMPLE",
            "inputParameters": {
                "bi": "${node1.output.response}"
            }
        }
    ],
    "outputParameters": {
        "bo": "${node2.output.bi}"
    },
    "schemaVersion": 2
}
```

## 运行Workflow
URL POST
```
http://localhost:8080/api/workflow/myworkflow2
```

BODY
```
{
    "inputVal":"test"
}
```