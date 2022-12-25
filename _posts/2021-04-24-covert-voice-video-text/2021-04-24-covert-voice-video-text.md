---
layout: post
title: "Covert Voice In A Video To Text"
date: "2021-04-24"
description: ""
coverimage: Covert_Voice_In_A_Video_To_Text.jpg
tags: voice convert
published: true
posttype: article
categories: tutorial
---
Have you ever wanted to extract the text from a video file? There are a number of software packages and transcription services flooding the search engines and it can be hard to pick a piece of software or a service. 

As a programmer my preferred choice to this solution was to script something myself using the Google Cloud Speech-to-Text service. 
 
This tutorial will demonstrate how to transcribe short video files ( less than 1 minute) using synchronous speech recognition and long audio files (longer than 1 minute) to text using asynchronous speech recognition.

We will look at doing this with Python and Curl. 

Two methods, RPC and REST. RPC takes in binary audio files however REST submits JSON and so we need to convert out file into base64 if we want to send it from out local machine. 


<h5 class="step">1. Downloading an API key for the Speech-to-Text service</h5>

First we sign up for an Google Cloud Web Services and download an API key for the Speech-to-Text service

In order to do this there are a few prerequisites: 

* [Set Up a Speech-to-Text Project](https://cloud.google.com/speech-to-text/docs/quickstart-client-libraries) in the Google Cloud Console. 
* You've set up your environment using [Application Default Credentials](https://cloud.google.com/speech-to-text/docs/quickstart-client-libraries) in the Google Cloud Console.

<h5 class="step">2. Creating a Virtual Environment </h5>

> For the purpose of this tutorial we are going to start using the python client libraries however if you don't wish to install the python libraries then it is possible to use the built in Curl command which will also demonstrate later on in this article as an alternative. 

With our API key downloaded we need to write some code. Due to the fact that we will be installing python modules we should start off by creating a python virtual environment to keep the libraries contained from interfering with that of the system. 
```
cd ~
mkdir converter
cd converter
apt-get install python3-venv
```

If you get an error creating the environment you may need to install the following package
```
apt-get install python3-venv
```

With our virtual environment created we need to activate it. 
```
source venv/bin/activate
```

<h5 class="step">3. Installing the Google Cloud Speech module for Python</h5>

With the environment activated. Install the following python client library. 
```
pip install --upgrade google-cloud-speech
```

<h5 class="step">4. Set an Environmental Variable to point to the API key</h5>
		
Next, place the downloaded API key in the projects directory. You do not need to keep the default name, you can call it anything you want. I have decided to rename mine to `config.json`

<img src="/static/cf20d2a3-e0cd-4d8b-87d9-6fc0c8a2c5f2.png" class="img-fluid" alt="">

Next Step, set the environment variable for the config file.
```
export GOOGLE_APPLICATION_CREDENTIALS="/home/evilsaint/converter/config.json"
```

<h5 class="step">5. Extracting the Audio</h5>

Next we extract the audio as the visual element is unneeded for the the conversion.  Run the following command to covert your file. 
```
ffmpeg -i <video-input-file.mp4> <audio-output-file.wav>
```
> Before proceeding to the next step it is a good idea to play the audio file (at least in part) to check the audio conversion worked?

Assuming you are happy with the playback you can now proceed to the next step. 


<h5 class="step">6. Downloading Google Cloud Speech Sample Application Code</h5>

Download the following file. 
```
wget https://raw.githubusercontent.com/googleapis/python-speech/master/samples/snippets/transcribe_async.py
```


<h5 class="step">7. Customise transcribe_async.py for the WAV</h5> 


Due to the fact we are using `WAV` files we either need to admit the following line or specify 44100.

```
sample_rate_hertz=16000
```

Edit the following lines for the Google Cloud Bucket and the local file functions. 
```
nano -l transcribe_async.py
```

<img src="/static/f943465c-0866-48a2-be78-037f02b45b4c.png" class="img-fluid" alt="">

<img src="/static/3317f1c2-cd2e-4f04-8385-98117a818cca.png" class="img-fluid" alt="">


Because the WAV file I have contains two channels I am going to modify the audio channel config and also the encoding. 
```
    config = speech.RecognitionConfig(
        encoding='LINEAR16',
        sample_rate_hertz=44100,
        language_code="en-US",
        audio_channel_count=2,
    )
```

<h5 class="step">8. Hosting the File In The Cloud (Optional)</h5>


One of the easiest ways to host the audio files is to upload them go you Google Cloud Platform (GCP) bucket. 

> You must have proper access permissions to read Google Cloud Storage files, such as one of the following:
 * Publicly readable (such as our sample audio files)
 * Readable by your service account, if using service account authorization.
 * Readable by a user account, if using 3-legged OAuth for user account authorization.


<img src="/static/0f752dd2-687c-4509-8cab-dbfbc1a1c6da.png" class="img-fluid" alt="">


<h5 class="step">9. Convert The File</h5>

Now we can convert our file from the cloud
```
python transcribe_async.py gs://evilsaint/audio-output.wav
```

<img src="/static/e126cd77-7708-40ab-b407-e67de97ee5a1.png" class="img-fluid" alt="">

Or we can convert the file locally. 
```
python transcribe_async.py audio-output.wav 
```


# Using Curl



<h5 class="step">1. Converting the Audio to a Text File</h5>

Next we need to covert our audio file to base64 before we can send it to the "Speech-to-Text" API from the local system. This is not needed if you are sending the file from the GPC bucket. 
```
base64 audio-output.wav -w 0 > audio-data.txt
```


<h5 class="step">2. Constructing the Curl command</h5>


Make a request.json file
```
touch request.json
```

Edit the file
```
nano request.json
```

Inside the file we can do 
```
{
  "config": {
        "encoding":"LINEAR16",
        "sample_rate_hertz":"44100",
        "language_code":"en-US",
        "audio_channel_count":"2",
  },
  "audio": {
      "uri":"gs://evilsaint/audio-output.wav"
  }
}
```

And make a curl command like so
```

```










  446  mv request.json sync-request.json
  447  cat sync-request.json 
  448  curl -s -H "Content-Type: application/json"     -H "Authorization: Bearer "$(gcloud auth application-default print-access-token)     https://speech.googleapis.com/v1/speech:recognize     -d @sync-request.json
  449  echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
  450  sudo apt-get install apt-transport-https ca-certificates gnupg
  451  curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -
  452  sudo apt-get update && sudo apt-get install google-cloud-sdk
  453  sudo apt-get install google-cloud-sdk-app-engine-java
  454  gcloud init
  455  gcloud init --skip-diagnostics
  456  history

```
cat sync-request.json 
{
  "config": {
        "encoding":"LINEAR16",
        "sample_rate_hertz":"44100",
        "language_code":"en-US",
        "audio_channel_count":"2",
  },
  "audio": {
      "uri":"gs://evilsaint/audio-output.wav"
  }
}
```


```
curl -s -H "Content-Type: application/json"     -H "Authorization: Bearer "$(gcloud auth application-default print-access-token)     https://speech.googleapis.com/v1/speech:recognize     -d @sync-request.json
```




{
  "error": {
    "code": 400,
    "message": "Sync input too long. For audio longer than 1 min use LongRunningRecognize with a 'uri' parameter.",
    "status": "INVALID_ARGUMENT"
  }
}


```
curl -s -H "Content-Type: application/json"     -H "Authorization: Bearer "$(gcloud auth application-default print-access-token)     https://speech.googleapis.com/v1/speech:longrunningrecognize     -d @sync-request.json
```



```
curl -s -H "Content-Type: application/json"     -H "Authorization: Bearer "$(gcloud auth application-default print-access-token)     https://speech.googleapis.com/v1/speech:longrunningrecognize     -d @sync-request.json
{
  "name": "1211050700785987050"
}
```

Maybe
```
curl -s -H "Content-Type: application/json"     -H "Authorization: Bearer "$(gcloud auth application-default print-access-token) https://speech.googleapis.com/v1/operations/1211050700785987050 
```


<img src="/static/9fc6cb8c-f6f5-4041-b83b-32b8562179d3.png" class="img-fluid" alt="">