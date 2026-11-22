# Workflows - Running in each environment
This document describes how to set up and test/run a project in devlopment, test and production.


## Development
I use VSCode for this step.

### Initial configuration

.vscode/launch.json

{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        { "name": "Python Debugger: Module",
          "type": "debugpy",
          "request": "launch",
          "module": "main",
          "cwd": "${workspaceFolder}/src/tro_banking/",
          "justMyCode": true,
          "env": {
                "PYTHONPATH": "${workspaceFolder}/src/tro_banking/python"
            }   
        }
    ]
}

Configure any softlinks that need to point to the NFSStorage. 

Use "run without debugging" or "start debuging" in vscode to execute.


## Test

Go to the TestSwarm project and start the test_swarm.

```
cd ???
vagrant up
```


## Production
