## Git and Github

pushing files to github from part 1 is very helpful, follow below steps for git setup-

1. create repo in you github

2. go to ec2 in your project directory

```
git init
git add *
git commit -m "initial commit"
git status
```

if you do 
```
git remote add origin https://github.com/your-github-username/your-repo-name.git
```
its gonna give error , as github has removed remote login, so using personal access token can solve this issue.
go to setting in github -->deveoper settings-->generate new token
save this token somewhere

```
git remote set-url origin https://<your-token>@github.com/your-github-username/your-repo-name.git
git push -u origin master
```

I faced an issue where it was pushing to github but it was showing ubuntu user(default user in ec2) and also the contribution page stayed empty even after the new commits.
Another issue yo might face is , while creating a github repository you might add a READme file, if you add any file through github UI always pull the new files in your local and then push the coode to github
```
git pull origin master
```
this is part of gitops best practices and it will keep your local files upto date and won't create a common error of 'merge conflicts' when you push to github. 

if you make changes to your project and you git is also setup , just use 

```
git add filename
git commit -m "message"
git status
git push
```

changing the branch and merging we will see ahead
