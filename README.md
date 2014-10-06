cruisecontrol.rb-plugins
========================

So far I've only one plugin ready which is based on
  https://github.com/javanthropus/cc.rb-plugins git_branch_trigger.rb

git_branch_changed_trigger.rb
-----------------------------

Created for building feature branches.
Main difference to git_branch_trigger.rb is that by using 
hash structure branch => sha1 it's able to 
detect change in branch that was added a long time ago.

### Installation:

- create special project called your_project-feature
- inside this project use custom build script that will switch branch to
  most recent one, eg:

        #!/bin/bash

        BRANCH=$(git for-each-ref --count=30 --sort=-committerdate refs/remotes/ --format='%(refname:short)'|head -n1|ruby -ne 'print $_.split("/").last')
        git checkout $BRANCH

        echo "Working on branch: $BRANCH"
        rake test

- inside cruise_config.rb add following:
   
        project.triggered_by GitBranchChangedTrigger.new(
          project,
          "git@gitlab:searchapi.git",
          ["origin/master"]
        )
  
  where last param is list of ignored branches. 
  If you use master/develop pass them here.
