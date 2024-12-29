# Jobs System Documentation

## Server-Side Implementation


### Core Job Structure

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

### Available Server Functions

#### CreateJob
Creates or updates a job in the system.

```lua
Core.Functions.CreateJob(jobName, label, config, override)
```

* Validates job configuration and grade structure
* Automatically adjusts rank numbering to start from 0
* Broadcasts updates to all clients via 'kCore:syncJobs'
* Returns the created job object or errors on invalid input

#### SetPlayerJob
Updates a player's job assignment.

```lua
Core.Functions.SetPlayerJob(source, job, grade)
```

* Updates player's job data:
  * Job name
  * Grade level
  * Salary
  * Grade label
* Automatically saves the updated data
* Returns true on success, false on failure

#### GetJob
Retrieves job configuration.

```lua
Core.Functions.GetJob(job)
```

* Returns the full job configuration or false if not found
* Includes all grades and metadata

#### GetPlayerJob
Retrieves a player's current job information.

```lua
Core.Functions.GetPlayerJob(source)
```

* Returns the player's current job data or false if player not found

#### GetAllJobs
Retrieves all registered jobs.

```lua
Core.Functions.GetAllJobs()
```

* Returns the complete Shared.Jobs table

### Job Synchronization

Jobs are synchronized to clients through the following events:

* `kCore:syncJobs`: Broadcasts job updates to all clients

## Client-Side Implementation

### Player Job Access

The client can access job information through the `Core.Player.Job` structure after loading:

```lua
Core.Player.Job = {
    name = "jobName",         -- Current job identifier
    grade = gradeNumber,      -- Current grade number
    salary = amount,          -- Current salary
    grade_label = "Grade Name" -- Current grade display name
}
```

### Job State Management

* Job data is synchronized automatically when:
  * The player first joins the server
  * Any job configurations are updated
  * The player's job is changed

### Checking Job Information

Example usage in client scripts:

```lua
-- Check if player has a specific job
if Core.Player.Job.name == "police" then
    -- Police-specific code
end

-- Check job grade
if Core.Player.Job.grade >= 2 then
    -- Higher rank specific code
end

-- Access salary information
local currentSalary = Core.Player.Job.salary

-- Get grade label
local gradeTitle = Core.Player.Job.grade_label
```

### Best Practices

1. Always check if the player is loaded before accessing job data:
```lua
if Core.Player and Core.Player.IsLoaded then
    -- Access job data here
end
```

2. Use job grade comparisons for permission checks:
```lua
if Core.Player.Job.grade >= requiredGrade then
    -- Allow access to feature
end
```

3. Listen for job updates if running long-term operations:
```lua
AddEventHandler('kCore:syncJobs', function(jobs)
    -- Update local job cache if needed
end)
```

### Command Examples

The system includes built-in commands for testing and administration:

* `/setjob [playerID] [job] [grade]`: Sets a player's job and grade
* `/createPolice`: Creates the LSPD job with predefined grades
* `/createFire`: Creates the LSFD job with predefined grades

These commands provide useful examples of job system interaction and can be used as templates for custom implementation.