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
   ```md
   ## DCO Sign Off

   All commits must be signed off with the Developer Certificate of Origin (DCO). This attests that you have the rights to submit your contribution under our project's license.

   To sign off your commits:

   1. Configure your Git client with your real name and email:
   git config --global user.name "Your Name"
   git config --global user.email "your.email@example.com"

   2. Add the -s flag when committing:
   git commit -s -m "Your commit message"

   Or add the sign-off manually with:
   Signed-off-by: Your Name your.email@example.com
   ```
5. [ ] Add a DCO section to the README.md
   ```md
   ## Developer Certificate of Origin

   We use the Developer Certificate of Origin (DCO) in lieu of a Contributor License Agreement for all contributions to this project. We require all commits to be signed off with the DCO. Please read [DCO.md](DCO.md) for details.
   ```
6. [ ] **???** Setup ghwf (`.github/workflows/dco.yml`): **???**
   ```yaml
   name: DCO Check
   on: [pull_request]
   
   jobs:
     dco_check:
       runs-on: ubuntu-latest
       name: DCO Check
       steps:
       - uses: actions/checkout@v3
         with:
           fetch-depth: 0
       - name: DCO Check
         uses: tim-actions/dco@master
         with:
           token: ${{ secrets.GITHUB_TOKEN }}
   ```
7. Add a `commit-msg` git hook to validate commits:
   ```sh
   #!/bin/sh

   if ! grep -q "^Signed-off-by: " "$1"; then
     echo "ERROR: Commit message must be signed off with DCO" >&2
     exit 1
   fi
   ```
8. 