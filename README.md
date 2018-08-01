# How to recreate a complete git repository from the .git directory

## Why ?

if you have a git clone of a repo, you basically have the entire history
in the .git directory, plus a copy of the most recent file version of each
file.
in the worst case this means twice the disk space usage.

for archiving purposes it thus makes sense to store only the contents of .git
and leave the rest away, as it can later be recreated.

there's also the problem that huge git repos are nearly impossible to clone
with a slow internet connection, since there's no way to resume the download
once it fails (say you've downloaded 99% of a 2GB repo, and your connection goes
off - you have to download the whole 2 GB again).
in that case the most effective method to transfer the repo to your box is the
following:

- ssh into some dedicated server that has a good bandwith.
- clone the git repo there. the process probably just takes a few seconds,
  since most dedicated servers are behind GBit connections.
- cd into the clone and do `tar cf myproject.tar .git/`.
  it makes no sense to waste time using compression since the contents of
  .git are already compressed.
- put myproject.tar in some directory thats available via http.
- download the file via `wget -c myhost.com/myproject.tar`.
  wget can (and with `-c`, will) automatically resume the download when your
  connection goes away.

## How ?

Once you have the bare .git contents put into an empty directory whose name
describes the original projects name, and cd into it, git thinks that you
deleted all the checked out files.
so the only thing you have to do is to undo your changes:

- `git stash` followed by `git stash drop` and voila - the entire git repo
  is now as if freshly cloned with all files checked out.
