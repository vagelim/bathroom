# Background

  * Working in a shared office space with a limited number of bathrooms can cause wasted time checking on availibility
  * We thought it would be convienant to have an easy way to remotely check availability

## Guidlines
  1. Do not be creepy 
    * a bathroom is a private space, no video etc
    * keep individual usage annoymous and untracked 
  1. Be reliable
    * minimize false positives and negatives
    * try to use locking as measure of occupied when possible
    * do not interfere with existing functionality of door or locks
  1. Keep it professional 
    * the office is still a place of business with frequent visitors so keep things neat and clean looking
    * adding devices on our network comes with a responsiblity to secure them properly


# Setup

```
sudo apt-get install daemontools-run git ucspi-tcp

git clone git@github.com:DataDog/bathroom.git

# Test sensor hookup
BATHROOM_CONFIG="12 men left:14:nc,12 men right:15:nc" bathroom/bin/bathroom_status

# Going the djb way mostly because this version of raspbian doesn't have a sane
# init system.
sudo mkdir /service
sudo mv bathroom /service/bathroom
ln -s /service/bathroom . # for convenience if you want
mkdir /service/bathroom/envdir/
echo "12 men left:14:nc,12 men right:15:nc" > /service/bathroom/envdir/BATHROOM_CONFIG # set according to your setup
sudo adduser --system --shell=/usr/sbin/nologin --ingroup=gpio bathroom
sudo ln -s /service/bathroom /etc/service/bathroom
```

# Usage

```
$ nc bathroom.example.com 50
Bathroom Status:
12 men left: Available
12 men right: Available
```
