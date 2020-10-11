# Gailbot Documentation

Tufts University (c) 2018 - 2020

## About

Gailbot is an automated transcription software named after [Gail Jefferson](https://en.wikipedia.org/wiki/Gail_Jefferson), inventor of the transcription system used for [Conversation Analysis](https://en.wikipedia.org/wiki/Conversation_analysis).

It is designed to be primarily used for generating Conversation Analysis transcripts that can then be improved manually.

Please see the **liability notice**.

## Overview

Gailbot either takes in recorded speech or records a new dialogue and uses a Speech to Text API to generate a time-aligned transcript. It uses a system of modules to detect conversation speech rate, analyze laughter, and to annotate structural features of conversation (pauses, gaps, overlaps etc.)

Gailbot renders the final transcript in JSON, CSV, and in time-aligned transcription formats including [CHAT](https://talkbank.org/manuals/CHAT.html), [TalkBank XML](https://talkbank.org/software/xsddoc/), and the Jeffersonian [CAlite](https://github.com/saulalbert/CABNC/wiki/CHAT-CA-lite) format developed at the Human Interaction Lab.

## Status / Contribute

**VERSION: 0.3.0**

This version of Gailbot has been tested on OSX Mojave 10.14.5.

This is an alpha version.

Users are encouraged to provide feedback, details regarding bugs, and development ideas.

Send an email to: [hilab-dev@elist.tufts.edu](mailto:hilab-dev@elist.tufts.edu)

## Installation

**Pre-requisites**

In order to use Gailbot, you should have some familiarity with using the terminal to install and run software.

You should also be aware that Gailbot uses [IBM Watson&#39;s STT API](https://cloud.ibm.com/apidocs/speech-to-text) that includes transcription charges and requires that the user create an IBM Bluemix account.

**Installation Steps**

**NOTE: Run these commands in order**

1. Install [Python 3.7](https://www.python.org/downloads/release/python-373/)

2. Install the [Homebrew](https://brew.sh/) packaging software (using their instructions)

3. Install the pip3 package installer by pasting this command in terminal:
- Brew install python3

4. Run the following commands by pasting them in terminal:
- Brew install ffmpeg
- Brew remove portaudio
- Brew install portaudio
- pip3 install pyaudio

5. Download and install the [CLAN editor](http://dali.talkbank.org/clan/) and [CAfont](http://dali.talkbank.org/clan/CAfont.otf) from [Talkbank](https://talkbank.org/software/).

6. Install the Gailbot directory by either:
- Downloading the [zip file](https://github.com/mumair01/Gailbot-3)
- Cloning the repository using [git clone](https://www.atlassian.com/git/tutorials/setting-up-a-repository/git-clone) 

7. Navigate to the Gailbot directory and use the &#39;requirements.txt&#39; file to install all libraries:
- Pip3 install -r requirements.txt

8. Create an account with IBM so you can use Watson&#39;s speech-to-text service
- You can sign up for a trial account [here](http://console.bluemix.net/catalog/services/speech-to-text).
- **NOTE:** Your IBM Bluemix username and password are required to establish a connection with Watson&#39;s Speech to Text service. Once you have registered you can find your credentials [here](ttps://console.us-east.bluemix.net/developer/watson/existing-services).
- Find transcription pricing [here](https://www.ibm.com/cloud/watson-speech-to-text/pricing).

## Usage

Gailbot can be executed using the following command from inside the Gailbot directory:

- Python3 gailbot-3.py -username [IBM Bluemix Username] -password [IBM Bluemix Password] -region [region]

- **NOTE**: The appropriate region for a user can be determined using the url provided
by the resource list alongside user credentials. Additionally, the location may be determined
[here](https://cloud.ibm.com/docs/containers?topic=containers-regions-and-zones). 




Gailbot can be used to perform three primary functions:

1. Transcribe an existing video or audio file.
2. Record and transcribe a new variable length conversation.
3. Re-apply post-processing algorithms to existing Gailbot transcript files and data without the need to re-transcribe the entire conversation using the (metered) Speech to Text service.

**Supported media formats**

Gailbot supports a number of audio and video file formats:

**AUDIO FORMATS**

| **Audio Format** | **Extension** |
| --- | --- |
| Audio/alaw | alaw |
| Ausio/basic | basic |
| Audio/flac | flac |
| Audio/g729 | g729 |
| Audio/l16 | pcm |
| Audio/mp3 | mp3 |
| Audio/mpeg | mpeg |
| Audio/mulaw | ulaw |
| Audio/ogg | opus |
| Audio/wav | wav |
| Audio/webm | webm |



**VIDEO FORMATS**

| **Video Format** | **Extension** | **Min. Required channels** |
| --- | --- | --- |
| Material exchange format | mxf | 2 |
| Quicktime file format | mov | 1 |
| MPEG-4 | mp4 | 1 |
| Windows media file | wmv | 1 |
| Flash video format | flv | 1 |
| Audio video interleave | avi | 1 |
| Shockwave flash | swf | 1 |
| Apple MPEG-4 | m4v | 1 |

**NOTE:** All video formats have a required minimum number of audio channels that the media file must have in order to be processed. This allows source separation to occur for multiple speakers.

**Transcribing existing conversations**

Gailbot allows users to transcribe all video / audio files that are supported while allowing a large level of customization. This allows users to tweak custom thresholds for different statistical models, CHAT files, and machine learning algorithms using a user-friendly command line interface.

A simple breakdown of the process is provided below. A more-detailed technical description is provided later on.

**STEP 1** : Select all files.

There are multiple ways to add supported files to Gailbot:
- **NOTE: Exclude square brackets when entering names in Gailbot**

- Add files as an individual file using the name of the file.
  - Example: [Sample.mp3]

- Add two pair files part of the same conversation using the **&#39;-pair&#39;** Note that this flag can be used multiple times in the same instance.
   - **NOTE:** Pair files are files that are conversations recorded with separate audio channels for each speaker.
   - '-pair [file-1 Name] [file-2 Name]'
 
- Add multiple files as separate files by placing them in a unique directory and using the **&#39;-dir&#39;** flag as follows:
  - '-dir [Directory Name]'

- To transcribe a directory with sub-directories containing two files part of the same conversation, use the **'-dirPair'** flag as follows:
  - '-dirPair [Directory Name]'
  - Note that the input directory must be organized as follows:
  	- Have sub-directories
  	- The sub-directories must contain **only** two files that are individual files part of the same conversation.


**STEP 2:** Select post-processing modules to be applied

- Gailbot allows the user to select the individual post-processing modules that will be applied to all files added as part of a single instance.
- By default, all post-processing modules are selected.
- This feature enables isolated testing of phenomenon that may be identified using only a single module or a specific combination of modules. Additionally, it allows user to a straightforward way to add custom developed post-processing modules to Gailbot.
- **NOTE:** Gailbot automatically calls customization menus for all post-processing modules added after the selection process.

**STEP 3:** Customize post-processing modules

- Once all post-processing modules have been selected, the user can customize these modules using their in-built and separate customization menus.
- **NOTE:** The user will only be able to customize modules that are selected to be applied.

**STEP 4:** Finalize and transcribe

- With all files added, post-processing modules selected, and their custom values defined, the user can finalize and send their transcription request to [Watson&#39;s STT API](https://cloud.ibm.com/apidocs/speech-to-text).
- All added files, their directories, content type, speaker names, and number of speakers are displayed here.
- **NOTE:** Files that have the same output directory are part of a **file pair.**
- Here, the user can add [**custom language and acoustic models**](https://cloud.ibm.com/docs/services/speech-to-text?topic=speech-to-text-languageCreate).
- The user can choose to opt out of Watson&#39;s default [data recording mechanism](https://cloud.ibm.com/docs/services/speech-to-text?topic=speech-to-text-summary#summary-x-watson-authorization-token) that allows IBM&#39;s Watson to improve its own transcription model. By default, the user has **opted out** of this service.
- Additionally, the user can select the method to interact with Watson: [Access or Watson authentication tokens](https://cloud.ibm.com/docs/services/speech-to-text?topic=speech-to-text-summary#summary-watson-token). Note that this feature does not alter the functionality of Gailbot in any way.
- Finally, the user can change the weight attributed to the custom model if used in the transcription process.

**Recording and Transcribing new conversations**

Gailbot allows users to record and transcribe new conversations on the go.

This allows users to test out Gailbot capabilities or added custom models for conversations that they are currently having and reduced the need to have a high-quality audio recording system for corpus generation.

Gailbot is able to achieve this using the Speech-to-text service&#39;s dialogue model.

The process for using this feature is the same as the process for transcribing an existing conversation defined above. However, an additional recording menu is presented that allows the user to specify different features of the recorded conversation including bit rate, channels etc.

**NOTE:** The recorded conversation is saved in the current directory as **&#39;Recorded.wav&#39;**.

**Re-applying post-processing modules**

In some cases, the user may have already run a media file through Gailbot but have the need to reproduce the transcript after modifying certain post-processing variables. This feature allows quick reproduction of files by only applying post-processing modules without sending requests to the Watson transcription service.

**NOTE:** This feature requires the original unchanged result directory produced by Gailbot.

This following is a list of files needed in the generated directory to use this feature:

| **File** | **Status** |
| --- | --- |
| Individual audio file | **REQUIRED** |
| Combined audio file | **REQUIRED** |
| JSON files | **REQUIRED** |
| CSV files | **NOT REQUIRED** |
| CHAT / CA files | **NOT REQUIRED** |



**\*\*NOTE:** In order to use this feature with **pair files** that are part of the same conversation, the user has to go through the process of adding files **twice** , once for each of the individual file part of the conversation. This is because Gailbot uses directory name to identify files that are part of the same conversation.

**Narrowband and Broadband files**

A characteristic of all media files is the **bit/sample-rate** i.e. the number of bits processed per unit of time during the file generation process.

By default, Gailbot uses one of a selection of [base speech models](https://cloud.ibm.com/docs/services/speech-to-text?topic=speech-to-text-models#modelsList)to transcribe media files. These base models are classified as **narrowband** or **broadband** depending on the sampling rate of the audio file:

- **Narrowband models**
  - Gailbot models used for audio files sampled at 8KHz, typically used for phone calls.
- **Broadband models**
  - Gailbot models used for audio files sampled at 16KHz or greater, more usual for high quality video and audio recordings.

Note that in order for Gailbot to function, users must select the correct **type** of base model for the files being processed. Gailbot runs checks on media files and will only send data to the Speech-to-text API if the sampling rate of the file matches the narrowband or broadband base model selected.

**\*\*NOTE:** Users can check the bit rate of a media file by right clicking on it and viewing its properties.

Base models can be modified using the **Pre-request menu**.

**\*\*NOTE:** The base model in this case is the base model for both the **custom language model** and the **custom acoustic model** (defined below) **.** A custom acoustic model must be trained in cases where it does not exist for a specific base model.

**Configuration file**

Gailbot is designed to be a highly flexible tool for use in different environments.

As such, users can modify a 'config.yaml' file containing different variables used by Gailbot permanently. In effect, this will allow users to change default settings permanenlty instead of having to modify them each time Gailbot is used.

**\*\*NOTE:** It is the user's responsiblity to set the correct 'type' i.e. integer, string etc. of configuration variables. A failure to do so may result in Gailbot not performing as expected or not running at all.

## Special Features and Post-processing

Gailbot&#39;s post-processing modules take the verbatim transcript data produced by the Speech-to-text API and apply user-defined heuristics, statistical models, machine learning models and neural networks to add a range of structural features of conversation to the final transcription files. This extensible set of features currently includes turn-taking, silences, laughter, speech rate, and overlaps.

Gailbot&#39;s post-processing interface is designed to provide a user-friendly command line interface to manage all post-processing modules and is scalable to allow users to add custom modules.

Additionally, Gailbot provides a number of special features that enhance user-experience and provide additional functionality.

**Custom language and acoustic models**

As described in the &#39;Narrowband and Broadband files&#39; section, Gailbot uses different base models to process transcription requests. However, users can train and use [custom language and acoustic models](https://cloud.ibm.com/docs/services/speech-to-text?topic=speech-to-text-languageCreate) to enhance Gailbot&#39;s transcription capabilities on top of the base transcription model.

The custom models provide the following functionality:

| **Functionality** | **Details** | **Language Model** | **Acoustic Model** |
| --- | --- | --- | --- |
| Enhances transcription quality. | Extracts and adds new custom words to existing dictionary. | **YES** | **NO** |
| Enhances transcription quality | Filters background audio based on training data | **NO** | **YES** |
| Uses base model | Trained on top of a base model | **YES** | **YES** |
| Re-trainable | Existing models can be retrained using new data | **YES** | **YES** |
| Resettable | Can be reset to remove all custom data | **YES** | **YES** |
| Upgradable | Base model can be upgraded after creation | **YES** | **YES** |
| Lists dictionary words | Shows all words learned using training data | **YES** | **NO** |
| Lists audio sources | Shows audio files used to train model | **NO** | **YES** |
| Adjustable weight | Weight applied to custom model vs. base model is adjustable | **YES** | **NO** |
| Required | Necessary to use custom model to process request | **NO** | **NO** |
| Model type same as content type | The model type (broadband or narrowband) must be the same as the content | **YES** | **YES** |



**TRAINING**

| **Custom Model** | **Training Details** |
| --- | --- |
| Language Model | Trained using a [custom corpus file](https://cloud.ibm.com/docs/services/speech-to-text?topic=speech-to-text-languageCreate#addCorpus) or using [individual words](https://cloud.ibm.com/docs/services/speech-to-text?topic=speech-to-text-languageCreate#addWords). |
| Acoustic Model | Trained using a [&#39;.wav&#39; audio file](https://cloud.ibm.com/docs/services/speech-to-text?topic=speech-to-text-manageAudio#listAudio) that is longer than 10 mins and less than 20 hours in length. | 

**Multi-Language Transcription**

By default, Gailbot uses the &#39;en-US-Broadband&#39; base transcription model to process requests.

In addition to this base model, Gailbot provides base models for different languages defined below.

Users can change the base model by changing the base model defined in the **custom language model interface.**

| **Model Name** | **Language** |
| --- | --- |
| Pt-Br-NarrowbandModel / pt-Br-BroadbandModel | Portuguese |
| Ko-Kr-NarrowbandModel / ko-Kr-BroadbandModel | Korean |
| fr-FR-NarrowbandModel / fr-FR-BroadbandModel | French |
| En-US-NarrowbandModel / en-US-BroadbandModel | US English |
| Zh-CN-NarrowbandModel / zh-CN-BroadbandModel | Mandarin |
| Ja-JP-NarrowbandModel / ja-JP-BroadbandModel | Japanese |
| En-GB-NarrowbandModel / en-GB-BroadbandModel | British English |
| En-ES-NarrowbandModel / en-ES-BroadbandModel | Spanish |
| Ar-AR-NarrowbandModel / ar-AR-BroadbandModel | Arabic |
| De-DE-NarrowbandModel / de-DE-BroadbandModel | German |



**\*\*NOTE:** Some post-processing modules may not function or function improperly when used with languages other than English.

**Speech Rate Detection**

Gailbot uses a statistical model developed at the [Tufts Human Interaction Lab](https://sites.tufts.edu/hilab/) that classifies a[turn construction unit (TCU)](https://en.wikipedia.org/wiki/Turn_construction_unit) as either a fast turn TCU or slow turnTCU depending on the average speech rate of each individual speaker.

This allows Gailbot to mark sound stretches, as well as areas of faster and slower-than-surrounding speech using markers defined as part of the CHAT and [CAlite format](https://github.com/saulalbert/CABNC/wiki/CHAT-CA-lite)s.

Below is a brief overview of the speech rate detection model:

- The speech rate is the number of syllables in a TCU divided by the total time of the TCU.
- The number of syllables is calculated using a syllable dictionary for known and machine learning algorithm for unknown words.
- The median syllable rate for each speaker is separately calculated.
- The [median absolute deviation](https://en.wikipedia.org/wiki/Median_absolute_deviation) is calculated for each speaker.
- Any TCU having a syllable rate greater than Median + (2 \* Median absolute deviation) is classified as a fast TCU while any TCU having a syllable rate less than Median – (2 \* Median absolute deviation) is classified as a slow TCU.

The model can be visualized for a sample conversation as follows:


 
**Laughter Analysis**

Gailbot uses a [Tensorflow](https://www.tensorflow.org/) deep neural network and the[librosa](https://librosa.github.io/librosa/tutorial.html#overview)audio processing library to detect and classify laughter in its transcripts.

Research has shown that laughterLaughter in natural conversation occurs at high frequencies greater higher than the average frequency of sounds in the audio signal. As such, the modelWe therefore use uses librosa to create a [Butterworth low-pass filter](https://en.wikipedia.org/wiki/Butterworth_filter) to hat removes lower frequency sounds from the audio signal, which it treats as a time series..

Next, the Gailbot then usesmodel uses a Tensorflow model to identify parts of the audio signal that contain laughter and returns the start and end time for each instance that it identifies.

This instance is then marked in the Gailbot transcript.

Gailbot uses a [Tensorflow](https://www.tensorflow.org/) deep neural network and the[librosa](https://librosa.github.io/librosa/tutorial.html#overview)audio processing library to detect and classify laughter in its transcripts. The Tensorflow model is trained to detect laughter using supervised learning of laughter instances extracted from the [switchboard corpus](https://catalog.ldc.upenn.edu/LDC97S62), and is inspired by [JRGillick&#39;s laughter detection module](https://github.com/jrgillick/laughter-detection) (Ryokai et al. 2018).

An instance of detected laughter is added to the Gailbot transcript as follows:

 
**\*\*NOTE:** This module can be attributed to [JRGillick&#39;s laughter detection library](https://github.com/jrgillick/laughter-detection).


**Beat and absolute timing**

CA transcription as described by [Hepburn &amp; Bolden (2012)](https://onlinelibrary.wiley.com/doi/10.1002/9781118325001.ch4) uses two methods of measuring silences: absolute time and &#39;beat time&#39;.

- **Absolute time:** measures the clock time between the end of one utterance and the start of the next in tenths of a second.
- **Beat time:** Relative measurements of the timings of gaps and pauses relative to the surrounding speech tempo, since it influences the perception of the length of silences in conversation.

The user can choose whether Gailbot transcribes absolute or &#39;beat&#39; timings:

- Absolute mode: All pauses and gaps are transcribed in absolute time.
- Beat mode: All pauses and gaps are transcribed in &#39;beats&#39;, based on a model that emulates [Jefferson&#39;s (1988)](http://liso-archives.liso.ucsb.edu/Jefferson/standard_maximum_silence.pdf) method of counting beats based on the tempo of surrounding speech.

**NOTE:** The type of Gailbot mode is added as a comment at the start of the transcript.

The model used to calculate beat timing was developed at the Tufts Human Interaction Lab and is summarized as follows:

- The syllable rate is the number of syllables in a TCU divided by the total time of the TCU.
- The median syllable rate is found from the syllable rate for all turns.
- The beat time is the pause or gap time in seconds multiplied by the median syllable rate per second.
- The beat time in seconds is the above value divided by 4.
- The beat time in seconds is added to the transcript.


## Liability Notice

**Gailbot is a tool to be used to generate specialized transcripts. However, it is not responsible for the quality of any output produced. Generated transcripts are meant to be a first pass in the transcription process and are designed to be improved incrementally. They are not meant to replace the manual transcription process and can be improved upon. Gailbot uses IBM Watson&#39;s Speech to Text API to generate text which required an IBM Bluemix account. The development team is not liable for any third-party transaction between the user and any external service used by Gailbot.**

**Additionally, the development team does not guarantee the accuracy or correctness of any statistical model used in Gailbot. These models have been developed in good faith and we hope that they are accurate. Users should always verify results.**

**We are committed to continue and develop Gailbot. Any feedback can be provided at:** [**hilab-dev@elist.tufts.edu**](mailto:hilab-dev@elist.tufts.edu) **.**

**However, we do not guarantee any response to emails/requests sent to us. We do not guarantee any bug-fixes or future updates.**

**Gailbot is an open-source product. We encourage its use in research. However, by using Gailbot, users agree to cite Gailbot and the Tufts Human Interaction Lab in any publications or results found as a direct or indirect result of using Gailbot.**
