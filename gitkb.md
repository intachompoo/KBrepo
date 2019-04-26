# Basic command
    git clone http://xxxxxxxxxxxxxxxxx.git
## After edit or update or new
    git add .
    git commit -m "update"
    git push origin master
## After git remote have update
    git pull

# Check new or update file with git fetch
    git fetch
        f469af4..64a7ecb  master     -> origin/master
    git log --graph --decorate f469af4..64a7ecb

--------------------------------------------------------------------------

# Check after git pull
    git pull
    git log -n 5 --oneline

---------------------------------------------------------------------------

# Store user pass 2Hr.
    git config credential.helper store
    git config --global credential.helper 'cache --timeout 7200'
