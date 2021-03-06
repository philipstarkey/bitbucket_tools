# bitbucket tools

random tools for dealing with bitbucket.org repos. Created as https://community.atlassian.com/t5/Bitbucket-articles/What-to-do-with-your-Mercurial-repos-when-Bitbucket-sunsets is a very long thread and difficult to search.

This repo contains dump_repos.py which will dump most things to local file system for offline backup. It does NOT import or convert it.

Getting my notes off my machine and in a shareable state.

TL;DR check out:

  * https://bitbucket.org/edrandall/bitbucket-hg-to-git/
  * https://magnushoff.com/blog/kick-the-bitbucket/
  * https://docs.gitlab.com/ee/user/project/import/bitbucket.html
  * or https://github.com/clach04/bitbucket_tools/blob/master/dump_repos.py which will export everything (project meta data, logo/avatar, issue text, download metadata and downloads) but not import/push it anywhere


## Tools

### bitbucket-cli - 2019

Updated version that works as of 2019, with REST v2.0 API https://developer.atlassian.com/bitbucket/api/2/reference/resource/

https://bitbucket.org/arrizza/bitbucket-cli/src/master/

Also includes a tool convert_to_git (recommend bitbucket-hg-to-git instead)

### bitbucket-hg-to-git

https://bitbucket.org/edrandall/bitbucket-hg-to-git

creates new BitBucket git repos from mercurial ones. Handles some meta data/tags -  handles meta data link description and private repos.

### bitbucket-hg-exporter

UNTESTED/partially tested - claims to handle:

> pull request history, issue trackers, wikis, forks, attachments and commit comments

https://github.com/philipstarkey/bitbucket-hg-exporter

Large histories take a long time, one (Jython 2.x) project I had was running for a day and was still chugging away. Handled import of a small project fairly well.

Appeared to handle code (branches) and project meta data.

  * Wiki not handled at all.
  * Downloads untested so far.
  * Avatar/icons untested so far (failed large project did appear to persist to local disk)
  * Issues handled BUT prompts before import (can only import once, unlike specific issue migration tools below) so this requires interaction.
 
Update 7th April 2020: v0.7.0 released - claims to decrease the download time. Can also use a local hg->git conversion tool (such as [hg-export-tool](https://github.com/chrisjbillington/hg-export-tool) instead of the GitHub source importer which has better handling of UTF8 charcters, author attributions and mercurial branches with multiple heads. I have not yet tested these improvements.

### kick the bitbucket escape-bitbucket

UNTESTED - appears to handle private? Partially handles wiki

https://magnushoff.com/blog/kick-the-bitbucket/

### bitbucket-backup

UNTESTED - appears to be code and wiki only, appears to handle OAuth. Based on readme https://github.com/clach04/bitbucket_tools/blob/master/dump_repos.py is more complete if login/password is a viable option for export.

https://github.com/samkuehn/bitbucket-backup

### BBMigrate

UNTESTED - shell scripts to dump/export to local file system. May handle Pull Requests - looks like its missing most metadata.
WARNING does not appear to handle downloads based on readme
https://bitbucket.org/Rarified/bbmigrate/src/master/


### bitbucket-issues-to-github

UNTESTED

https://github.com/fkirc/bitbucket-issues-to-github


### bitbucket-issue-migration

UNTESTED

https://github.com/jeffwidman/bitbucket-issue-migration


### .hgignore to .gitignore

UNTESTED

Perl script - not sure how complete
https://github.com/devzendo/hgignore-to-gitignore

### Maven POM updates 

UNTESTED

https://github.com/devzendo/bitbucket-to-github-pom-scm



### Sync - Git-hg Mirror

UNTESTED

https://githgmirror.com/. It can keep a git and hg repo (or two hg or two git repos) in sync in one direction or bidirectionally. It's also open source and you can run it yourself too. Only deals with code, so no wiki or issues.


## Migrating away from BitBucket

Lots of options, none complete. https://medium.com/collaborne-engineering/how-to-migrate-a-private-repository-from-bitbucket-to-github-6cddedd5d73 is a good overview on manually migrating **code only**.

GitHub's code/history import https://github.com/new/import worked well for me to import and convert a Mercurial code repo. It does not handle:
* project description - needs to manually copy/pasted 
* project logo/avatar/image  - needs to manually set (note you may not be able to get the original image that was used to upload, only a scaled-down copy unless you have the original stored somewhere)
* Issues / history / attachments
* PRs / history
* Downloads
* wiki

https://docs.gitlab.com/ee/user/project/import/bitbucket.html  claims to support issues (unclear about attachments), PRs, and wiki. does NOT list downloads.

### Migration tools

 * https://magnushoff.com/blog/kick-the-bitbucket/ - bash/shell script for moving to GitHub
      * https://gist.github.com/maghoff/2b86e4ca3208a5ce443b35c5860c8a4d
  * https://bitbucket.org/edrandall/bitbucket-hg-to-git - BitBucket to BitBucket git.

### Migration items

#### project description

needs to manually copy/pasted 

    curl https://api.bitbucket.org/2.0/repositories/clach04/jython/


#### project logo/avatar/image

WIP can this be scrapped?

avatar (two of them) in meta data is available (redirects, usually to AWS) but is small version cropped version and larger-uncropped (proabbly not original size)

    curl https://api.bitbucket.org/2.0/repositories/clach04/jython/

needs to manually set (note you may not be able to get the original image that was used to upload, only a scaled-down copy unless you have the original stored somewhere)

#### Issues / history / attachments

WIP

https://confluence.atlassian.com/bitbucket/export-or-import-issue-data-330797432.html?_ga=2.39865826.1751265516.1572798592-1434613483.1567279842

  * https://github.com/fkirc/bitbucket-issues-to-github
  * https://github.com/jeffwidman/bitbucket-issue-migration


From https://www.reddit.com/r/mercurial/comments/elu3m7/migrating_from_bitbucket_to_github/

> arganoid
> 
> I have now tried the second script. Overall I would say the first is quicker and easier, as with the second one you have to export issues via the Bitbucket interface, and also generate an API key from Github. The first script just uses your Bitbucket and GitHub passwords (although that could be a security risk if you don't trust the script).
> 
> I noticed the issues brought across by the first script retained their timestamps whereas the second one saves issues with the current time, albeit with a note in the text indicating the original date


    curl https://api.bitbucket.org/2.0/repositories/clach04/jython/issues  # no issues example

use V2 api to see if there issues at all

#### PRs / history


#### Downloads

WIP

meta data and list:

https://developer.atlassian.com/bitbucket/api/2/reference/resource/repositories/%7Bworkspace%7D/%7Brepo_slug%7D/downloads#get

    curl https://api.bitbucket.org/2.0/repositories/clach04/dava33display/downloads  # no downloads
    curl https://api.bitbucket.org/2.0/repositories/clach04/jython/downloads

each download:

https://developer.atlassian.com/bitbucket/api/2/reference/resource/repositories/%7Bworkspace%7D/%7Brepo_slug%7D/downloads/%7Bfilename%7D

where name is in array from above

    curl -S -L https://api.bitbucket.org/2.0/repositories/clach04/jython/downloads/jython251hacked_nopydoc.jar --output jython251hacked_nopydoc.jar


#### wiki

WIP anyway to create initial wiki page? No API, potentially could scrap/robot webpage

wiki declared in meta data with `has_wiki` attribute (see "project description").

Create an initial page and then force push the new git repo. The wiki content will need to be edited:

* Links seemed to work
* images links needed conversion from mediawiki to Markdown
* headers need converting to markdown


## Notes

From https://community.atlassian.com/t5/Bitbucket-articles/What-to-do-with-your-Mercurial-repos-when-Bitbucket-sunsets/ba-p/1155380/page/14?utm_campaign=immediate_general_comment&utm_content=topic&utm_medium=email&utm_source=atlcomm

> github's hg importer messes up the order of merge parents
> ... hg-fast-export gets it right:
> 
> https://github.com/frej/fast-export/
>
> If you use bookmarks in mercurial, hg-fast-export can't deal with them though. I made a tool that wraps hg-fast-export, first converting bookmarks to named branches so it can deal with them:
> 
> https://github.com/chrisjbillington/hg-export-tool


