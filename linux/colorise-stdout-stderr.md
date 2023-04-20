# Customize Your Bash Terminal: Display Stdout in Green and Stderr in Red

## Introduction

A colorful terminal can significantly improve your command-line experience by making it more visually appealing and easier to read. In this blog post, we'll show you how to customize your Bash terminal to display stdout (standard output) in green and stderr (standard error) in red.

## Step 1: Create a Colorize Script

First, create a script (e.g., `colorize.sh`) with the following content:

```bash
#!/bin/bash

# Define color codes
GREEN=$(tput setaf 2)
RED=$(tput setaf 1)
RESET=$(tput sgr0)

# Redirect stdout and stderr through while loops that add color codes
while IFS= read -r line; do
    echo "${GREEN}${line}${RESET}"
done < <("$@" 2> >(while IFS= read -r line; do echo "${RED}${line}${RESET}" >&2; done))
```

Make the script executable:

```bash
chmod +x colorize.sh
```

Now, you can use this script to run your commands with colored output. For example:

```bash
./colorize.sh your_command_here
```

This will show stdout in green and stderr in red.

## Step 2: Add an Alias for Easy Usage

If you want to use this frequently, you can add an alias to your `~/.bashrc` or `~/.bash_aliases` file:

```bash
alias colorize="/path/to/your/colorize.sh"
```

Remember to replace `/path/to/your/` with the actual path to the `colorize.sh` script. After adding the alias, run `source ~/.bashrc` or `source ~/.bash_aliases` to reload your configurations. Now, you can use the `colorize` alias to run your commands:

```bash
colorize your_command_here
```

## Conclusion

With this simple script and alias, you can now enjoy a more visually appealing terminal experience. Your stdout will be displayed in green, and stderr will be shown in red, making it easier to distinguish between the two types of output.


Editors Note:
This was generated using GPT4 using the prompt `can I change my bash to show std out in green and std error in red?` and tested before publishing
