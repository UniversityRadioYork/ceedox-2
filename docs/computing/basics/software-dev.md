# Software Development

This page offers a rapid tour of URY's **software development** resources.

## Acquiring URY source code

At time of writing, nearly all of URY's development is done on [computing:software:external:GitHub](computing/software/external/GitHub): see that article for more information.

GitHub uses [Git](https///git-scm.com/) as its version control system. Git is simple, which makes it both really nice to use and also really horrific with its learning curve. Reading [the enclosed instruction book](https///git-scm.com/doc) is recommended.

## Acquiring older URY source code

Some of URY's older source may not be available on GitHub. Before we used GitHub, we used:

- A Git server on [urybsod](computing/comp-machines/urybsod);

- A SVN server on [uryfs1](computing/comp-machines/uryfs1);

- A CVS server on [ury](computing/comp-machines/ury).

Most of these are now decommissioned, though backups may still exist. (Some of the old CVS root is now on GitHub as the private repository [cvs-ury](https///github.com/UniversityRadioYork/cvs-ury)).

Old documentation about these VCSes follows.

### urybsod git

#### Prerequisite knowledge

Use port forwarding using [ssh tunnels](.ssh_access) or [UNI VPN](.vpn_access)

#### Access

GIT repos are stored on **urybsod:/home/git** owned by the **git user and group**.\\

#### How to do stuff

_Always_ use the git user for developmenting purposes.

    git clone git@urybsod:somerepo.git

To commit changes to your local repository, first add any new files, then commit with a message:

    git add [filename]
    git commit -m "Enter a nice pretty git message here"

Once you are done working on whatever you're working on, you push changes back to the development server:

    git push

It may argue with you about there being other changes - apparently other people can edit code too! To 'pull' changes by others:

    git pull

That's it! One can now git. Once a push is made to many of our git repositories, an automated build process will be started by Jenkins. Once complete the built code can be pulled onto the live location.

Remember to refresh your local copy with the latest from the remote repo (git pull or git fetch/git merge), and commit all your changes to it, or branch.

#### Software specifics

Certain bits of software, notably the [ computing:software:Website ](computing/software/Website) and [ computing:software:external:Radioplayer ](computing/software/external/Radioplayer), have more involved git workflows than the standard push-pull. Check their pages for more information.

#### Repos

| **repo**        | **Description**                                                                                                                                                                              | **Live Pull Locations**                                                     |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| NewBAPS.git     | The [BAPS](computing/ury_software/baps) replacement                                                                                                                                          | -                                                                           |
| baps2-slut.git  | [BAPS2](computing/ury_software/baps2) The replacement of the replacement of [BAPS](computing/ury_software/baps)                                                                              |
| website.git     | Front website                                                                                                                                                                                | Deprecated software                                                         |
| radioplayer.git | Our URY version of [Radioplayer](computing/software/external/radioplayer)                                                                                                                    | Automagically by website.git                                                |
| SiS.git         | [Studio Information System](computing/ury_software/sis)                                                                                                                                      | Deprecated software                                                         |
| sis2.git        | [SIS2](computing/ury_software/sis2)                                                                                                                                                          | uryfs1:/usr/local/sis2                                                      |
| api.git         | [URY API](computing/ury_software/api) internals                                                                                                                                              | -                                                                           |
| myury.git       | MyURY - The Members Internal replacement                                                                                                                                                     | ury:/usr/local/myury                                                        |
| shibbobleh.git  | URY Single Sign-On Service                                                                                                                                                                   | ury:/usr/local/shibbobleh; uryfs1:/usr/local/shibbobleh; dog:/usr/local/dog |
| timelord.git    | Studio Clock                                                                                                                                                                                 | jukebox:/usr/local/timelord                                                 |
| uryjukebox.git  | iTones Web Management (Not inc. jukebox server API)                                                                                                                                          | uryfs1:/usr/local/uryjukebox                                                |
| propget.git     | Command-line utilities for querying [website properties](computing/software/in-house/website/properties)                                                                                     | Anywhere (separate install process)                                         |
| fpclient.git    | A FreeBSD port of https://github.com/lastfm/Fingerprinter/ with URY's API key. Required for MyURY/NIPSWeb. At time of writing the FreeBSD /usr/ports package has a sample API key hardcoded. | Made & installed on thunderhorn                                             |

### SVN

SVN replaced CVS
stored on **uryfs1** at **/var/svnury**
checkout using
svn co [svn+ssh://user@uryfs1.york.ac.uk:/var/svnury](svn+ssh///user@uryfs1.york.ac.uk//var/svnury) ury-svn

### CVS

user must be in cvs group on ury.york.ac.uk
export CVS_RSH="ssh"
cvs -d :ext:uryuser@ury.york.ac.uk/usr/home/cvsroot co urycvs/
