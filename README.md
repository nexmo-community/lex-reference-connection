# Lex reference connection code

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/nexmo-community/lex-reference-connection)

You can use the Lex reference connection code to connect a Vonage Voice API call to an Amazon Lex bot and then have an audio conversation with the bot. Voice transcripts and sentiment analysis are posted back to the Vonage Voice API application.

## About this reference connection code

Lex reference connection makes use of the [websockets feature](https://docs.nexmo.com/voice/voice-api/websockets) of Vonage Voice API. When a voice call is established, a Voice API application triggers a websocket connection to this Lex reference connection and streams the audio to and from the voice call in real time.

Lex reference connection then takes care of capturing chunks of speech using Voice Activity Detection then post them to the Amazon Lex Endpoint. When Lex returns audio, Lex reference connection code streams that back over the websocket to the voice call.

Lex reference connection does not store any Lex specific configuration or credentials: these are supplied in the [NCCO (Nexmo Call Control Oject)](https://developer.nexmo.com/voice/voice-api/ncco-reference#websocket-endpoint) from the Voice API application, telling the Voice API to connect the call to this Lex reference connection. This is a standard `connect` function used to connect calls to websockets, with a few specific parameters to connect to Lex.

See https://github.com/nexmo-community/lex-sample-voice-application for a **sample Voice API application** using this Lex reference connection code to connect voice calls to an Amazon Lex bot.

## Transcripts

This reference connection code will send caller's transcript (labeled as customer) and Lex bot's transcript to the Voice API application via a webhook call.

## Sentiment analysis

You may enable sentiment analysis by going to AWS console, Amazon Lex, your Lex bot, then Settings tab, General, Sentiment analysis set to Yes.

This reference connection code will send caller's sentiment analysis to the Voice API application via the same webhook call as mentioned above.

## Running Lex reference connection code

You deploy the Lex reference connection code in one of the following three ways.

### Docker

Start by copying the `.env.example` file over to a new file called `.env`:

```bash
cp .env.example > .env
```

Edit `.env` file, set the `PORT` value (e.g. *5000*) where websockets connections will be established.
Its value needs to be the same as specified in `Dockerfile` and `docker-compose.yml` files.

You can then launch the Lex reference connection code as a Docker instance by running:

```bash
docker-compose up
```

You need to know your docker container instance public hostname and port to set the argument for the parameter **LEX_REFERENCE_CONNECTION** in your **sample Voice API application** in order to connect Voice API calls to this Lex reference connection code, e.g. *myserver.mycompany.com:40000*, or *xxxxx.ngrok.io*.

### Heroku

You can deploy this application to Heroku in a single click using the 'Deploy to Heroku' button at the top of this page.

Once the app is deployed, make a note of the application path in the URL (e.g. *myappname.herokuapp.com* without the leading https://) as this is the argument you will need to set for the parameter **LEX_REFERENCE_CONNECTION** in your **sample Voice API application** in order to connect voice API calls to this Lex reference connection code.

### Locally

To run your own instance locally you'll need an up-to-date version of Python 3 (we have tested with version 3.8.5).

Install dependencies with:

```bash
pip install --upgrade -r requirements.txt
```

Copy the `.env.example` file over to a new file called `.env`.

```bash
cp .env.example > .env
```
Edit `.env` file, set the `PORT` value where websockets connections will be established.

This Lex reference connection code is a websocket based service, so make sure your proxy server (e.g. Nginx, ngrok) is configured to handle websocket connections to that PORT if you have one.

If you plan to test `Locally` with ngrok for both this **Lex reference connection code** and the **sample Voice API application**, you may set up [multiple ngrok tunnels](https://ngrok.com/docs#multiple-tunnels).

Use the **Lex reference connection code** host server name and port (e.g. *xxxx.ngrok.io*, or *myserver.mycompany.com:40000*) as this is the argument you will need to set for the parameter **LEX_REFERENCE_CONNECTION** in your **sample Voice API application** in order to connect voice API calls to this Lex reference connection code.

Start the reference connection code:

```bash
python server.py
```
