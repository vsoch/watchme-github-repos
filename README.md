# GitHub Repos

This is an example watcher to use [urls tasks](https://vsoch.github.io/watchme/watchers/urls) 
to monitor changes to GitHub repos (e.g., stars) using the [Version 3 API]. Each folder
within represents a GitHub repo we are watching:

 - [task-expfactory-expfactory](task-amazon)
 - [task-vsoch-scif](task-ebay)
 - [task-singularityhub-sregistry-cli](task-pusheencom)


### What is WatchMe?

WatchMe is a [tool for reproducible monitoring](https://vsoch.github.io/watchme).
This means that you can create one or more tasks to monitor web or system resources,
and collection version controlled results (git) at a regular interval (cron). 
For more details, see the [documentation base](https://vsoch.github.io/watchme)


## Clone

To clone this repository (and start with the watchers here), you can
first install watchme:

```bash
$ pip install watchme
```

and then get the repository:

```bash
              # repository                                      # watcher name
$ watchme get https://www.github.com/vsoch/watchme-github-repos github-repos
Added watcher github-repos
```

Conflrm that it was added:

```bash
$ watchme list
pusheen
github-repos
```

This will install the data to your $HOME/.watchme folder by default, unless
you've exported another `WATCHME_BASE_DIR`. Before you run the tasks, 
take a look at the data that has already been collected. For eack task
folder, you can export changes for a file like this 

```bash
#                <watcher>    <task>          <filename>
$ watchme export github-repos task-vsoch-scif result.json
```

If you expect the data in the files to be json (and want to parse it into the result)
then do this:

```bash
$ watchme export github-repos task-vsoch-scif result.json --json
```

There is a `TIMESTAMP` file that is kept in each folder as a record of when 
it was last run. You can then run the task manually in test mode to see output

```bash
$ watchme run github-repos --test
```

But likely you want to activate and schedule the task to run.


### Schedule the Task

Instead of a manual run, you likely want to run the task and look for changes 
over time. You can do that like this:

```bash
$ watchme schedule github-repos @daily
```

And then check that an entry has been added to crontab:

```bash
$ crontab -l
@daily /home/vanessa/anaconda3/bin/watchme run github-repos # watchme-github-repos
```

Finally, ensure that the watcher is active:

```bash
$ watchme activate github-repos
```

## Creation

If you want to reproduce creating this watcher (instead of cloning the repository), 
it looks something like this:

Install Watchme

```bash
$ pip install watchme
```

Initialize the base folder at $HOME/.watchme

```bash
$ watchme init
```

Create the pusheen watcher:

```bash
$ watchme create github-repos
Adding watcher /home/vanessa/.watchme/github-repos...
Generating watcher config /home/vanessa/.watchme/github-repos/watchme.cfg
```

And then install each of the tasks as follows:

```bash
$ watchme add github-repos task-expfactory url@https://api.github.com/repos/expfactory/expfactory save_as@json
$ watchme add github-repos task-vsoch-scif url@https://api.github.com/repos/vsoch/scif save_as@json
$ watchme add github-repos task-spython url@https://api.github.com/repos/singularityhub/singularity-cli save_as@json
$ watchme add github-repos task-sregistry url@https://api.github.com/repos/singularityhub/sregistry save_as@json
$ watchme add github-repos task-sregistry-cli url@https://api.github.com/repos/singularityhub/sregistry-cli save_as@json
$ watchme add github-repos task-singularity url@https://api.github.com/repos/sylabs/singularity save_as@json
$ watchme add github-repos task-spack url@https://api.github.com/repos/spack/spack save_as@json
```

After the original creation, you can easily export a current configuration file like this:


```bash
$ watchme inspect github-repos --add-command
```

## Export Data

You can export data as follows:

```bash
mkdir -p data
for repo in expfactory spack sregistry vsoch-scif singularity spython sregistry-cli
   do
       watchme export task-$repo result.json --out data/$repo.json
   done
```


