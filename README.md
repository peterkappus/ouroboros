# psynth
Peter's Superfun Browser-synth playground.

## Live updating with Docker & P5 Manager
- First time: `docker build -t p5manager .`
- Then run
```
docker run -it -p 5555:5555 -p 35729:35729 -v"$(PWD)/public":/app p5manager
```
- open a browser and visit http://localhost:5555/#/. (note the dot at the end)
- NOTE the provided `.p5rc` file in the root folder to tell p5manager to look in '.' for projects

## AWS Deployment
You might put this into a `deploy.sh` file to make life easier.
`s3cmd sync ./ s3://<YOUR_BUCKET_NAME> --delete-removed -P --rexclude=.git*`


### Deploy with Docker:
You can also do the deployment w/ docker so you don't have to install `s3cmd` locally:
```
# set your AWS credentials...
export AWS_ID=<YOUR AWS KEY ID>
export AWS_SECRET=<YOUR AWS KEY SECRET>

# or put them in a file...
source .secrets

docker run -v "$(pwd)"/public:/data --env AWS_ACCESS_KEY_ID=$AWS_ID --env AWS_SECRET_ACCESS_KEY=$AWS_SECRET garland/aws-cli-docker aws s3 sync . s3://www.peterkappus.com/voice/ --delete --acl=public-read --exclude=".git*"
```
