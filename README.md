# Autohook 🎣

Autohook configures and manages [Twitter webhooks](https://developer.twitter.com/en/docs/accounts-and-users/subscribe-account-activity/guides/managing-webhooks-and-subscriptions) for you. Zero configuration. Just run and go!

* 🚀 Spawns a server for you
* ⚙️ Registers a webhook (it removes existing webhooks if you want, and you can add more than one webhook if your Premium subscription supports it)
* ✅ Performs the CRC validation when needed
* 📝 Subscribes to your current user's context (you can always subscribe more users if you need)
* 🎧 Exposes a listener so you can pick up Account Activity events and process the ones you care about

## Usage

You can use Autohook as a module or as a command-line tool.

### Node.js module

```js
const { Autohook } = require('twitter-autohook');

(async ƛ => {
  const webhook = new Autohook();
  
  // Removes any existing webhook
  await webhook.removeWebhooks();
  
  // Listens to incoming activity
  webhook.on('event', event => console.log('Something happened:', event);
  
  // Starts a server and adds a new webhook
  webhook.start();
  
  // Subscibes to a user's activity
  webhook.subscribe({oauth_token, oauth_token_secret});
})();
```

### Command line

Starting Autohook from the comman line is useful when you need to test your connection and subscription.

When started from the command line, Autohook simply provisions a webhook. It echoes any incoming event to `stdout` only after you [subscribe your app](https://developer.twitter.com/en/docs/accounts-and-users/subscribe-account-activity/quick-start/enterprise-account-activity-api) to listen to the activity for one or more accounts. 

```bash
# Starts a server, removes any existing webhook, adds a new webhook, and subscribes to the authenticating user's activity.
$ autohook -rs

# All the options
$ autohook --help
```

## OAuth

Autohook works only when pass your OAuth credentials. You won't have to figure out OAuth by yourself – Autohook will work that out for you.

You can pass your OAuth credentials in a bunch of ways.

## Dotenv (~/.env.twitter)

Create a file named `~/.env.twitter` (sits in your home dir) with the following variables:

```bash
TWITTER_CONSUMER_KEY= # https://developer.twitter.com/en/apps ➡️ Your app ID ➡️ Details ➡️ API key
TWITTER_CONSUMER_SECRET= # https://developer.twitter.com/en/apps ➡️ Your app ID ➡️ Details ➡️ API secret key
TWITTER_ACCESS_TOKEN= # https://developer.twitter.com/en/apps ➡️ Your app ID ➡️ Details ➡️ Access token
TWITTER_ACCESS_TOKEN_SECRET= # https://developer.twitter.com/en/apps ➡️ Your app ID ➡️ Details ➡️ Access token secret
TWITTER_WEBHOOK_ENV= # https://developer.twitter.com/en/account/environments ➡️ One of 'Dev environment label' or 'Prod environment label'
```

Autohook will pick up these details automatically, so you won't have to specify anytihing in code or via CLI.

## Env variables

Useful when you're deploying to remote servers, and can be used in conjunction with your dotenv file.

```bash

# To your current environment
export TWITTER_CONSUMER_KEY= # https://developer.twitter.com/en/apps ➡️ Your app ID ➡️ Details ➡️ API key
export TWITTER_CONSUMER_SECRET= # https://developer.twitter.com/en/apps ➡️ Your app ID ➡️ Details ➡️ API secret key
export TWITTER_ACCESS_TOKEN= # https://developer.twitter.com/en/apps ➡️ Your app ID ➡️ Details ➡️ Access token
export TWITTER_ACCESS_TOKEN_SECRET= # https://developer.twitter.com/en/apps ➡️ Your app ID ➡️ Details ➡️ Access token secret
export TWITTER_WEBHOOK_ENV= # https://developer.twitter.com/en/account/environments ➡️ One of 'Dev environment label' or 'Prod environment label'

# To other services, e.g. Heroku
heroku config:set TWITTER_CONSUMER_KEY=value TWITTER_CONSUMER_SECRET=value TWITTER_ACCESS_TOKEN=value TWITTER_ACCESS_TOKEN_SECRET=value TWITTER_WEBHOOK_ENV=value
```
## Directly

Not recommended, because you should always [secure your credentials](https://developer.twitter.com/en/docs/basics/authentication/guides/securing-keys-and-tokens.html).

### Node.js

```js
new Autohook({
  token: 'value',
  token_secret: 'value',
  consumer_key: 'value',
  consumer_secret: 'value',
  env: 'env',
  port: 1337
});
```

### CLI

```bash
$ autohook \
  --token $TWITTER_ACCESS_TOKEN \
  --secret $TWITTER_ACCESS_TOKEN_SECRET \
  --consumer-key $TWITTER_CONSUMER_KEY \
  --consumer-sercret $TWITTER_CONSUMER_SECRET \
  --env $TWITTER_WEBHOOK_ENV
```

## Install

```bash
# npm
$ npm i -g twitter-autohook

# Yarn
$ yarn global add twitter-autohook
```
