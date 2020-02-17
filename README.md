# bitbucket-_tools

random tools for dealing with bitbucket.org repos

Getting my notes off my machine and in a shareable state.

## Tools

### bitbucket-cli - 2019

Updated version that works as of 2019, with REST v2.0 API https://developer.atlassian.com/bitbucket/api/2/reference/resource/

https://bitbucket.org/arrizza/bitbucket-cli/src/master/

Also includes a tool convert_to_git (recommend bitbucket-hg-to-git instead)

### bitbucket-hg-to-git

https://bitbucket.org/edrandall/bitbucket-hg-to-git

creates new BitBucket git repos from mercurial ones. Handles some meta data/tags.

### bitbucket-issue-migration

https://github.com/jeffwidman/bitbucket-issue-migration

## Migrating away from BitBucket

Lots of options, none complete.

GitHub's code/history import https://github.com/new/import worked well for me to import and convert a Mercurial code repo. It does not handle:
* project description - needs to manually copy/pasted 
* project logo/avatar/image  - needs to manually set (note you may not be able to get the original image that was used to upload, only a scaled-down copy unless you have the original stored somewhere)
* Issues / history / attachments
* PRs / history
* Downloads
* wiki
### project description

needs to manually copy/pasted 

### project logo/avatar/image

WIP can this be scrapped?

needs to manually set (note you may not be able to get the original image that was used to upload, only a scaled-down copy unless you have the original stored somewhere)

### Issues / history / attachments

WIP

https://github.com/jeffwidman/bitbucket-issue-migration

    curl https://api.bitbucket.org/2.0/repositories/clach04/jython/issues  # no issues example

use V2 api to see if there issues at all

### PRs / history


### Downloads

WIP

https://developer.atlassian.com/bitbucket/api/2/reference/resource/repositories/%7Bworkspace%7D/%7Brepo_slug%7D/downloads#get

    curl https://api.bitbucket.org/2.0/repositories/clach04/dava33display/downloads  # no downloads
    curl https://api.bitbucket.org/2.0/repositories/clach04/jython/downloads

### wiki

WIP anyway to create initial wiki page? No API, potentially could scrap/robot webpage

Create an initial page and then force push the new git repo. The wiki content will need to be edited:

* Links seemed to work
* images links needed conversion from mediawiki to Markdown
* headers need converting to markdown
