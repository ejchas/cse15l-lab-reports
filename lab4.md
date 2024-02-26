## Editing from the Command Line with `vim`

For the timed task that we did during the Week 7 Lab, I had to time myself and practice using `vim` commands effectively. 

### Step 4:
After forking the given repository and setting up a timer, I first logged into `ieng6` using the `ssh` command and my `ieng6` account:

<img width="541" alt="Lab Report 4 SC 1" src="https://github.com/ejchas/cse15l-lab-reports/assets/156235662/8dc5d415-47e2-4357-b122-8c4b4cbf5574">

#### Commands Used: 
`ssh` + `<space>` + `echastain@ieng6.ucsd.edu` + `<enter>`

This set of keypresses will establish an `ssh` connection where the hostname `@ieng6.ucsd.edu` is the specific server that I want to connect to. I already set up `ssh` keys to provide for passwordless authentication, so automatically, I'm granted access to the remote server by just typing my username `echastain`. This process took about 5 seconds, as the confirmation text I obtained in the terminal from logging in took a few seconds to load. 

### Step 5:

Next, I obtained the `SSH` URL from the fork of the repository on GitHub, and used the `git clone` command in the terminal to clone it into my workspace:

