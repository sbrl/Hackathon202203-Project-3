# Sustainability Hackathon Project 3 Example Code

> Example code for project 3 in the [March 2022 Hackathon in AI for Sustainability](https://bda-hull.github.io/hackathon-ai+sus.html).

Welcome to project 3 of the [March 2022 hackathon in AI for sustainability](https://bda-hull.github.io/hackathon-ai+sus.html)! This repository contains some sample code to get you started.

The canonical URL for this lab sheet is: https://github.com/sbrl/Hackathon202203-Project-3#readme

Please visit this link for the most up-to-date version of this lab sheet.

TODO: If we end up moving this repository, don't forget to update *all* the URL references below, above, and on the dataset download server!

## System Requirements
 - Python 3.7+, pip (usually bundled automatically. Linux users may want to run `sudo apt install python3-pip`)
 - Nvidia GPU (optional; Linux users must use the propriety Nvidia driver to get CUDA support)
 - 2 GiB free disk space (more if you want to download images)
     - A USB flash drive would also work here


## Getting Started

### Step 1: Download the code
To start, we need to download the sample code. We will do this by creating a new repository.

First, visit this url in your browser: <https://github.com/sbrl/Hackathon202203-Project-3>.

Then, after ensuring you're signed into GitHub, click the green "Use this template" button in the top right:

![](https://i.imgur.com/Jl5ax2Y.png)

You should see the following web page:

![](https://i.imgur.com/HIHim3o.png)

Fill in the repository name (and optionally the description), and then click "Create repository from template".

From here, you will need to clone your new repository. The following guide is great to help you do this: <https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository>


### Step 2: Download the dataset and model
The example code does not include the pretrained model or datasets, as these are larger files that can't be hosted in a git repository on GitHub.

Instead, these are hosted on-campus for faster downloads. We'll need to download a pre-trained model and some tweets to work with.

**Important:** You must be on the University's network to access these links! Any of the following will work:

 - Using a Lab PC on-campus
 - Using your own device on-campus connected to eduroam (UoH-Guest will NOT work)
 - Using the University's VPN

To get started, you will need 2 things:

1. A pretrained model
2. A data to make predictions with

Download the pretrained model from this link: <http://hackathon2022.mooncarrot.space/aimodel/transformer-g25.zip> [102.2MiB, 247MiB extracted, 78.6% accurate]

Download the StormFranklin dataset from this link: <http://hackathon2022.mooncarrot.space/dataset-StormFranklin.zip> [4.7MiB, 18MiB extracted]

For those who are adventurous, a full list of datasets and pre-trained models available, visit the following page: <http://hackathon2022.mooncarrot.space/README.html>

This includes:

 - Many more tweets
 - Images associated with tweets [TODO Download StormFranklin images]
 - Larger and slightly more accurate pretrained models

Downloading these extra resources is **not required** to follow this guide.

**Important:** The data provided here must **only** be used for the hackathon. If you'd like to use it for something else, then you will need to [apply for your own Twitter developer account](https://developer.twitter.com/en/portal/products) to avoid breaking their terms of service.


#### Dataset files
Once you've downloaded and extracted the dataset files, you should have the following files:

 - `tweets.jsonl`: The tweets themselves (795536 in total)
 - `users.jsonl`: The users who made the tweets (597266 in total)
 - `places.jsonl`: The places mentioned in the tweets - not necessarily where they were made (7353 in total)

Although the content of these files is different, they are formatted the same. They are plain text files, with 1 [JSON](https://json.org/) object on each line (for those interested in the specifics, this is the [JSON Lines](https://jsonlines.org/) format).

A tweet object might look like this (pretty-printed for your convenience):

```json
{
	"author_id": "XypeYhmi-BRGfuUOAznzsg",
	"text": "#Floods Hit York's #Jorvik #Viking Centre\\n#Centrequot #Yorks\\n<https://t.co/bPalntkb85> <https://t.co/DVJb2xs5GI" > ,
	"id": "-Ey1Sx_7GFhE2NfcREhILA",
	"entities": {
		"hashtags": [
			{ "start": 0, "end": 7, "tag": "Floods" },
			{ "start": 19, "end": 26, "tag": "Jorvik" },
			{ "start": 27, "end": 34, "tag": "Viking" },
			{ "start": 42, "end": 53, "tag": "Centrequot" },
			{ "start": 54, "end": 60, "tag": "Yorks" }
		],
		"annotations": [{ "start": 8 "end": 15,
			"probability": 0.3982,
			"type": "Place",
			"normalized_text": "Hit York"
		}],
		"urls": [
			{ "start": 61 "end": 84,
				"url": "https://t.co/bPalntkb85",
				"expanded_url": "http://wp.me/p67m4w-q2i",
				"display_url": "wp.me/p67m4w-q2i"
			},
			{ "start": 85 "end": 108,
				"url": "https://t.co/DVJb2xs5GI",
				"expanded_url": "https://twitter.com/newsinvideos/status/693030697062178816/photo/1",
				"display_url": "pic.twitter.com/DVJb2xs5GI"
			}
		]
	},
	"created_at": "2016-01-29T11:19:00.000Z",
	"public_metrics": {
		"retweet_count": 0,
		"reply_count": 0,
		"like_count": 0,
		"quote_count": 0
	},
	"conversation_id": "-Ey1Sx_7GFhE2NfcREhILA",
	"attachments": {},
	"lang": "en",
	"source": "NewsVideos",
	"media": [{
		"media_key": "N2ytVkhwmVGUM6wTz-Nz3Q",
		"type": "photo",
		"url": "https://pbs.twimg.com/media/CZ4jvziUYAAlTg7.jpg"
	}]
}
```

A user object might look like this:

```json
{
	"username": "p138ioZMq7BYWb2960oKMQ",
	"id": "yvaksbHNMfNv6oT5m0ZV4g",
	"location": "Johannesburg",
	"public_metrics": {
		"followers_count": 505,
		"following_count": 743,
		"tweet_count": 3141,
		"listed_count": 3
	}
}
```

A place object might look like this:

```json
{
	"place_type": "neighborhood",
	"id": "5cda6ef12a51f99a",
	"country": "Australia",
	"full_name": "Carrs Island, Grafton",
	"geo": {
		"type": "Feature",
		"bbox": [
			152.90282916,
			-29.68268499,
			152.92051416,
			-29.66376303
		],
		"properties": {}
	}
}
```


### Step 3: Install dependencies
Open your command prompt or terminal (Windows users: try [this tutorial](https://helpdeskgeek.com/how-to/open-command-prompt-folder-windows-explorer/) if you're unsure).

Then, **Windows users** run this command:

```bash
python -m pip install -r requirements.txt
```

If you are a **Linux user**, run this command instead:

```bash
sudo pip3 install -r requirements.txt
```

## Step 4: Making predictions
Now that we have our dependencies installed, we can look at running the codebase. Let's take a quick look at the files you'll see when you first look at the example code.

### Step 4.1: Code layout
There are several Python files here, but don't worry! Most of them you can ignore. Here's a quick overview as to what they do:

 - `make_predictions.py`: The main sample code! This shows you how to load the pre-trained model you downloaded above and maek a prediction with it.
 - `cats.py`: Some helper functions for converting category indexes that the AI model understands into their respective category names.
 - `requirements.txt`: Contains a list of Python pip packages that you'll need to install. See above for how to install these.
 - `glove/`: Ignore this. Contains functions for working with [GloVe](https://nlp.stanford.edu/projects/glove/), which is used as an embedding layer by the AI model.
 - `ai_layers/`: Definitely ignore this (it's significantly complicated)! This is the internal layer definitions required by the model in order to work.


### Step 4.2: Filepaths
Before we can run the code, we need to update the filepaths in `make_predictions.py` so that they point to the right place. Find the lines that look like this:

```python
# Change these filepaths to match your own filesystem.
# Note in particular the "g25" number, which may also be "g50", "g100", or "g200"
# depending on which model you are using.
# WARNING: The "g200" model requires at least 20GB of RAM!
filepath_checkpoint = "transformer-g25/transformer_checkpoint_g25.hdf5"
filepath_glove = "glove.twitter.27B.25d.txt"

filepath_tweets = "dataset/tweets.jsonl"
```

....and edit them to be paths to the right files.

 - `filepath_checkpoint` needs to point to the `.hdf5` checkpoint file you downloaded.
 - `filepath_glove` needs to point to the file that sits next to the `.hdf5` file that is named something like `glove.twitter.27B.25d.txt` for example.
 - `filepath_tweets` needs to point tot he file containing the tweets you want to make predictions for, which in the above sample datasets is called `tweets.jsonl`.

These filepaths can be relative to `make_predictions.py`.


### Step 4.3: Making predictions
Now we're ready to make some predictions! To make some predictions, we need to run the `make_predictions.py` Python script. This is easy to do, but differs slightly depending on your operating system.

**Windows users:** Navigate to the folder that contains `make_predictions.py` in Windows File Explorer. Then, click in the address bar, type `cmd`, and then hit enter to open your command prompt.

**macOS users:** Open a terminal, and then use the `ls` command to list the contents of the current directory. Using this information, you should be able to use the `cd` command to change directory into the directory that contains `make_predictions.py`. if you have your directory structure nested, it make take several `cd` commands to find it.

**Linux users:** Navigate to the location of `make_predictions.py` in your file manager, and then right click on the background → click "Open in Terminal". If this option doesn't appear for you, follow the instructions for macOS useers above.

Once you have your terminal or command prompt option, run the following command:

```bash
python3 make_predictions.py
```

It might take a minute to run. Once it's complete, you should see something like this:

```tsv
item_number	positive	negative
0	0.9116541147232056	0.08834586292505264
1	0.07721546292304993	0.9227845668792725
2	0.5343278050422668	0.46567219495773315
3	0.8913617134094238	0.10863829404115677
4	0.9576172232627869	0.04238273575901985
5	0.32121869921684265	0.6787812113761902
6	0.19379104673862457	0.8062089085578918
7	0.11957920342683792	0.8804208040237427
8	0.16673220694065094	0.8332678079605103
9	0.866070568561554	0.13392946124076843
```


### Step 5: What comes next
Thanks for following this guide! If you need any help or have any questions, we're happy to assist in the lab :-)

See also the *Frequently Asked Questions* section below for some common questions.

There is additional data available for you to make use of should you wish to on the download server: <http://hackathon2022.mooncarrot.space/README.html>

This includes:

 - Images associated with tweets
 - More tweets



## Frequently Asked Questions

### Why are all the twitter usernames / author names gibberish?
Usernames (and a few other things) are anonymised to protect the privacy of the users who made the tweets. This is required for ethical approval, so an deanonymised dataset is unfortunately unavailable.

Despite this, it is guaranteed that if an anonymised username is mentioned multiple times, it will always be the *same* gibberish (this is because a hash function is used to anonymise the data).


### Can I have the images associated with the tweets please?
Sure! You can download them from here: <http://hackathon2022.mooncarrot.space/README.html>. Depending on which one you pick, the download will be **very big** however (the largest one is ~19 GiB when extracted, 168952 images), so do be prepared for that! Windows users will need [7-Zip](https://7-zip.org) to extract it, as it's packed into a `.tar.xz` rather than a `.zip` to increase compression.


### How do the users and tweets relate to one another?
The `author_id` property on the tweet will match the `id` field of the user. Note that all these fields are anonymised as to protect users' privacy.


## Footnote
If you've read this far, thanks so much! I hope the hackathon has been a fun experience and you've learnt something.

For those curious, this project is based on my PhD project, which has the title "Using Big Data and AI to dynamically predict flood risk". I'm part of both the [Big data Analytics](https://bda-hull.github.io/) research group in the Department of Computer Science and Technology and the Energy and Environment Institute.

For those interested in what I do, I have [a blog](https://starbeamrainbowlabs.com/) on which I talk about my project and other things that interest me.

I'd like to thank Nina Dethlefs, my supervisor - without her tireless dedication and support I would not have been able to get as far as I have with my PhD project.
