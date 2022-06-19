# Basic Git Commands
List of basic git commands
<br>
<br>

## Add Local Connection

It is recommended that you first create the repository in GitHub before you start adding files and code in the local folder. To do this:<br>
1. Create a [new repository](https://github.com/new) on GitHub
2. Open the cmd and use the command ```cd path``` where path is the location you want to have your local folder.<br>
&nbsp;&nbsp;&nbsp;&nbsp;*Note that the final destination of the repository would be ```path\repo_name```*
3. Use ```git clone repo_link```. You can find the repo link on your repository, under code, under HTTPS

More info on [git clone](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository)
<br>
<br>

## Git Push
The git push command allows us to upload code from a local folder to the repository. To do this:<br>
1. Open the cmd and use the command ```cd path``` where path is the location of the local folder.
2. Add all files  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```git add -A```
3. Add a commit   &nbsp;&nbsp;&nbsp;&nbsp;```git commit -m "comment"```
4. Push           &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```git push``` or ```git push origin master```
<br>

## Git Pull
When working on teams, or on different machines, git pull allows us to download code from a GitHub repository to the local folder. To do this:
1. Open the cmd and use the command ```cd path``` where path is the location of the local folder.
2. Download the changes ```git pull```
