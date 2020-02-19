# Netflix Conductor dynamic Task

## Task 定义
URL POST
```
http://localhost:8080/api/metadata/taskdefs
```

BODY
```
[
    {
        "name":"user_task",
        "inputKeys":[
            "wi1",
            "wi2",
            "user_supplied_task"
        ]
    }
]
```

## Workflow 定义
URL POST
```
http://localhost:8080/api/metadata/workflow
```

BODY
```
{
    "name": "dynamicWorkflowTest",
    "version": 1,
    "tasks":[
        {
            "name": "user_task",
            "taskReferenceName": "t1",
            "inputParameters": {
                "wi1":"${workflow.input.wi1}",
                "wi2":"${workflow.input.wi2}",
                "taskToExecute": "${workflow.input.user_supplied_task}"
            },
            "type": "DYNAMIC",
            "dynamicTaskNameParam": "taskToExecute"
        }
    ]
}
```