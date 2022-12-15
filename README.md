<p align="center">
  <a href="#build-framework">
  <img src="https://raw.githubusercontent.com/armbian/build/master/.github/armbian-logo.png" alt="Armbian logo" width="144">
  </a><br>
  <strong>Armbian Linux Kernel Caching</strong><br>
<br>
<a href=https://github.com/armbian/gitutils/actions/workflows/main-latest.yml><img alt="GitHub Workflow Status" src="https://img.shields.io/github/actions/workflow/status/armbian/gitutils/main-latest.yml?logo=linux&label=Make%20shallow%20bundles&style=for-the-badge&branch=main"></a>
 <br>

<br>
<a href=https://twitter.com/armbian><img alt="Twitter Follow" src="https://img.shields.io/twitter/follow/armbian?logo=twitter&style=flat-square"></a>
<a href=http://discord.armbian.com/><img alt="Discord" src="https://img.shields.io/discord/854735915313659944?label=Discord&logo=discord&style=flat-square"></a>
<a href=https://liberapay.com/armbian><img alt="Liberapay patrons" src="https://img.shields.io/liberapay/patrons/armbian?logo=liberapay&style=flat-square"></a>
</p>

# Preface

- Full, non-shallow clones of Linux kernel trees are multiple gigabytes in size, and take a lot of time to process,
- Shallow clones are much smaller, but cloning/fetching shallow trees impose a huge load on the git server (eg: `kernel.org`),
- `kernel.org` has [git bundles](https://git-scm.com/docs/git-bundle) available for download over HTTPS/CDN (simple file download). In fact they [recommend their use](https://www.kernel.org/best-way-to-do-linux-clones-for-your-ci.html) and have [instructions](https://www.kernel.org/cloning-linux-from-a-bundle.html)

## What this repo does?

- it does the heavy lifting - grabbing bundles, updating them from live git servers, and makes them shallow and ready for consumption,
  - Produced shallow bundles are around 250mb as of 5.18,
  - Produced shallow bundles include all tags for the version involved, including `-rc` tags,
  - Scheduled runs update the bundles every 24hs, using GitHub actions, including caching,
  - They are deployed to two locations, as [Github releases](https://github.com/armbian/gitutils/releases/latest) and uploaded to main [Armbian CDN](https://dl.armbian.com/_gitbundles/)

## How to use

- Example for 5.17:

```bash
# Download the bundle from this repo's Github releases.
wget --continue --progress=dot:giga -O "linux-5.17.gitbundle" "https://github.com/armbian/gitutils/releases/download/latest/linux-5.17.gitbundle"
git init linux-5.17 # init an empty repo
cd linux-5.17 # go into it
git remote add "linux-5.17-bundle" "../linux-5.17.gitbundle" # add the downloaded bundle as a remote
wget -O ".git/shallow" "https://github.com/armbian/gitutils/releases/download/latest/linux-5.17.gitshallow" # download .git/shallow
git fetch linux-5.17-bundle # fetch from the bundle.
git checkout FETCH_HEAD # checkout from the bundle's HEAD
git tag -l # look at the available tags (all 5.17-related tags)
```

