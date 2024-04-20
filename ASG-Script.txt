#===================
#Installation commands. Please remove #from below install commands

#amazon-linux-extras install epel -y
#yum install stress -y
#chmod +x asgload.sh			==> Adding Executable permissions
#./asgload.sh					==> running the script
#====================

#!/bin/bash

# Define the maximum CPU load percentage
max_load=90

# Define the step size to increase the load
step_size=30

# Define the duration to maintain the load at 90% (in seconds)
duration=1800  # 30 minutes

# Loop to gradually increase the CPU load
for load in $(seq 0 $step_size $max_load); do
  stress --cpu 1 --timeout 5s &   # Adjust parameters as needed
  sleep 300  # Wait for 30 seconds
  pkill stress
  echo "Increased load to $load%"
  if [ $load -eq $max_load ]; then
    break
  fi
done

# After reaching 90% load, maintain it for the specified duration
echo "Maintaining 90% load for the next 30 minutes..."
stress --cpu 1 --timeout $duration  # Adjust parameters as needed
