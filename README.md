# homebrew-update-agent
Installs a macOS Launch Agent and a simple bash script to update Homebrew formula daily.  
Check out [this](http://www.grivet-tools.com/blog/2014/launchdaemons-vs-launchagents/) great blog post to learn more about launch agents.  

## Installation
1. Clone or download the repository  
`git clone https://github.com/mitchjmac/homebrew-update-agent.git
`
2. Navigate to cloned directory  
`cd /path/to/clone/homebrew-update-agent`
3. Make the installer executable, in case it's not already  
`chmod +x ./install`
4. Run the installer with the desired options, see flags below  
`./install -p /path/to/plist -s /path/to/script -t time`
5. Test to see if the agent is running  
`launchctl list | grep com.github.homebrew_update_agent`  
Output should be either:  
`-	0	com.github.homebrew_update_agent` or  
`<pid>	0	com.github.homebrew_update_agent`  
The former indicates the agent is installed and the script ran correctly; the latter indicates the script is still running. Test until you recieve the former of the two. (Depending on how many packages need updating, this may take a few minutes.)


## Flags
\-p: launch agent plist directory *(default: ~/Library/LaunchAgents)*  
`./install -p </path/to/directory>`  
\-s: brew update script directory *(default: ~/bin)*  
`./install -s </path/to/directory>`  
\-t: integer time to preform the update every day *(default: 2:00am)*  
`./install -t <hour>`: hour[0-23]  
\-u: uninstall the launch agent (deletes files and unloads agent)  
`./install -u`


## Notes
### Install Script
- install
- Bash script which installs the plist, the update script, and loads/starts the agent.
- Props to [this](http://stackoverflow.com/questions/31968664/upgrade-all-the-casks-installed-via-homebrew-cask) stack exchage forum for help updating brew casks

### Launch Agent
- com.github.homebrew_update_agent.plist
- A plist that points to the shell script and describes how often the script should be run
- Can be installed in `/System/Library/LaunchAgents`, `/Library/LaunchAgents`, or `~/Library/LaunchAgents`
- Unless your homebrew installation is accessible to multiple users (aka if other users can add and remove packages) the recomendation is to install the plist in `~/Library/LaunchAgents`
- If the LaunchAgent is not installed in `~/Library/LaunchAgents` then the install script should be run with sudo

### Update Script
- homebrew_update
- Bash script which updates homebrew
- Becuase lauch agents cannot access your user's path, the path to brew must be hardcoded. The install script will alter the path to brew in this script to your brew installation.
