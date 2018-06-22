# gcf-tools

### This package aims to make working with Google Cloud Background Functions a little easier and faster

Specifically, the shell commands here (in conjunction with the underlying GCP toolchain) can:

- Install and configure the Google Cloud Function Emulator
- Easily switch between cloud and emulator GCF targets
- Deploy Background Cloud Functions, both locally and to production
- Trigger Cloud Pubsub messages easily from the command line including data and attributes

Mostly, the scripts handle the steppiness and complexity of the above tasks and aim to help you develop faster and spend less time trying to grok and remember command line flags. 

## gcf-call
Call cloud functions running in the emulator.
```
Usage:
-----
gcf-call --name funcName --message msg [--attr1 value --attr2 value ...]

Flags:
-----
-h --help	: This help message
-m --message	: The message (data) to pass to the function
-n --name	: The name of the GCF function to call
--*		: Any argument pairs beginning with -- will get passed as 
		  attributes to the called function
```

This command spares you from having to base64 encode your message or pass JSON on the command line. Use `functions logs read` to read the node log output.

## gcf-deploy-pubsub
Deploy GCF Pubsub background functions to the emulator or to the cloud
```
Usage:
-----
gcf-deploy-pubsub [-l] --name nnnn --topic tttt --entry-point funcName [/path/to/nodefuncdir]

If the codepath is omitted, the current working directory is assumed.

Flags:
-----
-e --entry-point	: Code entry point function (required)
-h --help		: This help message
-l --local		: Deploy locally, to the emulator [off/cloud]
-m --memory		: Memory allocation, numeric, in MB (128,256,512,1024,2048) [128]
-n --name		: GCF name (required)
-p --project		: The GCP Project to deploy to [current project]
-s --stage-bucket	: Deployment stage bucket [last utilized stage bucket]
-t --topic		: Topic channel (required)
```

`gcf-deploy-pubsub` irons out some of the small differences between deploying to the local emulator and deploying the cloud. 

`--stage-bucket` is only needed for cloud deploys. Since it's highly likely that ever single cloud deploy for a project will use the same storage bucket, the last-used storage bucket is saved and used as the default when the argument is omitted.

`gcf-deploy-pubsub` will switch the current cloud configuration when it runs, so it's dependent on either using gcf-emulate to set up and manage the emulator or on using the [configuration pattern described in the emulator docs](https://github.com/GoogleCloudPlatform/cloud-functions-emulator/wiki/Using-the-Emulator-with-the-Cloud-SDK).

Node code changes in the emulator are picked up immediately, there's no need to redeploy emulator functions after a deploy.

## gcf-emulator
Install and control the local Google Cloud Functions Emulator
```
Usage:
-----
gcf-emulator --config default|emulator | --init | --start | --stop

Flags:
-----
-c --config		: Swap the gcloud config between 'default' and 'emulator'
-h --help		: This help message
-i --init		: Install the emulator and set up a mutated config to utilize it
--start			: Set the comfig to 'emulator' and start the emulator
--stop			: Restore the default config and stop the emulator
```
# Installation
Put the `bin` folder into your path (or don't)

# Dependencies
- `bash`
- `gcloud`
- `npm`
- `Perl5`

# See also
- [GCF Emulator Docs @ Google](https://cloud.google.com/functions/docs/emulator) 
- [Cloud Functions Emulator on Github](https://github.com/GoogleCloudPlatform/cloud-functions-emulator)

# Todo
### Direct Pubsub Messages from dev_appserver to the emulator
The original goal of this project was to redirect pubsub messages from a local instance of the Standard Go Environment to the emulator instead of the cloud. It would seem that this should be possible but I was unable to do it. `gcf-emulate` is useful for developing, debugging and testing, but complete local intergration testing is still missing this piece. Tips appreciated.

### Better Environment Checking
In most (or at least some) cases, the tool will fail sanely if you are missing basics like gcloud, but the handling and messaging for these cases could be cleaner.

While the syntactical nuttiness of bash scripting is always fun, I'm sure there are some bits that can be written better. Improvements welcome via PR.
