# Portfolio
The following site has been setup to show any work by @danmeal

# Installation

## To Run Locally
- Run `bundle exec jekyll serve`

# Deployments
Deployments are handled automatically via Github Pages

# Acknowledgements
- Background images from [https://unsplash.com/photos/Ptd-iTdrCJM](https://unsplash.com/photos/Ptd-iTdrCJM)

# Misc
export JEKYLL_VERSION=3.8
docker run --rm \
  --volume="$PWD:/srv/jekyll:Z" \
  -p 4000:4000 \
  -it jekyll/jekyll:3.8 \
  jekyll serve