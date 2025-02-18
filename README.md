# awsctx

`awsctx` is your new best friend for managing AWS profiles. Think of it as the `kubectx` of AWS, but without the Kubernetes-induced existential crisis. It’s a lightweight Bash/Zsh function that helps you list, switch, and manage AWS profiles like a pro. No more fumbling around with `export AWS_PROFILE`—let `awsctx` do the heavy lifting for you.

---

## Features

- **List Profiles**: Quickly see all your AWS profiles with a single command.
- **Highlight Current Profile**: Because knowing where you are is half the battle.
- **Switch Profiles**: Change AWS profiles with ease. No eval, no drama.
- **Color-Coded Output**: Because plain text is for amateurs.
- **Zero Dependencies**: Just Bash or Zsh and the AWS CLI. If you don’t have those, we need to have a serious talk.

---

## Installation

### Step 1: Add the Function to Your Shell Config

Copy the `awsctx` function below and paste it into your shell configuration file (`~/.bashrc` or `~/.zshrc`):

````bash
# Simple AWS Profile Switcher
function awsctx() {
    # Colors
    local BLUE=$'\033[0;34m'
    local YELLOW=$'\033[1;33m'
    local NC=$'\033[0m'

    # If no arguments, list profiles
    if [ -z "$1" ]; then
        echo "${BLUE}Available profiles:${NC}"
        aws configure list-profiles | nl -w2 -s') '
        echo "${BLUE}Current profile:${NC} ${YELLOW}${AWS_PROFILE:-default}${NC}"
        return
    fi

    # Switch to specified profile
    if aws configure list-profiles | grep -q "^${1}$"; then
        export AWS_PROFILE="$1"
        echo "${BLUE}Switched to profile:${NC} ${YELLOW}${AWS_PROFILE}${NC}"
    else
        echo "${YELLOW}Profile not found: $1${NC}"
        return 1
    fi
}
````


### Step 2: Reload Your Shell Config

After adding the function, reload your shell configuration to make it available in your current session:

````bash
source ~/.bashrc  # For Bash users
source ~/.zshrc   # For Zsh users
````


### Step 3: Verify Installation

Run the following command to ensure `awsctx` is working:

````bash
awsctx
````


If you see a list of your AWS profiles and the current profile highlighted, congratulations—you’re ready to roll.

---

## Usage

### List Profiles

To list all available AWS profiles and highlight the current one, simply run:

````bash
awsctx
````


Example output:

````
Available profiles:
  1) default
  2) dev
  3) prod
Current profile: dev
````


---

### Switch Profiles

To switch to a specific profile, pass the profile name as an argument:

````bash
awsctx <profile_name>
````


Example:

````bash
awsctx prod
````


Output:

````
Switched to profile: prod
````


You can verify the current profile by running:

````bash
echo $AWS_PROFILE
````


Output:

````
prod
````


---

### Notes

- **AWS CLI Required**: This function uses the `aws configure list-profiles` command, so make sure the AWS CLI is installed and configured on your system.
- **Default Profile**: If no `AWS_PROFILE` is set, the `default` profile is used.
- **Shell Compatibility**: Works with Bash and Zsh. If you’re using Fish, well, you’re swimming upstream.

---

## Troubleshooting

### Error: "Profile not found"

If you see this error, it means the profile you’re trying to switch to doesn’t exist. Double-check the profile name by running:

````bash
awsctx
````


If the profile is missing, make sure it’s defined in your AWS configuration file (`~/.aws/config`). You can add it using the AWS CLI:

````bash
aws configure --profile <profile_name>
````


---

## Uninstallation

Decided you don’t need `awsctx` anymore? (We’ll miss you.) Here’s how to remove it:

1. Open your shell configuration file (`~/.bashrc` or `~/.zshrc`):
   ```bash
   nano ~/.bashrc  # or ~/.zshrc
   ```

2. Delete the `awsctx` function.

3. Reload your shell configuration:
   ```bash
   source ~/.bashrc  # or source ~/.zshrc
   ```

---

## Example Output

### Listing Profiles

````bash
awsctx
````


Output:

````
Available profiles:
  1) default
  2) dev
  3) prod
Current profile: dev
````


---

### Switching Profiles

````bash
awsctx prod
````


Output:

````
Switched to profile: prod
````


---

## Final Words

That's much better than installing a new tool, right?

