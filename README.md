# Github->Gitlab Sync

## Overview
This pipeline is designed to sync a project from Github to a corresponding Gitlab repository.  Note that this is a one way sync,
ie. changes from Gitlab will NOT be synced back to Github.  

## How To Use
1. Create an SSH key: `ssh-keygen -t rsa -b 4096 -f github-gitlab-sync`
2. In Gitlab, under Settings->Repository, add the public key as a deploy key and make sure to check the "Write access allowed" box
3. (Optional) If you have a private Github repo, add the public key you created as a deploy key there also.
4. Add the private key to the Jenkins credential store:
  * Use type "SSH username and Private Key"
  * Paste in the private key directly
  * Set the username identical to the keyname, ie. `github-gitlab-sync` 
  * You can specify the credential ID, or allow Jenkins to randomly generate one for you
  * Record the credential's ID
5. Create a Jenkins Pipeline Job
  * Click Configure
  * Under "Pipeline" select "Pipeline Script from SCM", and enter in the URL to this repository `https://github.com/venicegeo/github-gitlab-sync.git`
  * Click Save
6. Run job to populate parameter fields
7. Configure parameters
  * Click configure
  * Configure default parameter values to match your Github/Gitlab repos

