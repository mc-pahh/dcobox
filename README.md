# dcobox

## DCO integration checklist:

1. [x] Add `DCO.md` to repo with the DCO:
   ```
   Developer Certificate of Origin
   Version 1.1
   
   Copyright (C) 2004, 2006 The Linux Foundation and its contributors.
   
   Everyone is permitted to copy and distribute verbatim copies of this
   license document, but changing it is not allowed.
   
   
   Developer's Certificate of Origin 1.1
   
   By making a contribution to this project, I certify that:
   
   (a) The contribution was created in whole or in part by me and I
       have the right to submit it under the open source license
       indicated in the file; or
   
   (b) The contribution is based upon previous work that, to the best
       of my knowledge, is covered under an appropriate open source
       license and I have the right under that license to submit that
       work with modifications, whether created in whole or in part
       by me, under the same open source license (unless I am
       permitted to submit under a different license), as indicated
       in the file; or
   
   (c) The contribution was provided directly to me by some other
       person who certified (a), (b) or (c) and I have not modified
       it.
   
   (d) I understand and agree that this project and the contribution
       are public and that a record of the contribution (including all
       personal information I submit with it, including my sign-off) is
       maintained indefinitely and may be redistributed consistent with
       this project or the open source license(s) involved.
   ```
2. [x] Add CONTRIBUTING.md to repo with instructions like: 
   ````md
   # DCO Sign Off
   
   All commits must be signed off with the Developer Certificate of Origin ([DCO.md](DCO.md)).
   This attests that you have the rights to submit your contribution under our project's license.
   
   To sign off your commits:
   
   1. Configure your Git client with your github account details:
      ```
      git config --global user.name "Your Name"
      git config --global user.email "your.email@example.com"
      ```
   2. If you've configured git to use our hooks (`.githooks`), you are now ready. Otherwise, either:
      1. use our `.githooks`:
         ```
         git config set core.hookspath .githooks
         ```
         **OR**  
      2. Add the `-s` flag when committing:
         ```
         git commit -s -m "Your commit message"
         ```
   ### Or 
   
   * Add the sign-off manually with:
      ```
      Signed-off-by: Your Name your.email@example.com
      ```
   ````
3. [x] Add a DCO section to the README.md
   ```md
   We use the Developer Certificate of Origin ([DCO.md](DCO.md)), this attests that you have the rights to submit your contribution under our project's license. We require all commits to be signed off with the DCO. 

   Please consult [CONTRIBUTING.md](CONTRIBUTING.md) for additional details.
   ```
4. [x] Add a `prepare-commit-msg` git hook to enforce sign-offs on commits:
   ```sh
   #!/bin/bash
   
   # A git commit hook that will automatically append a DCO signoff to the bottom
   # of any commit message that does not have one. This append happens after the git
   # default message is generated, but before the user is dropped into the commit
   # message editor.
   ROOT=$(git rev-parse --show-toplevel)
   cd $ROOT
   
   COMMIT_MESSAGE_FILE="$1"
   AUTHOR=$(git var GIT_AUTHOR_IDENT)
   SIGNOFF=$(echo "$AUTHOR" | sed -n 's/^\(.*>\).*$/Signed-off-by: \1/p')
   
   # Check for DCO signoff message. If one does not exist, append one and then warn
   # the user that you did so.
   if ! grep -qs "^$SIGNOFF" "$COMMIT_MESSAGE_FILE"; then
     echo -e "\n$SIGNOFF" >> "$COMMIT_MESSAGE_FILE"
     echo -e "Appended the following signoff to the end of the commit message:\n  $SIGNOFF\n"
   fi
   ```
5. [ ] Install [Github DCO Bot](https://probot.github.io/apps/dco/) **When PR is merged? To main?**
   1. [x] Add a `.github/dco.yml` file with
      ```
      allowRemediationCommits:
        individual: true
      ```
      to allow retroactive remediations.
