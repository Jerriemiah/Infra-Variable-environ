#  Mini Project Submission: Understanding Environment Variables & Infrastructure Environments

##  What I Learned

In this mini project, I explored and understood two important concepts in tech: **Infrastructure Environments** and **Environment Variables**. Although they sound similar, they are different and serve unique roles.

---

##  Infrastructure Environments (Where Code Lives)

Infrastructure environments are simply the **places where code is built, tested, and used by real people**. They help make sure that our code works correctly before it goes live.

### My Example Setup:

1. **Local (Development)**

   * A VirtualBox Ubuntu machine on my laptop
   * Where I write and test code by myself

2. **Testing (AWS Account 1)**

   * An EC2 server where the team tests the app
   * Helps find bugs without affecting real users

3. **Production (AWS Account 2)**

   * Live website on an EC2 server
   * Real customers use this version

Each one is its **own environment**, like different rooms in a house where different things happen.

---

##  Environment Variables (How Code Gets Info)

Environment variables are **key-value pairs** that help the script know which environment it's running in and what settings to use.

Instead of hardcoding things like database passwords, we use environment variables so that the same script can run in different places with the right settings.

###  Example:

| Environment | DB\_URL                   | DB\_USER      | DB\_PASS      |
| ----------- | ------------------------- | ------------- | ------------- |
| Local       | localhost                 | test\_user    | test\_pass    |
| Testing     | testing-db.example.com    | testing\_user | testing\_pass |
| Production  | production-db.example.com | prod\_user    | prod\_pass    |

Using these, we avoid writing sensitive info directly in the script, which is safer and more flexible.

---

##  Building the Shell Script

I created a script called `aws_cloud_manager.sh` to manage infrastructure based on the current environment.

---

###  Step 1: First Version with ENV Variable

I wrote the basic version that checks the `$ENVIRONMENT` variable.

```bash
#!/bin/bash

# Checking and acting on the environment variable
if [ "$ENVIRONMENT" == "local" ]; then
  echo "Running script for Local Environment ... "
elif [ "$ENVIRONMENT" == "testing" ]; then
  echo "Running script for Testing Environment ... "
elif [ "$ENVIRONMENT" == "production" ]; then
  echo "Running script for Production Environment ... "
else
  echo "No environment specified or recognized."
  exit 2
fi
```

To test it, I exported a variable like this:

```bash
export ENVIRONMENT=production
./aws_cloud_manager.sh
```

Output:

```
Running script for Production Environment ...
```

###  Screenshot 1: CLI showing environment variable set

<img width="808" height="637" alt="image" src="https://github.com/user-attachments/assets/edf94d1f-89c9-47f0-9d17-cce4886fd650" />

---

###  Problem With Hardcoding

If we set the environment inside the script like this:

```bash
ENVIRONMENT="testing"
```

Then the script **always runs as testing**, which is not dynamic.

---

###  Step 2: Use Command Line Arguments (Positional Parameters)

Instead of hardcoding, I learned to pass arguments to the script using **positional parameters** like `$1`, `$2`, etc.

Example call:

```bash
./aws_cloud_manager.sh production
```

In the script:

```bash
ENVIRONMENT=$1
```

This way, the script changes its behavior based on the argument given.

---

###  Step 3: Add Argument Validation

To make sure the user doesn’t forget to add the environment name, I added a check:

```bash
if [ "$#" -ne 1 ]; then
  echo "Usage: $0 <environment>"
  exit 1
fi
```

This means: if the user doesn’t pass exactly 1 argument, the script shows a helpful message.

---

###  Final Script:

```bash
#!/bin/bash

# Checking the number of arguments
if [ "$#" -ne 1 ]; then
  echo "Usage: $0 <environment>"
  exit 1
fi

# Get the environment name from the first argument
ENVIRONMENT=$1

# Run commands based on environment
if [ "$ENVIRONMENT" == "local" ]; then
  echo "Running script for Local Environment..."
elif [ "$ENVIRONMENT" == "testing" ]; then
  echo "Running script for Testing Environment..."
elif [ "$ENVIRONMENT" == "production" ]; then
  echo "Running script for Production Environment..."
else
  echo "Invalid environment specified. Please use 'local', 'testing', or 'production'."
  exit 2
fi
```

###  Screenshot 2: Final script
<img width="1015" height="635" alt="image" src="https://github.com/user-attachments/assets/c946ade1-79c5-4aa6-8ef9-f4351c8eec21" />

---

##  Summary

In this mini project, I learned:

* What **Infrastructure Environments** are (local, testing, production)
* What **Environment Variables** are and how they help
* How to create a dynamic **Shell Script** using:

  * Environment variables
  * Command-line arguments (`$1`, `$2`)
  * Validation for arguments
* How to write clean and safe scripts that work across different environments

This helps in building flexible and reusable tools when working in DevOps, scripting, or automation.

---


