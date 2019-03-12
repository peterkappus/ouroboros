# Ouroboros
This is a multi-media (audio & visual) art piece playing with vocal & mathematical harmonic resonance.

## Video
The video is created by the stand-alone version of Processing which runs the included .pde file.

See inside the file for details.

## Audio
The audio piece uses [P5](http://p5js.org/) in a web browser (deployed locally via [P5 manager](https://www.npmjs.com/package/p5-manager)). 

### Live updating with Docker & P5 Manager
Do these steps from inside the `public` folder.

```
cd public
```

- First time: `docker build -t p5manager .`
- Then run
```
docker run -it -p 5555:5555 -p 35729:35729 -v"$(PWD)":/app p5manager
```
- open a browser and visit http://localhost:5555/#/. (note the dot at the end)
- If necessary, allow access to your microphone
- Use the following keyboard shortcuts:
  - **M**: "mute" or unmute the current voice. The screen will turn red when the mic is active.
  - **Numbers 0-9**: Create a new "voice" with given reverb time (0 = min; 9 = max)
  - **K**: "kill" the oldest voice by fading it out and removing it. Use this to conserve resources and prevent stuttering/clipping
  - **ENTER**: Save a WAV file of the current session. You'll need to reload the page after using this option to reset the buffer (TODO: make this reload unnecessary)


### AWS Deployment
To deploy into an S3 Bucket for hosting, you can use the S3 utility `s3cmd` like so

```
s3cmd sync ./ s3://<YOUR_BUCKET_NAME> --delete-removed -P --rexclude=.git*
```

You might put the above into a `deploy.sh` file to make deployment easier.


### Deploy with Docker (recommended):
You can also do the deployment w/ docker so you don't have to install `s3cmd` locally:
```
# set your AWS credentials...
export AWS_ID=<YOUR AWS KEY ID>
export AWS_SECRET=<YOUR AWS KEY SECRET>

# or put the above in a secrets file...
source .secrets

docker run -v "$(pwd)"/public:/data --env AWS_ACCESS_KEY_ID=$AWS_ID --env AWS_SECRET_ACCESS_KEY=$AWS_SECRET garland/aws-cli-docker aws s3 sync . s3://www.peterkappus.com/voice/ --delete --acl=public-read --exclude=".git*"
```
