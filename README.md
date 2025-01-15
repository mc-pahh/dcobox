# dcobox

## Attempt 1

1. [ ] Create DCO.md in root
2. [ ] Github config:
   1. [ ] Enable branch protection for `main`(default) and `develop`
      1. [ ] Enable: **Require signed commits**
      2. [ ] default: Restrict deletions
      3. [ ] default: Block force pushes
3. [ ] Install [Github DCO Bot](https://probot.github.io/apps/dco/)
4. [ ] create a `CONTRIBUTING.md` file
   ````md
   # DCO Sign Off

   All commits must be signed off with the Developer Certificate of Origin (DCO).
   This attests that you have the rights to submit your contribution under our project's license.

   To sign off your commits:
   1. Configure your Git client with your real name and email:
      ```
      git config --global user.name "Your Name"
      git config --global user.email "your.email@example.com"
      ```
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
5. [ ] Add a DCO section to the README.md
   ```md
   ## Developer Certificate of Origin

   We use the Developer Certificate of Origin (DCO) in lieu of a Contributor License Agreement for all contributions to this project. We require all commits to be signed off with the DCO. Please read [DCO.md](DCO.md) for details.
   ```
6. [ ] Add a `prepare-commit-msg` git hook to enforce sign-offs on commits:
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

## Questions:

1. Do we care about `Verified`(read: signed) commits?
2. Do we use dco bot or roll our own wf?
3. 