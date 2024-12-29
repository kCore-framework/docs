# Jobs System Documentation

## Overview
The Jobs system provides a comprehensive framework for managing jobs, grades, and player assignments within the game environment. This documentation covers the core functions and their usage.

## Core Functions

### CreateJob
Creates a new job with specified grades and configuration.

```lua
Core.Functions.CreateJob(jobName, label, config, override)
```

**Parameters:**
- `jobName` (string): Unique identifier for the job
- `label` (string): Display name for the job
- `config` (table): Configuration table containing grades
- `override` (boolean): Optional flag to override existing job

**Config Structure:**
```lua
{
    grades = {
        {
            name = "Grade Name",
            salary = 1000,
            rank = 1
        }
    }
}
```

**Example:**
```lua
Core.Functions.CreateJob("police", "LSPD", {
    grades = {
        {
            name = 'Cadet',
            salary = 1000,
            rank = 1
        },
        {
            name = 'Officer',
            salary = 1500,
            rank = 2
        }
    }
})
```

### SetPlayerJob
Assigns a job and grade to a player.

```lua
Core.Functions.SetPlayerJob(source, job, grade)
```

**Parameters:**
- `source` (number): Player ID
- `job` (string): Job name
- `grade` (number): Grade rank

**Returns:**
- `boolean`: Success status

### GetJob
Retrieves job information.

```lua
Core.Functions.GetJob(job)
```

**Parameters:**
- `job` (string): Job name

**Returns:**
- `table`: Job information or `false` if not found

### GetPlayerJob
Retrieves a player's current job information.

```lua
Core.Functions.GetPlayerJob(source)
```

**Parameters:**
- `source` (number): Player ID

**Returns:**
- `table`: Player's job information or `false` if player not found

### GetAllJobs
Retrieves all registered jobs.

```lua
Core.Functions.GetAllJobs()
```

**Returns:**
- `table`: All registered jobs

## Command System

### /setjob
Sets a player's job and grade.

**Usage:**
```
/setjob [playerID] [job] [grade]
```

**Example:**
```
/setjob 1 police 2
```

### Debug Commands
- `/createPolice`: Creates the LSPD job with predefined grades
- `/createFire`: Creates the LSFD job with predefined grades

## Events

### kCore:syncJobs
Triggered when jobs are updated to synchronize client-side data.

**Parameters:**
- Jobs table containing all current jobs

### playerJoining
Triggers job synchronization when a player joins the server.

## Job Structure
```lua
{
    id = "jobName",
    label = "Display Name",
    grades = {
        [rank] = {
            name = "Grade Name",
            salary = amount,
            rank = position
        }
    }
}
```

## Error Handling

The system includes various error checks:
- Invalid job names or missing labels
- Invalid grade configurations
- Non-existent jobs or grades
- Missing or invalid parameters in commands

Error messages are displayed in the console or chat depending on the context.