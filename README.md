BashJect
====================

#### DISCLAIMER: This has only been tested on my machine! Please report bugs and comments ####

<bold>BashJect</bold> is a minimalist bash project manager. It allows you to switch your shell between different environnements. This is particularly useful for not polluting your main <em>bashrc</em>
with environnement variables, aliases and functions specific to one project.

It works in a very simple yet configurable way: 

*   a centralized file keeps a list of project identifiers and root paths (var: {BASHJECT\_PRJ\_STORE})
*   in each project root folder, a file is created (var: {BASHJECT\_PRJ\_FILE})
*   When switching to a project, this file is sourced

Random features:

* Allow a simple dependency system: just use a "virtual" project to gather all common stuff and use the <strong>source</strong> command to make it available in other projects
* Renaming / removing projects 
* <strong>list</strong> commands outputs project names separated by newlines (combine with <a href="http://tools.suckless.org/dmenu/">dmenu</a> to launch your favorite editor in the correct environment)
* Autocomplete included (for commands and args!)

Here's the help message:

    Usage:
            - Add a project with 'bashject add name path'
            - Enter a project with 'bashject set name'
            - Customize his environment with 'bashject edit name' (or editing his {BASHJECT_PRJ_DIR}/{BASHJECT_PRJ_FILE})
    
        Exported vars:
            - BASHJECT_PRJ_NAME: name of the project
            - BASHJECT_PRJ_PATH: path to the root of the project
    
        Commands:
            - bashject a|add      name path     # Adds a project
            - bashject s|set      name          # Enter a project
            - bashject source     name          # Source the project file of the specified project (useful to handle common envs)
            - bashject edit       name          # Edit the project file of the specified project with {EDITOR}
            - bashject rm|remove  name          # Remove the project (no filesystem modifications)
            - bashject rename     old    new    # Rename the project (no filesystem modifications)
            - bashject path       name          # Echoes a project's path
            - bashject names                    # Echoes all projects, separated by whitespaces
            - bashject list                     # Echoes all projects, separated by newlines (useful with dmenu, for example)
            - bashject cd                       # cd to the current project root path
            - bashject help       [cmd]         # Display global or specific command help
    
        Configuration (to override values, define them before sourcing this file)
            - BASHJECT_PRJ_STORE                # File in which we store all projects configuration  (default: '', current: '/home/ben/work/.projects')
            - BASHJECT_PRJ_FILE                 # Name of the project file, created at his root and sourced on 'bashject set'  (default: '', current: '.prj_env')
            - BASHJECT_CHANGE_PROMPT            # 1 to change the prompt to '[ BASHJECT_PRJ_NAME ] PS1' > on 'bashject set'  (default: '', current: '1')
            - BASHJECT_ALIAS                    # Name of an alias for bashject to define (i.e: 'bj'), autocompletion will be handled (default: '', current: 'bj')
            - BASHJECT_TMP                      # Temp file used for rename and remove operations (default: '', current: '/tmp/bashject.tmp')
            - BASHJECT_CD_WHEN_SET              # Change for project directory when set command is called (default: '1', current: '1')

To use it, just source it from your main <em>bashrc</em>:

    BASHJECT="$HOME/.bashject"
    [ -f "$BASHJECT" ] && source $BASHJECT

Example of a <a href="http://tools.suckless.org/dmenu/">dmenu</a> integration, to launch gvim in an enhanced environment:

    #!/bin/bash
    
    source "$HOME/.bashject"
    cmd=`bashject list | dmenu -i -p "Select Project " -nf '#888888' -nb '#222222' -sf '#ffffff' -sb '#285577'`
    if [ ! -z $cmd ]; then
        bashject set $cmd
        exec gvim .
    fi


