# Git workflow which remotely accesses a server and executes commands
## Prerequisites:
- Git repository and account
- Server with SSH keys and access allowed (The code and tutorial is specifically made for Digital Ocean's droplet. Some steps may vary depending on the exact host you use)
- Server's public SSH key linked to github account:
1) Go to "SSH and GPG keys" under profile settings: 
![2023_12_22_0sh_Kleki](https://github.com/davidkovacwebdev/Git-action-remote-command/assets/150269692/c08fe07c-e8a5-4c1e-ad70-868bcc57aa77)
2) Add a new key
![Screenshot from 2023-12-22 17-09-07](https://github.com/davidkovacwebdev/Git-action-remote-command/assets/150269692/cb18b0c7-00c7-44fb-86b0-635417e972d3)

 
## Basic workflow file which is to be pt in .github/workflows.


On each push to the master branch (can be changed at line 4.

`4.    - master`

Usually it's "main") 

Workflow triggers jobs which use [appleboy/ssh-action](https://github.com/appleboy/ssh-action) job uses sftp parameters which can be changed in file but it's best to use [Action Secret variables](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions).

Paremeters to be changed:
- host, represented by the ${{ secrets.host }} secret variable. Make sure to adjust the value on your GitHub repository secret
- username, represented by the ${{  secrets.username  }}
- key, represented by the ${{  secrets.key  }}
- port, represented by the ${{  secrets.port  }}
- Command timeout, change it per your needs

Finally, scripts option allows you to setup a set of commands executed on the server once successfully connected.
In the example commands are:
- `cd ~/droplet-deploy`  This command accesses the project folder, make sure to change it per your needs.
- `git pull origin master`  Pulls the changes from the master, also feel free to change the branch name or use any other command for that matter.
- `echo "That worked hopefully"`  Random comment

 
Last command "script" lists the command that should be executed on the server. By default, code executes a command which navigates to the project folder and executes "git pull" and a comment

 
## Things to double check:
- Make sure private key pasted to one of the secrets contains these comments as well:
  `-----BEGIN RSA PRIVATE KEY-----
...
-----END RSA PRIVATE KEY-----`

- There's a chance you'll encounter this error `Ssh: handshake failed: ssh: unable to authenticate, attempted methods [ none publickey]` in case you're running newer versions of ubuntu.
In that case I'd suggest checking out this part of the [following tutorial](https://medium.com/swlh/how-to-deploy-your-application-to-digital-ocean-using-github-actions-and-save-up-on-ci-cd-costs-74b7315facc2#b678) 
The whole tutorial is great so feel free to check it out

# Good luck



