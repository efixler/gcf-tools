# gcf-tools

This package provides tools to make working with Google Cloud Functions a little easier and faster.

Specifically, the shell commands here (in conjunction with the underlying GCP toolchain) can:

- Install and configure the Google Cloud Function Emulator
- Easily switch between cloud and emulator GCF targets
- Deploy Cloud Functions, both locally and to production
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
-m --message	: The message (data) to pass to the function
-n --name	: The name of the GCF function to call
-u --usage	: This help message
--*		: Any argument pairs beginning with -- will get passed as 
		  attributes to the called function
```

This command spares you from having to base64 encode your message or pass JSON on the command line.

## gcf-deploy-pubsub

## gcf-emulate

